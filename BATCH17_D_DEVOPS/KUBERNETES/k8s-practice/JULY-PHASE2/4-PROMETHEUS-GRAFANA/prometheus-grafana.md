# Prometheus + Grafana Setup on Kubernetes (using Helm)

This guide covers installing Prometheus and Grafana on a Kubernetes cluster using the
`kube-prometheus-stack` Helm chart, verifying the install, and accessing both dashboards.

> Namespace used throughout this guide: **`prometheus`**
> Helm release name used throughout this guide: **`monitoring`**

---

## 1. Theory: Monitoring, Observability, Prometheus & Grafana

### 1.1 What is Monitoring?

Monitoring means watching your system and checking if known things are okay — like CPU
usage, memory, or if a pod restarted. You decide in advance what to watch, and you get an
alert when something crosses a limit (example: "CPU is above 80%").

Simple way to think about it: **Monitoring answers questions you already know to ask.**

### 1.2 What is Observability?

Observability means you can understand *why* something went wrong, even if you never
thought about that problem before. It gives you enough information to dig in and find the
root cause, not just know that something is broken.

Observability is usually made of 3 parts:

| Part | What it means in simple words | Example tool |
|---|---|---|
| Metrics | Numbers over time (CPU %, memory, request count) | Prometheus |
| Logs | Text messages your app/system writes about events | Loki / EFK |
| Traces | The path a request takes across different services | Jaeger / Tempo |

Simple way to think about it: **Monitoring tells you something is wrong. Observability
helps you find out why.**

In this guide, Prometheus + Grafana cover the **metrics** part.

### 1.3 What is Prometheus?

Prometheus is a free, open-source tool that **collects numbers (metrics) from your
systems and stores them over time.** It also has an alerting feature.

In simple words, Prometheus does 3 main things:

- **Collects data**: It goes to each target (server, pod, app) every few seconds and pulls
  its metrics. This is called "scraping."
- **Stores data**: It saves all collected numbers with a timestamp, so you can see how
  things changed over time.
- **Sends alerts**: If a value crosses a limit you set (like "disk almost full"), Prometheus
  can trigger an alert.

A few extra words you will see often:

- **Exporter**: a small helper program that exposes metrics in a format Prometheus
  understands. Example: `node-exporter` gives CPU/memory/disk info of a server.
- **PromQL**: the query language used to ask Prometheus questions about the data
  (example: "average CPU usage in the last 5 minutes").
- **Alertmanager**: a separate tool that takes alerts from Prometheus and sends them to
  Slack, email, etc.

Prometheus does not have a very good-looking dashboard by itself — that is why we use
Grafana with it.

### 1.4 What is Grafana?

Grafana is a free, open-source tool used to **show data on nice, easy-to-read dashboards
and graphs.**

Grafana does not collect or store data itself. Instead, it connects to a data source (like
Prometheus) and asks it for data, then draws that data as graphs, tables, or charts.

In simple words:

- You connect Grafana to Prometheus (as a "data source")
- You build dashboards with graphs/panels
- You can also set up alerts in Grafana

### 1.5 Prometheus vs Grafana (Simple Summary)

| | Prometheus | Grafana |
|---|---|---|
| Job | Collects and stores metrics | Shows metrics as dashboards |
| Alerts | Yes (basic) | Yes (more flexible) |
| Good at | Data collection & storage | Visualization |

Simple way to remember it: **Prometheus collects the data. Grafana shows the data.**

### 1.6 How They Work Together on Kubernetes

```
 ┌─────────────┐    scrape (pull)    ┌────────────┐
 │  Exporters / │ ───────────────────▶│ Prometheus │
 │  App /metrics│                     │  (stores   │
 └─────────────┘                     │   data)    │
                                      └─────┬──────┘
                                            │ Grafana asks for data
                                            ▼
                                     ┌────────────┐        alerts
                                     │  Grafana   │◀──┐   ┌────────────┐
                                     │ Dashboards │   └───│Alertmanager│──▶ Slack/Email/PagerDuty
                                     └────────────┘       └────────────┘
```

In simple steps:

1. Every server/node has an "exporter" that exposes metrics (CPU, memory, etc). Apps can
   also expose their own metrics.
2. Prometheus visits (scrapes) each of these every few seconds and saves the numbers.
3. Prometheus checks its alert rules. If something is wrong, it tells Alertmanager, which
   sends a notification.
4. Grafana connects to Prometheus, pulls the stored data, and shows it as dashboards so
   people can easily see cluster/app health.

