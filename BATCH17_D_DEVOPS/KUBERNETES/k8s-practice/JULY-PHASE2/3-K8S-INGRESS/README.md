# Voting App on EKS — AWS Load Balancer Controller Ingress

Docker Samples voting app (`vote` → `redis` → `worker` → `postgres` → `result`) deployed on
Amazon EKS, exposed through a single ALB via the AWS Load Balancer Ingress Controller.

> **Cost warning:** this creates real AWS resources — an EKS cluster (~$0.10/hr control plane),
> EC2 worker nodes, and an Application Load Balancer. The node group uses `m7i-flex.large`
> (2 vCPU / 8 GiB, 29 pods/node), chosen because it's the free-tier-eligible instance type for
> this account with enough pod capacity to run the whole app — check `aws ec2 describe-instance-types
> --filters "Name=free-tier-eligible,Values=true"` if setting this up on a different account, since
> the free-tier-eligible list varies per account. Run the [Cleanup](#cleanup) section when you're
> done to avoid ongoing charges on the non-free-tier pieces (control plane, ALB).

## Architecture

```
Internet → ALB (AWS LB Controller) → Ingress → vote-service / result-service (ClusterIP)
                                                        ↓                ↑
                                                      redis          postgres
                                                        ↓                ↑
                                                          worker ────────┘
```

## Prerequisites — install tools (Windows / git-bash)

Using `winget` (built into Windows 10/11, no Chocolatey required) — run these from PowerShell or
cmd, since `winget` itself isn't available inside git-bash:

```powershell
winget install -e --id Amazon.AWSCLI
winget install -e --id Kubernetes.kubectl
winget install -e --id Helm.Helm
winget install -e --id eksctl.eksctl
```

Close and reopen your git-bash terminal after installing so `PATH` picks up the new tools.

Verify (from git-bash):
```bash
aws --version
kubectl version --client
helm version
eksctl version
```

## Step 1 — Configure AWS credentials

```bash
aws configure
```
Enter your Access Key, Secret Key, default region (e.g. `us-east-1`), and output format (`json`).

## Step 2 — Create a Route 53 hosted zone and delegate DNS

NS delegation can take anywhere from minutes to a couple hours to propagate, so do this now,
in parallel with cluster creation (Step 3) — it doesn't depend on the cluster existing.

**2a. Create the hosted zone:**
```bash
aws route53 create-hosted-zone --name b17facebook.xyz --caller-reference "voting-app-$(date +%s)"
```
Note the `Id` in the output (looks like `/hostedzone/Z0123456789ABC`).

**2b. Get the 4 Route 53 nameservers for that zone:**
```bash
aws route53 get-hosted-zone --id <ZONE_ID> --query "DelegationSet.NameServers" --output table
```

**2c. Update nameservers at your registrar:**
Log into your domain registrar, find the nameserver/DNS settings for the domain, and replace
its default nameservers with the 4 values from Step 2b. This delegates all DNS for the domain
to Route 53.

> **Already done for `b17facebook.xyz`:** zone `Z09020593QE7ZCUI17J3` already existed (this domain
> is already in active use for other projects) and delegation is already live — confirmed via
> `nslookup -type=NS b17facebook.xyz 8.8.8.8`. No propagation wait needed. Only `vote.` and
> `result.` records will be touched; existing records (`api.`, `bookstore.`, `main.`, etc.) are
> left alone.

**2d. Verify delegation once it propagates:**
```bash
nslookup -type=NS b17facebook.xyz
```
You should see the same 4 Route 53 nameservers back.

## Step 3 — Create the EKS cluster

Cluster definition lives in [cluster.yaml](cluster.yaml) (`withOIDC: true` sets up the IAM OIDC
provider automatically — no separate step needed). Edit the `region` field if you want a region
other than `us-east-1`.

```bash
eksctl create cluster -f cluster.yaml
```

This takes ~15-20 minutes and provisions the control plane, a 2-node managed node group, and
updates your local kubeconfig automatically.

Verify:
```bash
kubectl get nodes
```

## Step 4 — Install the AWS Load Balancer Controller

**3a. Create the IAM policy** the controller needs:
```bash
curl -O https://raw.githubusercontent.com/kubernetes-sigs/aws-load-balancer-controller/v2.9.0/docs/install/iam_policy.json


aws iam create-policy --policy-name AWSLoadBalancerControllerIAMPolicy --policy-document file://iam_policy.json
```

These two commands are part of the setup for the AWS Load Balancer Controller. They create an IAM policy that gives the controller permission to create and manage AWS load balancers on your behalf.

1. downloads policy JSON file from the AWS Load Balancer Controller GitHub repository.
2. creates a new IAM policy in your AWS account using the downloaded JSON file. (customer managed policy)

Note the `Arn` returned — you'll need your AWS account ID for the next command
(`aws sts get-caller-identity --query Account --output text`).

**3b. Create the IAM role + Kubernetes ServiceAccount (IRSA):**

Next, you attach this policy to an IAM role that the Kubernetes controller will assume via IRSA (IAM Roles for Service Accounts).

```bash
eksctl create iamserviceaccount \
  --cluster=mg --region=us-east-1 \
  --namespace=kube-system --name=aws-load-balancer-controller \
  --attach-policy-arn=arn:aws:iam::<ACCOUNT_ID>:policy/AWSLoadBalancerControllerIAMPolicy \
  --override-existing-serviceaccounts --approve
```

This command creates a Kubernetes Service Account and links it to an AWS IAM Role using IRSA (IAM Roles for Service Accounts). This allows the AWS Load Balancer Controller to securely access AWS APIs without using long-lived AWS credentials.


**3c. Install the controller via Helm:**
```bash
helm repo add eks https://aws.github.io/eks-charts
helm repo update

VPC_ID=$(aws eks describe-cluster --name mg --region us-east-1 --query "cluster.resourcesVpcConfig.vpcId" --output text)

helm install aws-load-balancer-controller eks/aws-load-balancer-controller \
  -n kube-system \
  --set clusterName=mg \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller \
  --set region=us-east-1 \
  --set vpcId="$VPC_ID"
```

**3d. Verify:**
```bash
kubectl get deployment -n kube-system aws-load-balancer-controller
```
Expect `1/1` ready within a minute or two.

## Step 5 — Deploy the voting app

```bash
kubectl apply -f postgres-deployment.yml -f postgres-service.yml
kubectl apply -f redis-deployment.yml -f redis-service.yml
kubectl apply -f voting-app-deployment.yml -f voting-app-service.yml
kubectl apply -f result-app-deployment.yml -f result-app-service.yml
kubectl apply -f worker-app-deployment.yml
```

`voting-service` and `result-service` are `ClusterIP` ([voting-app-service.yml](voting-app-service.yml),
[result-app-service.yml](result-app-service.yml)) — they're only reachable inside the cluster; the
Ingress/ALB is what exposes them externally.

Verify pods come up:
```bash
kubectl get pods
```

## Step 6 — Edit and deploy the Ingress

[ingress.yml](ingress.yml) is already set up with:
```yaml
    - host: vote.b17facebook.xyz
      ...
    - host: result.b17facebook.xyz
```
Host-based rules (rather than path-based) are used because both the vote and result apps serve
static assets from `/` — a `/result` path prefix would break the result app's asset URLs, since
ALB Ingress (unlike nginx) has no rewrite-target support.

It's also set up for HTTPS: `listen-ports` opens both 80 and 443, `ssl-redirect: '443'` makes the
controller auto-redirect any HTTP request to HTTPS instead of forwarding it, and
`certificate-arn` tells the ALB which ACM cert to terminate TLS with (must be a **regional** cert
in `us-east-1`, since that's where the ALB lives, and must cover both `vote.b17facebook.xyz` and
`result.b17facebook.xyz` — either as SANs on one cert or a wildcard `*.b17facebook.xyz`). Replace
`<CERT_ARN>` with your existing certificate's ARN:

```yaml
    alb.ingress.kubernetes.io/certificate-arn: <CERT_ARN>
```

Deploy it:
```bash
kubectl apply -f ingress.yml
```

## Step 7 — Get the ALB address and create DNS records

```bash
kubectl get ingress voting-app-ingress -w
```
Wait for the `ADDRESS` column to populate (1-3 minutes) — that's the ALB's DNS name (e.g.
`k8s-default-votingap-xxxx-yyyy.us-east-1.elb.amazonaws.com`).

**Easiest: via the Route 53 console.** Route 53 → Hosted zones → `b17facebook.xyz` → Create record:
- Toggle **Alias** on.
- Record name: `vote`. Route traffic to: **Alias to Application and Classic Load Balancer** →
  your region → select the ALB from the dropdown. Save.
- Repeat with record name `result`, pointing at the same ALB.

**Or via CLI**, using the ALB's own hosted zone ID (fixed per region — for `us-east-1` it's
`Z35SXDOTRQ7X7K`; find yours in the
[ELB regional zone ID table](https://docs.aws.amazon.com/general/latest/gr/elb.html) if different):
```bash
ALB_DNS=$(kubectl get ingress voting-app-ingress -o jsonpath="{.status.loadBalancer.ingress[0].hostname}")
```
Then submit a change batch (save as `dns-change.json`, substituting `<ZONE_ID>` and `$ALB_DNS`)
for each of `vote` and `result`:
```json
{
  "Changes": [{
    "Action": "UPSERT",
    "ResourceRecordSet": {
      "Name": "vote.b17facebook.xyz",
      "Type": "A",
      "AliasTarget": {
        "HostedZoneId": "Z35SXDOTRQ7X7K",
        "DNSName": "<ALB_DNS>",
        "EvaluateTargetHealth": false
      }
    }
  }]
}
```
```bash
aws route53 change-resource-record-sets --hosted-zone-id <ZONE_ID> --change-batch file://dns-change.json
```

## Step 8 — Test

```bash
curl -I http://vote.b17facebook.xyz    # expect a 301 redirect to https
curl https://vote.b17facebook.xyz
curl https://result.b17facebook.xyz
```
Or open both hostnames in a browser — HTTP requests auto-redirect to HTTPS via the ingress's
`ssl-redirect` annotation. Cast a vote on `vote.b17facebook.xyz` and confirm the result updates on
`result.b17facebook.xyz`.

## Cleanup

To avoid ongoing charges, tear down in this order:
```bash
kubectl delete -f ingress.yml
helm uninstall aws-load-balancer-controller -n kube-system
eksctl delete cluster -f cluster.yaml
```
(Deleting the Ingress first lets the controller deregister/delete the ALB before the cluster and
node group are removed.)

The Route 53 hosted zone (~$0.50/month) is left in place since it's not part of the compute spend
and you'll likely want to keep the domain delegation. Delete it only if you're done with the
domain entirely:
```bash
aws route53 delete-hosted-zone --id <ZONE_ID>
```