The `kube-prometheus-stack` Helm chart installs everything needed for this — Prometheus,
Alertmanager, node-exporter, kube-state-metrics, and Grafana (already connected to
Prometheus) — in one go.

---

## 2. Prerequisites

- A running Kubernetes cluster (EKS/Minikube/Kind/etc.)
- `kubectl` installed and configured to talk to the cluster
- `eksctl` installed (if provisioning an EKS cluster on AWS)
- `aws` CLI configured with valid credentials (if using EKS)
- `helm` v3 installed on your machine

### 2.1 Install kubectl (Linux)

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

Verify:

```bash
kubectl version
```

### 2.2 Install eksctl (on an AWS Linux machine)

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp

sudo mv /tmp/eksctl /usr/local/bin
```

Verify:

```bash
eksctl version
```

### 2.3 Install Helm (Linux)

```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

Verify:

```bash
helm version
```

### 2.4 Configure AWS CLI

```bash
aws configure
```

Enter your `AWS Access Key ID`, `AWS Secret Access Key`, default region, and output format
when prompted.

### 2.5 Create an EKS Cluster

```bash
eksctl create cluster testcluster
```

> This provisions a new EKS cluster named `testcluster` (default: 2 `m5.large` nodes) and
> automatically updates your local `~/.kube/config` so `kubectl` points to it. Cluster
> creation typically takes 15-20 minutes.

### 2.6 Verify Cluster Access

```bash
kubectl cluster-info
kubectl get nodes
helm version
```

---

## 3. Create a Dedicated Namespace

Keep monitoring components isolated from application workloads.

```bash
kubectl create namespace prometheus
```

Verify:

```bash
kubectl get ns
```

---

## 4. Add the Prometheus Community Helm Repo

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

Verify the chart is available:

```bash
helm search repo prometheus-community/kube-prometheus-stack
```

> `kube-prometheus-stack` bundles Prometheus, Alertmanager, Grafana, node-exporter,
> kube-state-metrics, and pre-built Grafana dashboards — no need to install Grafana separately.

---

## 5. Install kube-prometheus-stack

```bash
helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace prometheus \
  --create-namespace
```

- `monitoring` (first arg) = Helm release name
- `--namespace prometheus` = target namespace
- `--create-namespace` = creates namespace if it doesn't already exist

### Optional: Install with custom values

Create a `values.yaml` to override defaults (storage size, admin password, retention, etc.):

```yaml
grafana:
  adminPassword: "Admin@123"
  service:
    type: NodePort

prometheus:
  prometheusSpec:
    retention: 10d
    storageSpec:
      volumeClaimTemplate:
        spec:
          accessModes: ["ReadWriteOnce"]
          resources:
            requests:
              storage: 10Gi

alertmanager:
  alertmanagerSpec:
    storage:
      volumeClaimTemplate:
        spec:
          accessModes: ["ReadWriteOnce"]
          resources:
            requests:
              storage: 2Gi
```

Install/upgrade with the custom values file:

```bash
helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace prometheus \
  --create-namespace \
  -f values.yaml
```

---

## 6. Verify the Installation

```bash
kubectl get pods -n prometheus
kubectl get svc -n prometheus
kubectl get pvc -n prometheus
helm list -n prometheus
```

You should see pods for:
- `monitoring-kube-prometheus-operator`
- `monitoring-kube-prometheus-prometheus` (StatefulSet, Prometheus server)
- `monitoring-grafana`
- `monitoring-kube-state-metrics`
- `monitoring-prometheus-node-exporter` (DaemonSet, one per node)
- `alertmanager-monitoring-kube-prometheus-alertmanager`

Wait until all pods show `Running` / `1/1` or `2/2` Ready.

---

## 7. Access Prometheus UI

### Option A: Port-forward (quick, local testing)

```bash
kubectl port-forward -n prometheus svc/monitoring-kube-prometheus-prometheus 9090:9090
```

Open in browser: `http://localhost:9090`

### Option B: Change service type to NodePort/LoadBalancer

```bash
kubectl edit svc monitoring-kube-prometheus-prometheus -n prometheus
```

Change `type: ClusterIP` to `type: NodePort` (or `LoadBalancer` on cloud), save, then:

```bash
kubectl get svc -n prometheus monitoring-kube-prometheus-prometheus
```

Access via `http://<node-ip>:<node-port>`.

---

## 8. Access Grafana UI

### Get the admin password (auto-generated if not set in values.yaml)

```bash
kubectl get secret -n prometheus monitoring-grafana \
  -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

Default username: `admin`

### Option A: Port-forward

```bash
kubectl port-forward -n prometheus svc/monitoring-grafana 3000:80
```

Open in browser: `http://localhost:3000`

Login with:
- Username: `admin`
- Password: (from the command above, or the one you set in `values.yaml`)

### Option B: NodePort / LoadBalancer

```bash
kubectl edit svc monitoring-grafana -n prometheus
```

Change `type: ClusterIP` to `type: NodePort` or `LoadBalancer`, then:

```bash
kubectl get svc -n prometheus monitoring-grafana
```

Access via `http://<node-ip>:<node-port>`.

---

## 9. Confirm Prometheus Data Source in Grafana

`kube-prometheus-stack` auto-configures Prometheus as a Grafana data source. To verify:

1. Login to Grafana
2. Go to **Connections → Data sources**
3. Confirm `Prometheus` is listed and shows a green "working" check when tested

---

## 10. Explore Pre-Built Dashboards

`kube-prometheus-stack` ships with ready-made dashboards. In Grafana:

1. Go to **Dashboards** (left sidebar)
2. Open folders like `Kubernetes / Compute Resources / Cluster`, `Kubernetes / Nodes`,
   `Kubernetes / Pods`, etc.
3. These are powered automatically by `node-exporter` + `kube-state-metrics` + Prometheus

### Optional: Import extra community dashboards

1. In Grafana: **Dashboards → New → Import**
2. Enter a dashboard ID from https://grafana.com/grafana/dashboards/ (e.g., `315` for Node
   Exporter Full)
3. Select the `Prometheus` data source
4. Click **Import**

---

## 11. Basic Prometheus Queries (PromQL) to Try

```promql
up
kube_pod_status_phase
node_memory_MemAvailable_bytes
rate(container_cpu_usage_seconds_total[5m])
sum(rate(node_cpu_seconds_total{mode!="idle"}[5m])) by (instance)
```

Run these in Prometheus UI (`Graph` tab) or as Grafana panel queries.

---

## 12. Monitor a Custom Application (ServiceMonitor)

To scrape metrics from your own app, expose a `/metrics` endpoint and create a
`ServiceMonitor` CRD so Prometheus Operator auto-discovers it:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: my-app-monitor
  namespace: prometheus
  labels:
    release: monitoring   # must match the Helm release label Prometheus selects
spec:
  selector:
    matchLabels:
      app: my-app          # must match your Service's labels
  namespaceSelector:
    matchNames:
      - default
  endpoints:
    - port: metrics         # named port on your Service exposing /metrics
      interval: 15s
      path: /metrics
```

Apply it:

```bash
kubectl apply -f servicemonitor.yaml
```

Verify target appears: Prometheus UI → **Status → Targets**.

---

## 13. Upgrade / Uninstall

### Upgrade (after changing values.yaml)

```bash
helm upgrade monitoring prometheus-community/kube-prometheus-stack \
  --namespace prometheus \
  -f values.yaml
```

### Uninstall

```bash
helm uninstall monitoring -n prometheus
```

> Note: PVCs are not deleted automatically by Helm. Clean up manually if needed:

```bash
kubectl get pvc -n prometheus
kubectl delete pvc <pvc-name> -n prometheus
```

Delete namespace (optional, removes everything left behind):

```bash
kubectl delete ns prometheus
```

---

## 14. Troubleshooting

| Issue | Check |
|---|---|
| Pods stuck in `Pending` | `kubectl describe pod <pod> -n prometheus` — often a PVC/storage class issue |
| Grafana login fails | Re-fetch the secret; confirm no `values.yaml` override mismatch |
| Prometheus target `DOWN` | Check Service labels match `ServiceMonitor` selector and `release` label |
| No data in Grafana panels | Confirm Prometheus data source URL and that Prometheus is actually scraping (`Status → Targets`) |
| `helm install` fails - already exists | `helm list -n prometheus` then `helm uninstall monitoring -n prometheus` before retrying |

---

## Quick Reference (Command Summary)

```bash
kubectl create namespace prometheus
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install monitoring prometheus-community/kube-prometheus-stack -n prometheus
kubectl get pods -n prometheus
kubectl port-forward -n prometheus svc/monitoring-kube-prometheus-prometheus 9090:9090
kubectl port-forward -n prometheus svc/monitoring-grafana 3000:80
kubectl get secret -n prometheus monitoring-grafana -o jsonpath="{.data.admin-password}" | base64 --decode
```
