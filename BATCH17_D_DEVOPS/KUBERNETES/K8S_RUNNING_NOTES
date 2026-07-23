
1. What Docker & Docker Compose Do Well ???

Docker helps you:

      Package your app and dependencies into portable containers.      
      Run the same container anywhere (dev, test, prod).      
      Isolate processes and environments.
      
Docker Compose adds: (CONTAINER AS A SERVICE) 

    Multi-container setup — e.g., running a web app + DB + Redis + xyz ..etc  together.    
    Simple configuration — define everything in docker-compose.yml.    
    Great for local development and small deployments.
    
So far, so good , we can 

    Package apps consistently (Dockerfile) 
    Define multiple containers easily (docker-compose.yml)
    Run them locally with one command
    Even auto-restart containers with (restart: always)



CHALLENGES WITH DOCKER &  DOCKER COMPOSE 
========================

1 Scaling and High Availability

     Docker Compose doesn’t automatically scale containers across multiple machines.     
     If one container/node fails, you need to manually restart or handle failover.     
     No built-in load balancing or auto-recovery.

2 Multi-host / Cluster Management
      Docker Compose works mostly on a single host (machine)
      Real production systems need to run across multiple servers or clouds — Compose doesn’t handle that.

3. Self-healing & Monitoring
      If a container crashes, Compose won’t automatically reschedule or replace it.   
      You need to handle health checks and restarts manually.

4. Rolling Updates / Zero-downtime Deployments

      Docker Compose lacks advanced deployment strategies (rolling updates, blue-green, canary, etc.).

5. Networking & Service Discovery

      Compose’s network works fine locally, but in a distributed environment you need dynamic service discovery, DNS, and load balancing — which Compose doesn’t provide natively.
	  
6. Resource Management

     You can set limits, but no cluster-level scheduling based on CPU/RAM usage	 
	 
7.  Monitoring and logging need extra tools	 
	 


==============================================
    Docker can RUN containers.

    But Docker alone cannot properly
    ORCHESTRATE containers at production scale.


--------------------------------------------------
WHAT IS CONTAINER ORCHESTRATION?
--------------------------------------------------

    Container Orchestration =
        Managing containers automatically.

    Includes:

        • Scheduling
        • Scaling
        • Networking
        • Recovery
        • Load balancing


--------------------------------------------------
CONTAINER ORCHESTRATION TOOLS
--------------------------------------------------

    • Docker Swarm
    • Kubernetes (K8S)


====================================================================
DOCKER SWARM VS KUBERNETES (K8S)
====================================================================

+----------------------+-------------------------+--------------------------+
| FEATURE              | DOCKER SWARM            | KUBERNETES (K8S)         |
+----------------------+-------------------------+--------------------------+
| Complexity           | Simple                  | Complex                  |
| Setup                | Easy                    | Medium/Hard              |
| Learning Curve       | Easy                    | Steep                    |
| Scaling              | Basic                   | Advanced                 |
| High Availability    | Limited                 | Excellent                |
| Auto Healing         | Basic                   | Advanced                 |
| Load Balancing       | Built-in simple LB      | Advanced LB              |
| Rolling Updates      | Basic                   | Advanced                 |
| Rollback             | Limited                 | Strong support           |
| Monitoring           | Limited                 | Rich ecosystem           |
| Networking           | Simple                  | Advanced                 |
| Multi-host Support   | Yes                     | Yes                      |
| Large Production     | Not ideal               | Best choice              |
| Ecosystem            | Smaller                 | Very large               |
| Community Support    | Medium                  | Huge                     |
| Cloud Support        | Limited                 | Full cloud support       |
| Resource Scheduling  | Basic                   | Intelligent scheduling   |
| Popularity           | Less used               | Industry standard        |
+----------------------+-------------------------+--------------------------+


--------------------------------------------------
DOCKER SWARM
--------------------------------------------------

    • Native Docker orchestration tool
    • Easy to setup
    • Good for small projects
    • Lightweight
    • Limited advanced features


--------------------------------------------------
KUBERNETES (K8S)
--------------------------------------------------

    • Advanced orchestration platform
    • Production-grade
    • Handles massive scale
    • Self-healing
    • Auto scaling
    • Industry standard


--------------------------------------------------
WHEN TO USE DOCKER SWARM?
--------------------------------------------------

    • Learning
    • Small projects
    • Simple applications
    • Small teams


--------------------------------------------------
WHEN TO USE KUBERNETES?
--------------------------------------------------

    • Enterprise production
    • Microservices
    • High traffic applications
    • Large-scale deployments


--------------------------------------------------
WHY KUBERNETES (K8S)?
--------------------------------------------------

    Kubernetes solves production problems.

    Features:

        • Production-grade scaling
        • High Availability
        • Self-healing
        • Auto scaling
        • Rolling updates
        • Auto rollback
        • Load balancing
        • Cluster management


--------------------------------------------------
WHAT IS KUBERNETES?
--------------------------------------------------

    Kubernetes (K8S) is an open-source
    container orchestration platform.

    Used to manage containerized applications.


--------------------------------------------------
KUBERNETES HISTORY
--------------------------------------------------

    • Developed by Google
    • Written in Go/Golang
    • Donated to CNCF in 2014
    • Kubernetes v1.0 released in 2015


FEATURES:
--------------------------------------------------
1) AUTOMATED SCHEDULING
--------------------------------------------------

    Kubernetes automatically places
    containers on best available nodes.


--------------------------------------------------
2) SELF-HEALING
--------------------------------------------------

    If container/node fails:

        Kubernetes automatically:

            • Restarts containers
            • Replaces failed containers
            • Reschedules workloads


--------------------------------------------------
3) ROLLING UPDATES & ROLLBACK
--------------------------------------------------

    Deploy new version without downtime.

    If issue occurs:

        Rollback automatically.


--------------------------------------------------
4) AUTO SCALING
--------------------------------------------------

    Kubernetes can automatically:

        • Scale up
        • Scale down

    Based on:

        • CPU
        • Memory
        • Traffic


--------------------------------------------------
5) LOAD BALANCING
--------------------------------------------------

    Kubernetes distributes traffic
    across multiple containers automatically.





SOFTWARES:  eksctl , kubectl , aws cli , visual studo code (ide) 

MINIKUBE
kind


aws  - EKS (bill) 
azure - AKS
gcp - GKE

1. chacolety 
--------------------------------------
2. eksctl -- CLUSTER CREATION 
 eksctl version
0.215.0

eksctl creaate cluster <b17k8scluster>

--------------------------------------

3. kubectl -- CLI TO INTERACT WITH CLUSTER ( API SERVER) 

 kubectl version --client
Client Version: v1.34.1-eks-113cf36
Kustomize Version: v5.7.1

--------------------------------------
4. AWS CLI 
 aws --version
aws-cli/2.23.9 Python/3.12.6 Windows/11 exe/AMD64

ACCESS KEY 
SECRET ACCESS KEY 

-----------------------------------------

5. visual studio code  (IDE ) 
   - For syntax auto suggestions / syntax auto completion 
   - easy to manage the code / script 

============================


DATE: 26 MAY 2026


WHY K8S 

High availability , Zero downtime ,

docker 
   Dockercli
   --
   containerd(runtime)

K8S ARCHITECTURE:
================

Kubernetes Architecture is based on a master-worker model, where the Master manages and controls the cluster, and the Workers run the containerized applications. Let's break down the architecture and its core components.

KUBERNETES ARCHITECTURE :

Master Node:    The control plane responsible for managing the Kubernetes cluster.
Worker Nodes:   The nodes that run the containerized applications (pods).
Cluster:        A set of worker nodes managed by the master node.


---------------------------------------

1. MASTER NODE (CONTROL PLANE): 
============================
The Master Node is the brain of the Kubernetes cluster. 
It manages the cluster, maintains the desired state, schedules workloads, and monitors the overall health of the cluster.
It consists of several critical components:

     KUBE-APISERVER:     
     The API server is the central point of contact for all Kubernetes components. It handles requests from clients, whether that’s the kubectl CLI, the Kubernetes UI, or other components in the system.     
     The API server exposes the REST API, which is used by external clients to interact with Kubernetes. It ensures the system’s state is updated.
	 
    -  The entry point /Gateway to the Kubernetes cluster.
    -  All commands (like kubectl) talk to the API server.
    -  Think of it as the front desk of K8s.
	   
	 
	 
     ETCD:     
 
	- A key-value database that stores all cluster data — pods, deployments, secrets, etc.
	- It is highly available and persistent - Kubernetes can recover the cluster's state even after a failure.
	- etcd ensures consistency across the entire cluster.
    - It’s like Kubernetes’ memory.
     
	 
     KUBE-SCHEDULER:     
     
	- Watches for new pods that need to be scheduled.
    - Decides which node a pod should run on (based on resources, affinity rules, etc)  
    - ensures that workloads are distributed across nodes in an efficient and optimal manner.
     
     
	 KUBE-CONTROLLER-MANAGER:     
	 
	- Runs various controllers (like replica controller, node controller, etc.) 
    - Ensure that the cluster's desired state is maintained. (desired state = actual state) 
    - Ex: if you said you want 3 replicas but only 2 are running, it creates 1 more. 

     
     CLOUD-CONTROLLER-MANAGER (OPTIONAL):     
	 
    - This component is used when running Kubernetes in a cloud environment. 
	- Interacts with the underlying cloud provider (e.g., AWS, AZURE , Google Cloud) to manage resources like    load balancers, volumes, and instances.
	- It helps in maintaining cloud-specific services in coordination with Kubernetes.
     
	 
2. WORKER NODE COMPONENTS:
===========================

     A Worker Node (also called a Minion) runs the applications and workloads in the form of Pods. Each worker node contains the following components:
     
     KUBELET:     
	 
	- The agent on each worker node that talks to the API Server.
	-  Makes sure containers are running as instructed.
	- takes action to rectify any issues (e.g., restarting a failed container).
	
     
	 
     KUBE-PROXY:     
	 
	- Handles networking — ensures pods can talk to each other and the internet.
	- Manages load balancing and routing traffic to the right pod.     
	
     
     CONTAINER RUNTIME (DOCKER/ cri-o / containerd)  

    - Runs the containers inside a pod (Docker, containerd, CRI-O, etc).
    - It pulls container images, starts containers, and manages the lifecycle of containers within the pods.
	 
======================================     
     PODS:     
	 
	- Pod : The Smallest Deployable Unit in K8S
    - Pod is the basic building block in Kubernetes.
	- It wraps one or more containers that share the same network and storage.
	- Usually, one container per pod (but sometimes more if they’re tightly coupled).
	 

So who actually makes the pod run ? 
   
   
   API Server - Accepts pod creation requests, validates and stores them in etcd 
   Scheduler - Watches for unscheduled pods, selects a node - Decides which node should host the pod
   API Server - Updates Pod with node assignment     
   Kubelet	-  on the chosen node , talks to Container Runtime to  create containers, 
   
   
 So kubelet Actually runs and monitors the pod on its node, kubelet reports status back to API SERVER 

=============================================================================================


Where can you set up a Kubernetes cluster?

1. Self-managed cluster - You install and manage everything yourself.
    Treditional approach - Self managed , barematel , customer managed k8s cluster 
    Kubeadm → install K8s manually on servers (on-prem or in the cloud).
	
	
  Kubernetes Control Plane - Self Managed  
        Need to make Control Plane Highly Available
        Maintain multiple EC2 in multiple AZ
        Scale Control Plane if needed
        Keep etcd up and running
        Overhead of managing EC2s
        Security Patching
        Replace failed EC2s/vms/physical servers
        Orchestration for Kubernetes Version Upgrade

2. Managed Kubernetes services (cloud)
    
	 k8s cluster as a service , Cloud providers give you a ready-to-use Kubernetes control plane.
     You just focus on your apps, not managing Kubernetes itself.

         AWS	   EKS (Elastic Kubernetes Service)
         GCP      GKE (Google Kubernetes Engine)
         Azure	   AKS (Azure Kubernetes Service)



   
   CLOUD MANAGED KUBERNETES ( EKS / AKS / GKE ) 

       Kubernetes Control Plane - AWS Managed - eks
       
       ● AWS Manages Kubernetes Control Plane 
       ● Creates and manages the Control Plane for you (API server, etcd, scheduler, etc.)
       ● Handles upgrades, scaling, and security patches automatically.
       ● AWS maintains High Availability - Multiple EC2s in Multiple AZs
       ● AWS Detects and Replaces Unhealthy Control Plane Instances
       ● AWS Scales Control Plane
       ● Provides Automated Version Upgrade and Patching


   in EKS you only need to manage Data Plane 

   ● Create Worker Nodes (EC2 instances or Fargate tasks)   - we will use managed node groups 
   ● Deploy our  Pods and apps.
   ● Cluster configuration
   ● Security policies
   ● Monitoring/logging
   
   
   
Aws - eks 
EKS K8S CLUSTER SETUP 


====================================================

Ways To Spin Up EKS Cluster:

           
           eksctl CLI
		   
		   
          
What is eksctl - CloudFormation stack
terraform 
  

     ● CLI tool for creating clusters on EKS
     ● Easier than console
     ● Abstracts lots of stuff - VPC, Subnet, Sec. Group , nodes , docker etc.
     using CloudFormation stack 

=======================	   
aws configure

Accesskey 
Secret Access key 
---
AKIA5FQ3NZxxxxxxxxxxxxxxxxxx
awdmV1Uglg3Tvxxxxxxxxxxxxxxxxxxxxx

1. awscli & configufre 
aws sts get-caller-identity

once configure is done , our terminal will have admin accss to our aws account 


=======================================================
eks --> free
200 $

eksctl create cluster  

--> it creates  two m5.large large nodes by default for data plane . Insted go for t3.micro

  
     Control Plane - AWS WILL TAKE CARE !   
     Data Plane  - 2 m5.large worker nodes 


eksctl create cluster --name <name> --version <> --nodegroup-name <> --node-type t3.micro --nodes 4 --managed 



eksctl create cluster --name b17dcluster --nodegroup-name b17ng --node-type t3.micro --nodes 4 --managed 


eksctl create cluster prod
=======================================================


eksctl get cluster 
eksctl delete cluster <clustername> 
=============================================================


2024-11-07 19:03:13 [ℹ]  waiting for CloudFormation stack "eksctl-b15cluster-cluster"
:
:
2024-11-07 19:34:33 [ℹ]  waiting for CloudFormation stack "eksctl-b15cluster-nodegroup-b15ng"
:
:
2024-11-07 19:37:57 [ℹ]  nodegroup "b15ng" has 4 node(s)
2024-11-07 19:37:57 [ℹ]  node "ip-192-168-1-245.ec2.internal" is ready
2024-11-07 19:37:57 [ℹ]  node "ip-192-168-30-5.ec2.internal" is ready
2024-11-07 19:37:57 [ℹ]  node "ip-192-168-48-160.ec2.internal" is ready
2024-11-07 19:37:57 [ℹ]  node "ip-192-168-52-207.ec2.internal" is ready
2024-11-07 19:37:57 [✔]  created 1 managed nodegroup(s) in cluster "b15cluster"
2024-11-07 19:38:01 [ℹ]  kubectl command should work with "C:\\Users\\Dell\\.kube\\config", try 'kubectl get nodes'
2024-11-07 19:38:01 [✔]  EKS cluster "b15cluster" in "us-east-1" region is ready


-------------------------------------------------------------
CONTROL PLANE (MASTER NODES) :

Kubernetes master nodes are distributed across several AWS availability zones (AZ), and traffic is managed by Elastic Load Balancer (ELB).

EKS is a managed service, so you have no direct access to the master nodes. And you don't need the access.
==================================

pod - Containers are encapsulated into a k8s object called pod ,in simple words pod is a abstraction on containers 

Pod can have multiple continers but in general not of same container type (mostly helper container) 	

--------------------------------------------
Creating a POD from Commandline : 


docker run --name ct1 image
kubectl run testpod --image=tomcat 


================================================================
Important: Your Account Type Matters ⚠️

AWS now has:

            Free Plan
            Paid Plan

        EKS is usually not available on the restricted Free Plan because it is a paid service. To use EKS, you may need to upgrade to the Paid Plan. Your credits will still apply after upgrading.

        So:
        ✅ Upgrade to Paid Plan
        ✅ Credits still remain
        ✅ EKS charges consume credits automatically

Here’s the order AWS generally follows 👇

            Free Tier benefits applied first
            Remaining bill calculated
            AWS promotional credits applied next
            Your card charged only if bill still remains



=================================================================


29 MAY 2026 

DISCUSSED :
✅ Pod and its lifecycle/status
✅ ImagePullBackOff error – causes and troubleshooting
✅ CrashLoopBackOff error – causes and troubleshooting
✅ Pod in Pending state – reasons and debugging
✅ Pod creation using Manifest YAML files


yaml - one of the data structure format to represent data 
       yaml file is used to represent data , in our case k8s object configuration data

for ex : 
server Info : 
name = insta 
created = 29-05-2026 
status = active 
owner = Madhu




server details :


name = insta
created = 29-05-2026
status = active 
owner = Madhu

name = whatsapp
owner = kiran
created = 29-05-2026
status = active 



====================

--- show above server data as xml , yaml and json format 

-----------------------------------
key value pair - 

name: server1

-------------------
Array/ List :

Fruits:
- grape   
- banana
- Orange: 
    Carbs: 5g   
	Fat: 0.1 g
	Calories: 50


  
- apple
    Fat: 0.1 g
    Carbs: 5g
    Calories: 50
    
    
-------------------

Dictionary - Set of properties , grouped together

Orange: 
   Calories: 50
   Fat: 0.1 g
   Carbs: 5 g
   
   
make sure we are giving spaces properly 
 - equal no of spaces   
----------------------------------------


INSTALL VSCODE --> INSTALL YAML EXTENSION --> GO TO SCHEMA: SETTING.json

=================


    "yaml.schemas": {    

        "kubernetes": "*.yaml" 
        
    },

==========================================================

cat pod1.yaml

---
apiVersion: v1
kind: Pod
metadata:
  name: insta-pod
  labels:
    app: insta
    env: production

spec:
  containers:
  - name: insta-container
    image: tomcat 

============================================================
kubectl apply -f pod1.yaml


===============================

1 JUNE 2026

--------
kubectl run pod nginx --image nginx:latest --dry-run -o yaml 
--------
kubectl explain pods ---> gives explaination of pods manifest files 

--------
kubectl explain pods.metadata

--------
kubectl cluster-info
kubectl cluster-info dump ---> total cluster info

--------

kubectl api-resources --->resorces we can create like pods,deployments...etc

kubectl api-resources | grep pod 
--------
kubectl cordon instance ip ---> unschedule the nodes ,no new pod will be schedule
Marks a node as unschedulable—new pods will not be scheduled on it, but existing pods continue to run.. 
when You want to stop placing new workloads on the node but don't want to disrupt what's already running.
--------

kubectl drain ip-172-20-103-135.ec2.internal --->  evicts all pods to another node and schedule will be disable
--------
kubectl uncordon ip-172-20-38-30.ec2.internal


kubectl get all 
==========================		
kubectl get replicaset
NAME     DESIRED   CURRENT   READY   AGE
tomcat   5         5         5       116s
-----------

So Why do we need labels and Selectoers ?

Labels can be used as filter to replica set , so replica set will monitor and ensure desired no of pods are running 

==========================================================
scale replicas and ways to scale :

1. kubectl scale replicaset insta-rs --replicas=3
1. Change manifest file -->- then kubectl apply -f insta-rs.yaml
3. kubectl scale --replicas=6 -f insta-rs.yaml
4. kubectl edit replicaset <replicasetname> --> opens a temporoay file in memory



History:
421  cd 1june2026/
  422  ll
  423  cat 1-pod.yaml
  424  kubectl get po
  425  kubectl describe po fb-pod
  426  kubectl cluster-info
  427  kubectl cluster-info dump
  428  kubectl api-resources
  429  kubectl api-resources
  430  kubectl run pod2 --image tomcat --dry-run -o yaml
  431  kubectl get nodes
  432  kubectl get po
  433  ll
  434  kubectl apply -f 3-mc-pod.yaml
  435  kubectl get po
  436  kubectl describe po twitter-pod
  437  kubectl get po
  438  kubectl logs twitter-pod -c twitter-container-1
  439  kubectl logs twitter-pod -c twitter-container-2
  440  kubectl apply -f 3-mc-pod.yaml
  441  kubectl apply -f 3-mc-pod.yaml
  442  kubectl apply -f 3-mc-pod.yaml
  443  kubectl get nodes
  444  kubectl get nodes -o wide
  445  kubectl get po -o wide
  446  kubectl get po -o wide
  447  kubectl cordon ip-192-168-39-105.ec2.internal
  448  kubectl get nodes
  449  kubectl get po -o wide
  450  kubectl uncordon ip-192-168-39-105.ec2.internal
  451  kubectl get nodes
  452  kubectl drain ip-192-168-39-105.ec2.internal
  453  kubectl drain ip-192-168-39-105.ec2.internal
  454  kubectl drain ip-192-168-39-105.ec2.internal --ignore-daemonsets
  455  kubectl drain ip-192-168-39-105.ec2.internal --ignore-daemonsets --force
  456  kubectl get nodes
  457  kubectl get po -o wide
  458  kubectl delete po pod1
  459  kubectl delete po twitter-pod
  460  kubectl api-resources
  461  kubectl api-resources | grep replicaset
  462  ll
  463  kubectl apply -f 3-mc-pod.yaml
  464  kubectl apply -f 4-rs.yaml
  465  kubectl get all
  466  kubectl get rs
  467  kubectl get po
  468  kubectl delete po instagram-rs-qjbw9
  469  kubectl get po
  470  kubectl get rs
  471  kubectl describe rs instagram-rs
  472  kubectl get po -o wide
  473  kubectl apply -f 4-rs.yaml
  474  kubectl get all
  475  kubectl scale rs instagram-rs --replicas 5
  476  kubectl get all
  477  kubectl describe po /instagram-rs-zghgm
  478  kubectl describe po instagram-rs-zghgm
  479  kubectl scale rs instagram-rs --replicas 2
  480  kubectl get all
  481  kubectl edit rs instagram-rs
  482  kubectl get all
  483  kubectl edit rs instagram-rs
  484  kubectl get all
  485  kubectl delete rs instagram-rs
  486  kubectl get all
  487  ll
  488  kubectl apply -f 5-pod.yaml
  489  kubectl get po
  490  kubectl apply -f 4-rs.yaml
  491  kubectl get po
  492  kubectl get po --show-labels
  493  kubectl delete po instgram-pod
  494  kubectl get po --show-labels
  495  git status
  496  git add . && git commit -m "1 june 26"
  497  git push
  498  eksctl get cluster
  499  eksctl delete cluster b17dcluster

-
====================================================
DATE : 2 JUNE 2026



=====================================================================

To create a deployment using imperative command, use kubectl create:

kubectl create deployment nginx --image=nginx

kubectl create deployment instadeploy --image=<image> --replicas=3 --dry-run -o yaml
--------------------------------------------------------


cat 1-deploy_AC_V1.yaml


---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: fb-deployment
  labels:
    app: fb-app
    env: prod
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  selector:
    matchLabels:
      app: fb-app
      env: prod
  template:
    metadata:
      labels:
        app: fb-app
        env: prod
        version: v1_ac 
    spec:
      containers:
      - name: facebook-container
        image: jenkins/jenkins:lts
        ports:
        - containerPort: 8080
      
====================
kubectl apply -f 02-DEPLOYMNET-PRACTICE/1-deploy_AC_V1.yaml

==================================================================

cat 2-deploy_VC_v2.yaml
---



---
apiVersion: apps/v1
kind: Deployment 
metadata:
  name: fb-deployment
  labels:
    app: fb-app
    env: prod
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  
  selector:
    matchLabels:
      app: fb-app
      env: prod
  template:
    metadata: 
      labels:
        app: fb-app
        env: prod
        version: v2_vc

    spec:
      containers: 
      - name: facebook-container
        image: devopshubg333/batch17d:pythonimg_v1
        ports:
        - containerPort: 8080
      
==============
kubectl apply -f 02-DEPLOYMNET-PRACTICE/1-deploy_AC_V1.yaml
kubectl apply -f 02-DEPLOYMNET-PRACTICE/2-deploy_VC_v2.yaml

 kubectl get deployment

------------

 kubectl rollout pause deploy fb-deploy
 kubectl rollout resume deploy fb-deploy
 kubectl rollout undo deploy fbdeploy



====
 cd 2june2026/
  517  ll
  518  cat 1-rs.yaml
  519  kubectl apply -f 1-rs.yaml
  520  kubectl get all
  521  kubectl delete rs fb-replicaset
  522  ll
  523  kubectl apply -f 2-fb-deploy_V1.yaml
  524  kubectl get deploy
  525  kubectl get all
  526  kubectl delete  deploy fb-deploy
  527  kubectl delete  deploy fb-deployment
  528  kubectl apply -f 2-fb-deploy_V1.yaml
  529  kubectl get all
  530  kubectl describe deploy fb-deployment
  531  ll
  532  kubectl apply -f 3-fb-deploy_V2.yaml
  533  kubectl get all
  534  kubectl get all
  535  kubectl rollout pause deploy fb-deployment
  536  kubectl rollout resume deploy fb-deployment
  537  kubectl get po --watch
  538  kubectl get all
  539  kubectl describe deploy fb-deployment
  540  kubectl describe deploy fb-deployment
  541  kubectl describe deploy fb-deployment
  542  kubectl get all
  543  kubectl get po
  544  kubectl get po -o wide
  545  kubectl delete po fb-deployment-58b564c465-fkxpn
  546  kubectl get po -o wide
  547  kubectl get po --show-labels
  548  kubectl delete po fb-deployment-58b564c465-w824b
  549  kubectl get po --show-labels
  550  ll
  551  kubectl apply -f 4-lb-svc.yaml
  552  kubectl get all
  553  kubectl get all
  554  kubectl describe deploy fb-deployment
  555  kubectl apply -f 3-fb-deploy_V2.yaml
  556  kubectl get all
  557  kubectl rollout undo deploy fb-deployment
  558  kubectl get all
  559  kubectl apply -f 3-fb-deploy_V2.yaml
  560  kubectl delete deploy /fb-deployment
  561  kubectl delete deploy fb-deployment
  562  kubectl get all
  563  kubectl get all
  564  kubectl apply -f 2-fb-deploy_V1.yaml
  565  kubectl get all
  566  kubectl apply -f 3-fb-deploy_V2.yaml
  567  kubectl get po --watch
  568  hisroty

==================================================================

DATE: 3 JUNE 2026

In Kubernetes, Service types are abstractions that define how to expose Pods. The common types are:

      ClusterIP (default)
      
      NodePort
      
      LoadBalancer

-------------------------------------------

flow :

                   Client (Internet)
                      ↓
                   External Load Balancer
                      ↓
                   NodePort (on a Kubernetes Node)
                      ↓
                   ClusterIP (Kubernetes internal)
                      ↓
                   Pod (endpoint)


----------------------------------------------




NameSpaces :

Namespaces are Kubernetes objects which partition a single Kubernetes cluster into multiple virtual clusters

which means that using the combination of an object name and a Namespace, each object gets an unique identity across the cluster.
------------

A namespace in Kubernetes is a way to divide your cluster into smaller, isolated spaces for better organization and management.
------------

When you have a big Kubernetes cluster that runs many applications, you don’t want everything mixed together.
So, you create namespaces to separate and organize things.

Example:

Imagine you have one big Kubernetes cluster for your company.

You can make:

     dev namespace → for developers to test things     
     staging namespace → for testing before release     
     prod namespace → for real users
     
Each namespace can have its own:

       Pods (containers)       
       Services       
       Configurations       
       Resource limits (CPU, memory, etc.)

And Kubernetes makes sure that things in one namespace don’t clash with another — for example, you can have a web service in both dev and prod, and they won’t conflict.

--------------------------------------------------------

kubectl get pod -o wide
kubectl get pod -o wide --all-namespaces

=================================

kubectl get ns

kubectl create ns kirannamespace

kubectl apply  -f pod.yaml -n kirannamespace

kubectl get pods --all-namespaces

=========================================

history:
 cd 3-june26/
  507  ll
  508  kubectl apply -f 2-fb-deploy_V1.yaml
  509  kubectl get all
  510  kubectl get po
  511  kubectl get po -o wide
  512  kubectl get po -o wide
  513  kubectl exec -it fb-deploy-cb496c7bc-cvvb2 -- bash
  514  ll
  515  kubectl apply -f 4-lb-svc.yaml
  516  kubectl get all
  517  kubectl get svc
  518  kubectl get svc
  519  ll
  520  kubectl apply -f 3-fb-deploy_V2.yaml
  521  kubectl get all
  522  kubectl exec -it fb-deploy-7f9d4f8dd9-88bj9 -- bash
  523  kubectl rollout undo deploy fb-deploy
  524  kubectl describe deploy fb-deploy
  525  kubectl get svc
  526  ll
  527  kubectl apply -f 5-cip-svc.yaml
  528  kubectl get svc
  529  kubectl get svc
  530  kubectl get po
  531  kubectl exec -it fb-deploy-cb496c7bc-49nzl -- bash
  532  ll
  533  kubectl apply -f 6-nodeport-svc.yaml
  534  kubectl get svc
  535  kubectl get svc
  536  kubectl delete all --all
  537  ll
  538  pwd
  539  cd ../1june2026/
  540  ll
  541  cat 1-pod.yaml
  542  kubectl apply -f 1-pod.yaml
  543  kubectl apply -f 1-pod.yaml
  544  kubectl describe po fb-pod
  545  kubectl create ns dev
  546  kubectl create ns QA
  547  kubectl create ns qa
  548  kubectl create ns uat
  549  kubectl create ns prod
  550  kubectl get ns
  551  ll
  552  kubectl apply -f 1-pod.yaml -n qa
  553  kubectl apply -f 1-pod.yaml -n prod
  554  kubectl get po
  555  kubectl get po -n qa
  556  kubectl get po -n prod
  557  kubectl get po --all-namespaces
  558  kubectl create ns preprod
  559  ll
  560  kubectl apply -f 1-pod.yaml -n preprod
  561  kubectl get po --all-namespaces
  562  kubectl apply -f 1-pod.yaml
  563  kubectl get ns
  564  kubectl apply -f 1-pod.yaml
  565  kubectl get po -n uat
  566  kubectl get po --all-namespaces
  567  kubectl get ns
  568  eksctl get cluster
  569  eksctl delete cluster b17d
===============================


4 JUNE :

MINI PROJECT 



=======================================

5 JUNE 2026

----------------------
RESOURCE REQUESTS & LIMITS :

Resource requests & limits :

Resource Requests
A request is the amount of CPU and memory that Kubernetes will allocate to a container. When you specify a request, Kubernetes ensures that your container will always have at least that much resource available when it runs

------------
LIMITS : 
The maximum amount of CPU and memory the container can use. 
If the container exceeds these, it may be throttled (CPU) or killed (Memory).



with out resource requests & limits :

	  
-------
     Kubernetes has no idea how much CPU/memory this needs.
     It might schedule it anywhere.     
     If the node gets busy, this pod could slow down or get evicted.     
     It can also use unlimited CPU/memory, possibly hurting other apps.	  
	  

========================================================


apiVersion: v1
kind: Pod
metadata:
  name: with-resources
spec:
  containers:
    - name: web
      image: nginx:latest
      resources:
        requests:
          cpu: "200m"     # needs at least 0.2 of a CPU core
          memory: "256Mi" # needs at least 256 MB of RAM
        limits:
          cpu: "500m"     # can use up to 0.5 CPU core
          memory: "512Mi" # can use up to 512 MB of RAM
		  
		  
---------------------------------------------------		  
   Kubernetes knows this pod needs a small amount of CPU/memory to start.   
   It will schedule it on a node that has enough room.   
   If the pod tries to use more than the limit   
       CPU: it just gets throttled (slowed down).       
       Memory: it gets killed (OOMKilled) to protect the node.		  
       

==========================================================================
how to log into the pod ? 

kubect exec -it <podname> -- /bin/bash
kubect exec -it <podname> -- env

----------------------

ENV vs CONFIG MAP vs  SECRET 

Environment Variables (ENV):

        environment variables are key–value pairs injected into the container’s runtime environment.
        App configs like port numbers, feature flags, etc.
        Env variables are Typically non-sensitive.
        Not encrypted.
        Easily visible in kubectl describe pod.

You can define them:

       Directly in a Pod spec
       Or indirectly using a ConfigMap or Secret



ENV vs CONFIG MAP vs  SECRET 

Environment Variables (ENV):

        environment variables are key–value pairs injected into the container’s runtime environment.
        App configs like port numbers, feature flags, etc.
        Env variables are Typically non-sensitive.
        Not encrypted.
        Easily visible in kubectl describe pod.

You can define them:

       Directly in a Pod spec
       Or indirectly using a ConfigMap or Secret
=====================

ENV:

--- 
apiVersion: v1
kind: Pod
metadata:
  name: envex-pod 

spec: 
  containers:
    - name: insta-pod-cont
      image: devopshubg333/batch16d:fb_httpd
      env:
        - name: Course
          value: AWS DevOps 

        - name: Trainer 
          value: Madhukiran Gorekar 

        - name: Institute
          value: MindCircuit 
    

===

=====================

ConfigMap

   ConfigMaps are Kubernetes API objects that store configuration data as key–value pairs (strings).
    They’re meant for non-sensitive settings.

Ways to use a ConfigMap:
      As environment variables
      As command-line arguments
      As files mounted into a container


configmap.yaml:


---
apiVersion: v1
kind: ConfigMap
metadata:
  name: mc-config

data:
  Course: AWS DevOps 
  Trainer: Madhukiran Gorekar
  Institute: MindCircuit


-------------------------------------------------

--- 
apiVersion: v1
kind: Pod
metadata:
  name: configmap-ex-pod 

spec: 
  containers:
    - name: insta-pod-cont
      image: devopshubg333/batch16d:fb_httpd

      envFrom:
        - configMapRef:
            name: mc-config

===========================================

Secret:

 Secrets are like ConfigMaps, but intended for sensitive data such as credentials or tokens.
Kubernetes stores them base64-encoded, not encrypted by default (though you can enable encryption at rest).

A Secret is used to store sensitive data, such as passwords, OAuth tokens, SSH keys, and API keys, in a way that is more secure than using a plain ConfigMap. 

Secrets are encoded (base64), which helps to obscure their content but is not a fully secure solution; they are more secure than environment variables or plain text files


encode:


 echo -n "AWS DevOps" | base64
QVdTIERldk9wcw==

echo -n "Madhukiran Gorekar" | base64
TWFkaHVraXJhbiBHb3Jla2Fy

echo -n "MindCircuit" | base64      -----> TWluZENpcmN1aXQ=




decode:
echo -n "QXdzIERldk9wcw==" | base64 --decode
echo -n "TWFkaHVraXJuYSBnb3Jla2Fy" | base64 -d
==================================

---
apiVersion: v1
kind: Secret
metadata:
  name: mc-secret
type: Opaque
data:
  Course: QVdTIERldk9wcw==
  Trainer: TWFkaHVraXJhbiBHb3Jla2Fy
  Institute: TWluZENpcmN1aXQ=


============================

--- 
apiVersion: v1
kind: Pod
metadata:
  name: secret-ex-pod 

spec: 
  containers:
    - name: insta-pod-cont
      image: devopshubg333/batch16d:fb_httpd

      envFrom:
        - secretRef:
            name: mc-secret

=================================================================


kubectl get secret 

=============================================================================
Real time scenarios : 

configmap:

apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: "production"
  LOG_LEVEL: "info"

---------------------------------------------
( username : myuser 
  password : mypassword  ) 
  
echo -n "myuser" | base64
bXl1c2Vy

echo -n "mypassword" | base64
bXlwYXNzd29yZA==



secret:

apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  DB_USER: bXl1c2Vy
  DB_PASSWORD: bXlwYXNzd29yZA==


--------------------------------------------------
Deployment/pod : 

apiVersion: apps/v1
kind: Deployment
metadata:
  name: backend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: backend
  template:
    metadata:
      labels:
        app: backend
    spec:
      containers:
      - name: backend
        image: myapp:1.0
        env:
          - name: SERVICE_NAME
            value: "user-service"
        envFrom:
          - configMapRef:
              name: app-config
          - secretRef:
              name: db-secret


==========================================================================




------------------------------------------------
CLUSTER UPGRADE in K8S 
=====================================================================

======================================

=============================
LABELS vs ANNOTATIONS in K8S 


---
apiVersion: v1
kind: Pod
metadata:
  name: fbpod
  labels:
    app: facebook
	env: lab
	
  annotations:
    imageregistry: "https://hub.docker.com/"
    build-url: "https://jenkins.example.com/build/142"
	prometheus.io/scrape: "true"
    prometheus.io/port: "8080"
	
	
spec:
  containers:
  - name: nginx
    image: nginx


=============================

10 JUNE 26


ARGOCD : 

kubectl create namespace argocd

kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml -n argocd


kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'




username: admin 

Get the Initial Admin Password:
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

==============================================================






ADV SCHEDULING : 

DATE: 15 JUNE 2026 
========================================
KUBERNETES SCHEDULING 
========================================

1. WHAT IS SCHEDULING?
----------------------
In Kubernetes, the scheduler decides - which node a Pod will run on.
By default, Kubernetes automatically picks the best node based on CPU, memory, and node load.

But you can control where your Pods run using:
- nodeName
- nodeSelector
- nodeAffinity / podAffinity / podAntiAffinity
- taints and tolerations

These features give you fine control over Pod placement.


2. NodeName — FORCE A POD TO RUN ON A SPECIFIC NODE
---------------------------------------------------
The simplest and most direct method.

Example:
  apiVersion: v1
  kind: Pod
  metadata:
    name: pod-on-node3
  spec:
    nodeName: worker-node3
    containers:
    - name: nginx
      image: nginx

Meaning:
This Pod will run ONLY on the node named worker-node1.

Note:
If the node doesn’t exist or is not ready, the Pod stays in "Pending" state.


3. nodeSelector — SELECT NODE BASED ON LABELS
---------------------------------------------
More flexible than nodeName.

Step 1: Label your node
  kubectl label nodes worker-node1 env=prod

Step 2: Use nodeSelector in Pod spec
  apiVersion: v1
  kind: Pod
  metadata:
    name: pod-prod
  spec:
    nodeSelector:
      env: prod
    containers:
    - name: nginx
      image: nginx

Meaning:
This Pod will only run on nodes that have the label env=prod.


4. nodeAffinity — ADVANCED NODE SELECTION
-----------------------------------------
nodeAffinity provides more powerful selection logic with expressions and operators.

Example 1: REQUIRED NODE AFFINITY
  apiVersion: v1
  kind: Pod
  metadata:
    name: pod-with-node-affinity
  spec:
    affinity:
      nodeAffinity:
        requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: env
              operator: In
              values:
              - prod
    containers:
    - name: nginx
      image: nginx

Meaning:
This Pod will only run on nodes that have the label env=prod.

------------
requiredDuringSchedulingIgnoredDuringExecution

This is a hard requirement:
The Pod must be scheduled onto a node that satisfies the rules.
If no matching node exists, the Pod remains in Pending state.

IgnoredDuringExecution means that once the Pod is running, Kubernetes won't evict it if the node labels later change.

-------------
preferredDuringSchedulingIgnoredDuringExecution:

This is a soft requirement 

--------------------------------------
  apiVersion: v1
  kind: Pod
  metadata:
    name: pod-with-node-affinity
  spec:
    affinity:
      nodeAffinity:
        requiredDuringSchedulingIgnoredDuringExecution:  
          nodeSelectorTerms:
          - matchExpressions:
            - key: env
              operator: In
              values:
              - prod
            - key: region
              operator: NotIn
              values:
              - us-west-1
    
    containers:
    - name: nginx
      image: nginx

This Pod will only run on nodes that have the label env=prod and not in region us-west-1.

-------------------------------------------------------------
Example 2: PREFERRED NODE AFFINITY (soft rule)



apiVersion: v1
kind: Pod
metadata:
  name: pod-preferred-affinity
spec:
  affinity:
    nodeAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      # Preference 1: Prefer nodes in region us-east-1
      - weight: 10
        preference:
          matchExpressions:
          - key: topology.kubernetes.io/region
            operator: In
            values:
            - us-east-1

      # Preference 2: Prefer nodes in availability zone us-east-1b
      - weight: 5
        preference:
          matchExpressions:
          - key: topology.kubernetes.io/zone
            operator: In
            values:
            - us-east-1b

  containers:
  - name: nginx
    image: nginx



Meaning:
Prefer nodes in region us-east-1, but schedule elsewhere if unavailable.


---------------------
  spec:
    affinity:
      nodeAffinity:
        preferredDuringSchedulingIgnoredDuringExecution:
          - weight: 10
            preference:
              matchExpressions:
              - key: region
                operator: In
                values:
                - us-east-1
          - weight: 5
            preference:
              matchExpressions:
              - key: env
                operator: In
                values:
                - prod





5. podAffinity — RUN PODS CLOSE TO EACH OTHER
---------------------------------------------
Used when you want certain Pods to run on the same node or nearby.

Example:
  apiVersion: v1
  kind: Pod
  metadata:
    name: frontend
  spec:
    affinity:
      podAffinity:
        requiredDuringSchedulingIgnoredDuringExecution:
        - labelSelector:
            matchExpressions:
            - key: app
              operator: In
              values:
              - backend
          topologyKey: "kubernetes.io/hostname"
    containers:
    - name: nginx
      image: nginx

Meaning:
This Pod will run on the same node as another Pod with the label app=backend.


DATE: 16 JUNE 2026 

6. podAntiAffinity — KEEP PODS APART
------------------------------------
Used to spread Pods across nodes for high availability.

Example:
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: web
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: web
    template:
      metadata:
        labels:
          app: web
      spec:
        affinity:
          podAntiAffinity:
            requiredDuringSchedulingIgnoredDuringExecution:
            - labelSelector:
                matchExpressions:
                - key: app
                  operator: In
                  values:
                  - web
              topologyKey: "kubernetes.io/hostname"
        containers:
        - name: nginx
          image: nginx

Meaning:
Kubernetes will NOT schedule two web Pods on the same node.


7. TAINTS AND TOLERATIONS — PROTECT NODES
-----------------------------------------
Taints repel Pods. Tolerations allow specific Pods to bypass that taint.

Step 1: Taint a node
  kubectl taint nodes worker-node1 dedicated=database:NoSchedule

Meaning:
No Pods can run on this node unless they have a matching toleration.

Step 2: Add a toleration to your Pod
  apiVersion: v1
  kind: Pod
  metadata:
    name: db-pod
  spec:
    tolerations:
    - key: "dedicated"
      operator: "Equal"
      value: "database"
      effect: "NoSchedule"
    containers:
    - name: mysql
      image: mysql

Meaning:
This Pod is allowed to run on a node tainted with dedicated=database:NoSchedule.


8.   WHEN TO USE WHAT

   check  image 




COMBINING MULTIPLE RULES
----------------------------
You can mix multiple scheduling features in one Pod spec.

Example:
  spec:
    nodeSelector:
      env: prod  #2,4,5
    tolerations:  #2,4
    - key: "dedicated"
      operator: "Equal"
      value: "web"
      effect: "NoSchedule"
    affinity:
      podAntiAffinity:
        requiredDuringSchedulingIgnoredDuringExecution:
        - labelSelector:
            matchLabels:
              app: web
          topologyKey: "kubernetes.io/hostname"

Meaning:
Run only on production nodes (env=prod), tolerate the "web" taint, and ensure no two web Pods are on the same node.


==========================

Pod affinity 
pod antiaffinity 
taint 
tolerations 
===========================================


========================================
KUBERNETES HEALTH CHECKS (PROBES)
========================================

1. WHAT ARE HEALTH CHECKS (PROBES)?
-----------------------------------
Kubernetes uses probes to check if your container is healthy and ready to serve traffic.
Without probes, Kubernetes assumes your app is always fine — even when it’s stuck or crashed.
Probes help detect and automatically fix unhealthy containers.




Types of probes:
- Liveness Probe: Checks if the app is alive.
- Readiness Probe: Checks if the app is ready to serve traffic.
- Startup Probe (optional): Used for slow-starting apps to avoid false failures.

WHY PROBES :

By default, Kubernetes only checks the container’s main process to decide if it’s healthy.
If the main process crashes, Kubernetes restarts the container.

However, this can be misleading:
- A process might still be running, but the application inside is broken (e.g., timeouts, bugs, DB connection loss).
- Kubernetes would still mark it as “Running,” even though it’s not actually healthy.

Probes solve this by letting you define custom health checks,  that tell Kubernetes when to restart or stop routing traffic to a container.

----------------------------------------

Kubernetes provides three types of probes:

1. Liveness Probe   – Checks if the container is alive.
2. Readiness Probe  – Checks if the container is ready to serve traffic.
3. Startup Probe    – Ensures the container has fully started before other probes run.
			         

------------------------------------------------------------
LIVENESS PROBE
------------------------------------------------------------

The Liveness probe determines whether a container is healthy and running properly.

Action:
- If the probe fails (exit code ≠ 0), Kubernetes restarts the container.
- Ensures that only healthy containers remain active.

$?  --> 0 


Supported Mechanisms:
---------------------
1. Exec Command:
   - Runs a command inside the container.
   - Success → exit code 0.
   - Failure → exit code 1.

2. HTTP Request:  (curl)
   - Kubernetes calls a URL within the container.
   - Success → HTTP status 200–399.
   - Failure → Any other response.

3. TCP Socket:  mysql : 3306 (netstat )
   - Checks if a specific port is accepting traffic.
   - Success → Port is open.
   - Failure → Port is closed.

----------------------------------------
4. LIVENESS PROBE EXAMPLE
----------------------------------------
apiVersion: v1
kind: Pod
metadata:
  name: liveness-pod
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
   
    livenessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 10
      failureThreshold: 3



      ## kubectl exec -it liveness-pod -- curl localhost:8080

Explanation:
- Kubernetes waits 5 seconds before first check.
- Checks every 10 seconds.
- If 3 checks fail, container restarts.

Action:
- If the probe fails (exit code ≠ 0), Kubernetes restarts the container.
- Ensures that only healthy containers remain active.

--------------------------------

apiVersion: v1
kind: Pod
metadata:
  name: liveness-pod
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80

    readinessProbe:
      exec:
        command:
        - cat
        - /usr/share/nginx/html/iindex.html
      initialDelaySeconds: 5    
      periodSeconds: 10         
      failureThreshold: 3       



------------------------------------------------------------
 READINESS PROBE
------------------------------------------------------------

The Readiness probe checks whether a container is ready to receive traffic.

Action:
- If the probe fails → Pod is removed from the service’s endpoint list (no traffic routed).
- If it succeeds → Pod is added back to the service.

Difference from Liveness Probe:
- Liveness failure → Pod restarts.
- Readiness failure → Pod stays running but stops receiving traffic.

Example:
----------

apiVersion: v1
kind: Pod
metadata:
  name: readiness-pod
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
   
#    livenessProbe:
 #     httpGet:
 #       path: /
 #       port: 80
 #     initialDelaySeconds: 5
 #     periodSeconds: 10
 #     failureThreshold: 3


    readinessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 5

 #   readinessProbe:
 #     httpGet:
  #      path: /ready
 #       port: 8080
  #    initialDelaySeconds: 5
  #    periodSeconds: 5



Explanation:
- Pod is marked ready only if /ready endpoint responds successfully.
- If probe fails, Pod is removed from Service load balancer temporarily.


----------------------------------------
 TCP PROBE EXAMPLE
----------------------------------------
livenessProbe:
  tcpSocket:
    port: 3306
  initialDelaySeconds: 10
  periodSeconds: 20

Explanation:
- Checks if TCP port 3306 (e.g., MySQL) is reachable.
- If not, container restarts.


------------------------------------------------------------
STARTUP PROBE
------------------------------------------------------------

The Startup probe ensures that slow-starting containers aren’t prematurely restarted by Liveness probes.

How It Works:
-------------
- Kubernetes runs the Startup probe first.
- Liveness and Readiness probes run only after the Startup probe succeeds.
- If the Startup probe fails, the container is killed and restarted based on the pod’s restart policy.

When to Use:
------------
Use a Startup probe when your application takes significant time to initialize, to prevent early restarts during startup.



    # --- STARTUP PROBE ---
    startupProbe:
      exec:
        command:
        - sh
        - -c
        - "mysqladmin ping -h 127.1.2.1 -u root -prootpassword"
		
      initialDelaySeconds: 10        # Wait 10s before the first check
      periodSeconds: 10              # Check every 10s
      failureThreshold: 30           # wait up to 30 failures, Allow up to 5 minutes (30 * 10s) for startup



Explanation:
- Kubernetes gives app up to 5 minutes (30 x 10s) to start.
- During this time, no liveness or readiness checks run.
- After success, other probes start.

----------------------------------------
9. HOW TO CHECK PROBE STATUS
----------------------------------------
kubectl describe pod <pod-name>

Look in the "Events" section:
Example:
Liveness probe failed: HTTP probe failed with statuscode: 500
Killing container nginx.

----------------------------------------
COMMON PROBE PARAMETERS
----------------------------------------


initialDelaySeconds  --   Wait time before first check                
periodSeconds        --   Time between probe checks                   
timeoutSeconds       --   How long to wait for a response             
failureThreshold     --  Number of failures before marking unhealthy 
successThreshold     --   Successes needed to mark healthy again      
					  

----------------------------------------
 REAL-WORLD COMBINED EXAMPLE
----------------------------------------
apiVersion: apps/v1
kind: Deployment
metadata:
  name: facebookapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:v1
        ports:
        - containerPort: 8080
    
        startupProbe: 
          httpGet:
            path: /healthz
            port: 8080
          failureThreshold: 30
          periodSeconds: 10  # curl <podid:80/healthz>

        livenessProbe:
          httpGet:
            path: /live
            port: 8080
          initialDelaySeconds: 10
          periodSeconds: 5
          failureThreshold: 3

        readinessProbe:
          httpGet:
            path: /ready
            port: 8080
          initialDelaySeconds: 5
          periodSeconds: 5

Explanation:
- startupProbe gives app time to initialize.
- livenessProbe restarts container if it becomes unresponsive.
- readinessProbe ensures Pod only gets traffic when ready.


----------------------------------------
TROUBLESHOOTING TIPS
----------------------------------------
- Use "kubectl describe pod" to see probe failures.
- Check probe paths and ports match your container.
- Increase "initialDelaySeconds" for slow apps.
- For debugging, try "kubectl exec -it <pod> -- curl localhost:<port>/<path>".


------------------------------------------------------------
PROBE EXECUTION FLOW
------------------------------------------------------------

1. Container starts.
2. Kubernetes waits for initialDelaySeconds.
3. Startup Probe runs.
   - Fails → Pod restarts (based on restartPolicy).
   - Succeeds → Move to next step.
4. Liveness and Readiness probes start.
5. Liveness failure → Pod restarts.
6. Readiness failure → Pod removed from service endpoints.
7. Probes continue running periodically.

------------------------------------------------------------
BEST PRACTICES FOR DEFINING PROBES
------------------------------------------------------------

- Keep probes lightweight. Avoid heavy operations (e.g., full DB queries).
- Tune frequency:
   - Too frequent → Resource overhead.
   - Too infrequent → Slow issue detection.
- Use Startup probes for slow apps.
- Skip probes for simple containers that exit cleanly on failure.
- Review probe logic regularly after updates.
- Be mindful of restart policies (Never → failed containers won’t recover).


==========================================================================================================================================

date: 19 june 2026

K8S SECURITY 

========================================
KUBERNETES RBAC
========================================

WHAT IS RBAC?
-------------
RBAC stands for Role-Based Access Control.
It defines who (identity/suject) can do what(role) on which Kubernetes resources.

In short:
- Authentication: Who are you?
- Authorization: What are you allowed to do?

Example: A developer might read Pods but not delete them.


WHY RBAC IS IMPORTANT
---------------------
Without RBAC, anyone with cluster access could modify or delete everything.
RBAC provides fine-grained control, ensuring safety, accountability, and compliance.


MAIN RBAC COMPONENTS: 

--------------------
Suject/identity --> user or Service account 
Role — Defines permissions (verbs) for resources in one namespace.
RoleBinding — Grants Role permissions to users or service accounts in a namespace.


ClusterRole — Defines permissions cluster-wide.
ClusterRoleBinding — Grants ClusterRole permissions cluster-wide.


BASIC FLOW
-----------
Role defines WHAT actions are allowed.( create,delete,deletecollection,get,list,patch,update,watch)
ServiceAccount or User defines WHO is trying to access.
RoleBinding connects them together.


USER CREATION
----------------
- Kubernetes does not manage users directly (unlike Linux systems ).
- You cannot create users using commands like “useradd”.
- Kubernetes delegates authentication to external Identity Providers (IDPs).

IDENTITY PROVIDERS (IDPs)
----------------------------
- Common examples:
  - AWS IAM (for EKS)
  - Azure AD (for AKS)
  - LDAP
  - OCTA
  - SSO (Single Sign-On)
  - Keycloak (open-source identity broker)
  
- Kubernetes API server can work as an OAuth server to authenticate users.
- Users authenticate via existing systems (e.g., Google login, GitHub login).
- In enterprise setups:
  - IAM or SSO defines users and groups.
  - Kubernetes imports these identities and enforces access via RBAC.


SIMPLE EXAMPLE (READ PODS ONLY)  

                ServiceAccount
                    dev-user ( dev namesapce)
                        │
                        ▼
                RoleBinding
                read-pods-binding
                        │
                        ▼
                Role
                pod-reader:
                Permissions:
                get pods
                list pods
                watch pods


-------------------------------
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: dev
  name: pod-reader
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]

apiVersion: v1
kind: ServiceAccount
metadata:
  name: dev-user
  namespace: dev

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: read-pods-binding
  namespace: dev
subjects:
- kind: ServiceAccount
  name: dev-user
  namespace: dev
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io



kubectl auth can-i get pods \
  --as=system:serviceaccount:dev:dev-user \
  -n dev

  "If I were the ServiceAccount dev-user, would I be allowed to get pods in the dev namespace?"


kubectl auth can-i get pods --as system:serviceaccount:dev:dev-user -n dev
kubectl auth can-i delete pods --as system:serviceaccount:dev:dev-user -n dev


 kubectl auth can-i get pods --as=system:serviceaccount:dev:dev-sa -n dev
yes

 kubectl auth can-i delete pods --as=system:serviceaccount:dev:dev-sa -n dev
no


=================================================================================================
DATE: 22 JUNE 2026

KUBERNETES EKS IAM USER ACCESS – 

Create an EKS cluster

Create IAM users




madhu 

kiran



========================
b17-eks-describe-policy

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "eks:DescribeCluster"
            ],
            "Resource": "*"
        }
    ]
}

===========================

Map above IAM users to Kubernetes using the aws-auth ConfigMap (kube-system) 

Assign permissions (edit, view) to each user using RBAC 

Verify access using kubectl

===========================

STEPS:

CREATING THE EKS CLUSTER WITH EKSCTL

     eksctl create cluster --name b17dcluster  --nodegroup-name b17ng --node-type t3.micro --nodes 4 --managed


What this does:

           Creates VPC, subnets, security groups
           
           Creates control plane
           
           Creates worker node group
           
           Automatically generates a kubeconfig entry
           
           Creates the node IAM role and maps it in aws-auth for nodes
           
           After cluster creation, kubectl can talk to the cluster using:
           
           kubectl get nodes
		   

CREATING IAM USERS IN AWS (MADHU AND KIRAN)

Two IAM users were created in the AWS IAM console:

User: madhu
Access Keyxxxxxxx
Secret Access Keyxxxxxxxx

User: kiran
Access Keyxxxxxxx
Secret Access Keyxxxxxxxx


For each user:

An Access Key ID was created

A Secret Access Key was created

These keys are used to configure AWS CLI on the machine where kubectl will run.




COLLECTING IAM USER ARNs

Each IAM user has a unique ARN. Example:

arn:aws:iam::<ACCOUNT-ID>:user/madhu
arn:aws:iam::<ACCOUNT-ID>:user/kiran


These ARNs are required to map the users into Kubernetes using the aws-auth ConfigMap.


-----------------------------


UPDATING aws-auth CONFIGMAP TO MAP IAM USERS TO KUBERNETES

The aws-auth ConfigMap is located in the kube-system namespace.

To edit:

kubectl edit configmap aws-auth -n kube-system

The following lines were added to map the users:

kubectl get configmap aws-auth -n kube-system -o yaml
---------------------------------
  mapUsers: |
    - userarn: arn:aws:iam::141559732042:user/madhu
      username: madhu
      groups:
        - system:authenticated

    - userarn: arn:aws:iam::141559732042:user/kiran
      username: kiran
      groups:
        - system:authenticated
-----------------------------------

Meaning:

Each IAM user is now recognized by Kubernetes

They can authenticate but cannot perform actions yet

Permissions will be assigned using RBAC



kubectl get configmap aws-auth -n kube-system -o yaml
--------------------------------------
apiVersion: v1
data:
  mapRoles: |
    - rolearn: arn:aws:iam::141559732042:role/eksctl-b16dcluster-nodegroup-b16ng-NodeInstanceRole-df0QmXnnuYNj
      groups:
      - system:bootstrappers
      - system:nodes
      username: system:node:{{EC2PrivateDNSName}}
  mapUsers: |
    - userarn: arn:aws:iam::141559732042:user/madhu
      username: madhu
      groups:
        - system:authenticated

    - userarn: arn:aws:iam::141559732042:user/kiran
      username: kiran
      groups:
        - system:authenticated
kind: ConfigMap
metadata:
  creationTimestamp: "2025-11-11T14:13:29Z"
  name: aws-auth
  namespace: kube-system
  resourceVersion: "4469"
  uid: 8d6a9821-8b7b-4e75-b5ab-c80f5af8deca

---------------------------------------------------

ASSIGNING PERMISSIONS USING RBAC
    Check manifest files in folder  

VERIFYING USER ACCESS

Switch AWS CLI credentials to test each user:

aws configure
(enter Madhu's or kiran's keys)

Then test permissions:

kubectl auth can-i create pods
kubectl auth can-i delete deployments
kubectl auth can-i get pods -n prod
kubectl auth can-i list services
kubectl auth can-i create pods --as madhu -n dev
kubectl auth can-i create pods --as madhu -n prod
kubectl auth can-i create pods --as kiran -n dev

Expected results:

kubectl auth can-i create pods --as madhu -n dev

Madhu (edit role) should be able to create, update, delete resources.

Ramani (view role) should only be able to read resources.

kubectl auth can-i create pods --as madhu -n dev

Example tests:
As Madhu:
kubectl create deployment test --image=nginx

As kiran:
kubectl create deployment test --image=nginx
(This should be denied.)


==========================================================================================================================

IRSA ( IAM Roles for Service Accounts) 
------------
IRSA stands for IAM Roles for Service Accounts.
It is a mechanism in Amazon EKS that allows you to give AWS IAM permissions to Pods, not to human users.

IRSA is used when an application running inside Kubernetes needs to access AWS services securely. 

Examples:
- A Pod needs to read/write an S3 bucket.
- A Pod needs DynamoDB read/write access.
- A Pod needs to publish messages to an SNS topic.
- A Pod needs CloudWatch logging permissions.

Instead of giving wide privileges to the entire EKS worker node, IRSA gives permissions only to the specific Pod that needs it.


WHY IRSA IS USED
----------------
Generally, EC2 worker nodes carried broad IAM roles.
All Pods running on the node automatically inherited those wide permissions.
This was insecure and violated least-privilege principles.

IRSA solves this by:
- Attaching IAM roles directly to ServiceAccounts.
- Ensuring Pods using that ServiceAccount have only the permissions defined in that IAM role.
- Eliminating the need to store AWS access keys inside Pods.


HOW IRSA WORKS (CONCEPT)
------------------------
1. You create an IAM Role with a trust policy for the EKS OIDC provider.
2. You annotate a Kubernetes ServiceAccount with that IAM Role ARN.
3. Any Pod that uses that ServiceAccount automatically receives AWS credentials mapped to that IAM Role.
4. The Pod can now call AWS APIs securely with the least-privilege permissions you provided.


IRSA VS IAM USERS (HUMAN ACCESS)
--------------------------------
IRSA is only for Pods.
IAM Users or IAM Roles are for humans and administrators.

Clear comparison:

Feature                     IAM User Access                 IRSA (Pod Access)
---------------------------------------------------------------------------
Purpose                     Human access to EKS cluster     Pod access to AWS services
Level                       Kubernetes authentication       Pod-to-AWS authorization
Tools Used                  aws-auth ConfigMap + RBAC       OIDC provider + IAM role + SA
Example                     User can get pods, deploy apps  Pod can read/write S3 bucket


IMPORTANT DISTINCTION
---------------------
IAM User access = Controls what humans can do with kubectl.
IRSA = Controls what Pods can do when calling AWS services.

They are complementary:
- DevOps engineers, developers, admins use kubectl via IAM + RBAC.
- Applications inside Pods use IRSA to call AWS resources.


EXAMPLE SCENARIO
----------------
A Pod needs to access an S3 bucket.

Without IRSA:
- You must give the node IAM Role S3 access, which means every Pod on that node also gets S3 access. Not secure.

With IRSA:
- Create an IAM Role that ONLY has "s3:GetObject" permissions.
- Attach it to the Pod using a ServiceAccount annotation.
- Only that Pod gets S3 permissions. No other Pod gets those permissions.

================================================


DATE: 23 JUNE 26

AWS CLOUD BASICS

AWS --> CLOUD 

datacenter

region 
av-zone ( NV - US-EAST-1 (1a,1b,1c,1d,1e,1f) 

Amazon Elastic Compute Cloud (EC2)

ELB 

ASG 

vpc (cidr - 10.81.0.0/16 ) ---> subnet --  public subnet , pvt subnet , igw , route table , nat gateway

Website ( Domain name)  ---> LB / Cloud Front (CDN)

route 53 --> dns service by AWS 

EBS (BLOCK)

EFS (FILE)

S3 BUCKET (OBJECT)

DATABASE : 

DB AS A SERVICE ( RDS )


IAM ( USERS , GROUPS, POLICIES  )

IAM ROLE 

KUBERNETS ( CLUSTER AS A SERVICE - EKS )
=====================================================

DATE: 24 JUNE 2026

K8S VOLUME PROVISIONING 

devopshubg333/batch17d:httpd

==============================================================================

KUBERNETS VOLUMES :

In Kubernetes, pods are temporary (ephemeral) by design. This means that any data stored inside a pod will be lost if the pod is deleted, restarted, or recreated — for example, during scaling or node failures.

To prevent data loss and keep your data available even when pods change, Kubernetes provides a feature called Volume Provisioning. This lets you attach external storage to your pods so that the data stays safe and accessible.

Two main ways to provision volumes in Kubernetes:

    Static Provisioning – You manually create a Persistent Volume (PV) ahead of time, and then link it to your pod using a Persistent Volume Claim (PVC).
    
    Dynamic Provisioning – Kubernetes automatically creates and manages storage when your pod requests it, using a predefined StorageClass.

In AWS, you can use two common storage options for persistent data:

   Amazon EBS (Elastic Block Store) – Great for single-pod, block-level storage with high performance.   
   Amazon EFS (Elastic File System) – Ideal for shared file storage that multiple pods can access at the same time.


Static Provisioning using EBS: 

In Static Provisioning, the storage volume is created manually before it is used by a pod. The Kubernetes administrator creates an EBS volume in AWS, and then defines a PersistentVolume (PV) that references it. Pods can then claim and use this storage through a PersistentVolumeClaim (PVC).

STEPS:

create a volume (ebs)


1. Install the Amazon EBS CSI Driver
     The EBS CSI driver is required in modern clusters. It handles attaching/detaching EBS volumes to pods.
	 
	 kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=release-1.51"
	 
2. Modify IAM ROLE & attach a policy for EBSCSI Driver permission	 
	 
2. Create EBS volume (Availability Zone (AZ) — it must match the AZ of your Kubernetes worker node ) 
3. Create a PersistentVolume (PV)
     Define a PV referencing the EBS volume using the CSI driver:
4. Create a PersistentVolumeClaim (PVC)
5. Use the PVC in a Pod
6. Verification & Testing ( Create a test file inside the pod ) 
7. Delete the pod:
8. Recreate the pod (using the same PVC) and verify the file persists:
9. This proves data persistence across pod restarts.

STATIC PROVISIOING :


kid     --> mother --> father --> wallet 
POD/APP       PVC          PV          EBS

 pod                       sc  ----> ebs 

VOLUMES , 
Persistant Volume (PV) 
Persistant Volume  Claim 

Installing EBS CSI Driver :

kubectl apply -k "github.com/kubernetes-sigs/aws-ebs-csi-driver/deploy/kubernetes/overlays/stable/?ref=release-1.51"


=====================================================
DYNAMIC PROV 



Dynamic Provisioning on EKS using Amazon EBS (with CSI Driver)

Dynamic provisioning allows Kubernetes to automatically create and manage EBS volumes when a pod requests storage. Unlike static provisioning, you do not manually create PVs; Kubernetes handles volume creation using a StorageClass and the EBS CSI driver.



  1.Create a StorageClass
  2. Create a PersistentVolumeClaim (PVC)
  3. Use the PVC in a Pod
  4. Apply the manifest
  5. Write a file in the pod:
  6. Delete the pod:
  7. Recreate the pod:
  
Dynamic provisioning is recommended for production environments because it reduces manual effort and prevents AZ mismatches.  

==================================

DATE: 26 JUNE 26

Kubernetes Volume Provisioning on EKS using Amazon EFS

Amazon EFS is a fully-managed, network-attached, shared file system that can be mounted by multiple pods across multiple nodes simultaneously. This makes it ideal for workloads requiring shared access, like web servers, content management systems, or big data pipelines.


 - 1.  Install the Amazon EFS CSI Driver 
 
kubectl apply -k github.com/kubernetes-sigs/aws-efs-csi-driver/deploy/kubernetes/overlays/stable/ecr/?ref=release-2.1

kubectl apply -k 
    "github.com/kubernetes-sigs/aws-efs-csi-driver/deploy/kubernetes/overlays/stable/ecr/?ref=release-2.1" 


 - 2.  Attach AmazonEFSCSIDriverPolicy / Permission in IAM roe 
 
 - Create an Amazon EFS File System ( same VPC where your EKS cluster resides.) 
 
 - Configure security groups to allow NFS traffic (TCP 2049) from your worker nodes.
   (  Unlike EBS, EFS is network-attached and does not require AZ matching. ) 
 
 
 -Create a PersistentVolume (PV)
 -Create a PersistentVolumeClaim (PVC)
 -Use PVC in a Pod
 - LB SERVICE 
 - verify 
 ============================================================
 Dynamic Provisioning with EFS

Dynamic provisioning allows Kubernetes to create subdirectories in EFS automatically when a PVC is requested.



 - Create a StorageClass for EFS
 - Create a PVC Using the StorageClass
 - Use PVC in a Pod  , svc
 - VERIFY 




DRIVER INSTALL : 



kubectl apply -k github.com/kubernetes-sigs/aws-efs-csi-driver/deploy/kubernetes/overlays/stable/ecr/?ref=release-2.1




==============================================
DATE: 29 JUNE 26 


Kubernetes StatefulSets: 

A StatefulSet is a Kubernetes controller used to manage stateful applications — workloads that require stable, unique network identifiers and persistent storage.



In Kubernetes, most workloads (like Deployments) are stateless, meaning:

   Each pod is identical.   
   It doesn’t matter which pod serves your request.   
   You can delete, recreate, or scale pods freely — no data or identity is lost.

Stateful means the application has memory or identity that must persist, such as:

    A database that stores data on disk. 
    A message broker (like Kafka, RabbitMQ) that keeps message queues. 
    Any system that requires stable storage or a unique network identity
	
	
Let’s say you’re running a 3-node mysql/MongoDB cluster:

   You can’t just start all pods at once. The primary must start first.
   Each pod must keep its own data volume (so the database doesn’t lose its data).
   Each pod must have a stable name (mongo-0, mongo-1, mongo-2) to connect to each other.

so statefulsets provides :

A StatefulSet is used when your application needs:

✅ Stable pod names (not random) - Stable network identity
✅ Persistent storage for each pod
✅ Ordered creation and deletion of pods ,  Ordered deployment & scaling
   
  
=======================

1. Create a Headless Service: 
---- This service ensures each pod gets a stable DNS entry like:


2. Create the StatefulSet



==========================================================================================================


AUTO SCALING :
Auto Scaling means automatically adjusting resources (like servers, pods, or CPUs) based on the load on your system.

       If your app gets busy, it adds more resources (scales up).       
       If traffic is low, it removes resources (scales down).
	   Keep performance good without wasting resources.


Horizontal Auto Scaling?  (scaling out or in )  ----> by adding or removing instances /PODS 

Vertical Auto Scaling? (scaling up or down)   ---> by increasing or decreasing the resources (CPU/RAM) of the same Instance/ Pod.



HPA: (HORIZONTAL POD AUTOSCALAR) 
metric server:
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/high-availability-1.21+.yaml



 Run this in a separate terminal
# so that the load generation continues and you can carry on with the rest of the steps
kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://php-apache; done"




======================================
Daemonset:

-  A DaemonSet ensures that a copy of a specific Pod runs on every (or selected) node in your Kubernetes cluster.

- So while a Deployment scales pods across nodes, a DaemonSet ensures one (or a few) specific pods exist per node — typically for background system-level workloads.

- A DaemonSet runs one pod per node — automatically managing cluster-wide agents like log collectors, monitoring daemons, and security scanners


DaemonSet_Use_Cases:

  NodeMonitoring:

      Deploys node-level monitoring agents to collect CPU, memory, disk, and network metrics from each node.
      These metrics are typically scraped by Prometheus or sent to monitoring platforms like Datadog.
	  
    Examples:
      - Prometheus Node Exporter
      - Datadog Agent

  LogCollection:	
  
      Collects and forwards logs from containers and system daemons running on each node.
      Ensures centralized log aggregation and analytics in tools like Elasticsearch, Loki, or Splunk.
	  
    Examples:
      - Fluentd
      - Filebeat
      - Vector

  SecurityScanning:

      Runs continuous or scheduled node-level security scanning agents to detect vulnerabilities, malware,
      and suspicious runtime behavior across nodes and containers.
	  
    Examples:
      - Falco
      - Aqua Trivy Daemon



  Networking:

      Installs and manages Container Network Interface (CNI) plugins that provide pod networking,
      network policies, and service routing functionality across all nodes.
	  
    Examples:
      - Calico
      - Cilium
      - Weave Net

  Storage:

      Deploys storage interface drivers (CSI drivers) to enable dynamic or static provisioning of persistent
      volumes for pods, ensuring consistent storage management across nodes.
	  
    Examples:
      - Amazon EBS CSI Driver
      - Amazon EFS CSI Driver
      - NFS Driver

  SystemMetrics:

      Collects detailed node health and system metrics, such as kernel issues, node conditions,
      hardware utilization, and GPU metrics for high-performance workloads.
    Examples:
      - Node Problem Detector
      - nvidia-device-plugin



  
Concept	Memory Hook
Deployment  --  I just want X replicas of my app, doesn’t matter which node or order.
StatefulSet	--  want pods with sticky identity and persistent storage, in order.
DaemonSet	 -- I want one pod on every node, always.


=======================================================================================


PHASE 2 --  21 JULY 2026 


HELM :

HELM


========================================
HELM FOR KUBERNETES: 
========================================

1. WHAT IS HELM?
----------------
Helm is the package manager for Kubernetes.
It helps you package, share, and deploy complex applications easily.

Think of it like:
- apt or yum for Kubernetes.
- Instead of applying many YAML files manually, you use one simple command.

Example:
helm install myapp ./mychart

This command deploys your entire application (Deployments, Services, Configs, etc.) in one go.


2. WHY USE HELM?
----------------
Without Helm:
- You manage multiple YAML files manually.
- Upgrades and rollbacks are manual and error-prone.
- Hard to reuse the same setup for multiple environments.

With Helm:
- Combines all YAMLs into one “Chart”.
- Configuration is handled via “values.yaml”.
- Rollbacks and upgrades are easy.
- Enforces standard folder structure and reusability.


3. KEY HELM CONCEPTS
--------------------
Chart          → A package of Kubernetes YAML files.
Release        → A deployed instance of a Chart.
Repository     → A collection of Charts (like DockerHub for Helm).
values.yaml    → Configuration file that defines variables and parameters.
templates/     → Folder containing Kubernetes YAML templates.
Chart.yaml     → Metadata about the Chart (name, version, description).


4. INSTALLING HELM
------------------
Linux/macOS:
  curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

Windows (using Chocolatey):
  choco install kubernetes-helm

Verify installation:
  helm version


5. STRUCTURE: 
-------------------------------
Command:
  helm create myapp

This creates a directory:
  myapp/
  ├── Chart.yaml
  ├── values.yaml
  ├── charts/
  └── templates/


6. UNDERSTANDING STRUCTURE
--------------------------
Chart.yaml        - Metadata about the chart.
values.yaml       - Central configuration file.
templates/        - Kubernetes resource templates (Deployment, Service, etc.).
_helpers.tpl      - Template helpers for naming and labels.



charts.yaml
https://helm.sh/docs/topics/charts





7. CREATING A SAMPLE HELM CHART

helm install fb-app .
helm upgrade fb-app
helm histroy fb-app
helm template .
helm rollback fb-app 1
helm uninstall fb-app


-------------------------------------------


DATE: 23 JULY 2026 


# Helm Practice Guide - Using the Bitnami NGINX Chart

---

## Verify Helm Installation

```bash
helm version
```

**Explanation:** Displays the installed Helm client version.

---

## Add the Bitnami Helm Repository

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
```

**Explanation:** Registers the Bitnami chart repository with Helm.

---

## Update Repository Metadata

```bash
helm repo update
```

**Explanation:** Downloads the latest chart information from all configured repositories.

---

## List Configured Repositories

```bash
helm repo list
```

**Explanation:** Displays all Helm repositories configured on your system.

Example Output:

```text
NAME      URL
bitnami   https://charts.bitnami.com/bitnami
```

---

## Search for the NGINX Chart

```bash
helm search repo nginx
```

or

```bash
helm search repo bitnami/nginx
```

**Explanation:** Searches the local repository cache for available NGINX charts.

---

## Display Chart Information

```bash
helm show chart bitnami/nginx
```

**Explanation:** Shows chart metadata such as name, version, description, and application version.

---

## View Default Configuration

```bash
helm show values bitnami/nginx
```

**Explanation:** Displays the default configuration values used by the chart.

---

## Save Default Values

```bash
helm show values bitnami/nginx > values.yaml
```

**Explanation:** Exports the default configuration into a local `values.yaml` file for customization.

---

## Install the Chart

```bash
helm install my-nginx bitnami/nginx
```

**Explanation:** Deploys the Bitnami NGINX chart with default settings.

---

## Verify the Installation

```bash
helm list
```

**Explanation:** Lists all installed Helm releases.

```bash
kubectl get pods
kubectl get svc
kubectl get deployments
```

**Explanation:** Displays the Pods, Services, and Deployments created by the Helm release.

---

## Check Release Status

```bash
helm status my-nginx
```

**Explanation:** Displays the current status and details of the Helm release.

---

## View Installed Values

```bash
helm get values my-nginx
```

**Explanation:** Shows the user-supplied configuration values used during installation.

Display all values:

```bash
helm get values my-nginx --all
```
-----------------------------------------

## Preview Templates Without Installing

```bash
helm template my-nginx bitnami/nginx
```

**Explanation:** Renders Kubernetes manifests locally without creating any resources.

Save the rendered manifests:

```bash
helm template my-nginx bitnami/nginx > manifests.yaml
```

----------------

## Install with Custom Values

Create a custom `values.yaml` file:

```yaml
replicaCount: 2

service:
  type: NodePort
```

Install using the custom file:

```bash
helm install my-nginx bitnami/nginx -f values.yaml
```

**Explanation:** Installs the chart using customized configuration values.

---

## Upgrade the Release

Increase the number of replicas:

```bash
helm upgrade my-nginx bitnami/nginx --set replicaCount=3
```

**Explanation:** Updates the existing release with new configuration values.

Upgrade using a values file:

```bash
helm upgrade my-nginx bitnami/nginx -f values.yaml
```

**Explanation:** Applies configuration changes from a values file.

---

## View Release History

```bash
helm history my-nginx
```

**Explanation:** Displays all revisions of the Helm release.

---

## Roll Back to a Previous Revision

```bash
helm rollback my-nginx 1
```

**Explanation:** Restores the release to revision 1.

---

## Validate the Chart

```bash
helm lint bitnami/nginx
```

**Explanation:** Performs a basic validation of the chart syntax and best practices.

---

## Uninstall the Release

```bash
helm uninstall my-nginx
```

**Explanation:** Removes the Helm release and all Kubernetes resources created by it.

---

## Verify Cleanup

```bash
helm list
kubectl get pods
kubectl get svc
```

**Explanation:** Confirms that the release and associated resources have been removed.

---

# Quick Reference Commands

```bash
helm version

helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm repo list

helm search repo bitnami/nginx

helm show chart bitnami/nginx
helm show values bitnami/nginx

helm install my-nginx bitnami/nginx

helm list
helm status my-nginx

helm get values my-nginx
helm get values my-nginx --all
helm get manifest my-nginx

helm template my-nginx bitnami/nginx
helm install my-nginx bitnami/nginx --dry-run --debug

helm upgrade my-nginx bitnami/nginx --set replicaCount=3

helm history my-nginx
helm rollback my-nginx 1

helm uninstall my-nginx
```

===============================================================================

# Helm Repository Creation

## Create a Working Directory

```bash
mkdir helm-practice
cd helm-practice
```

**Explanation:** Creates a new directory where all Helm charts and repository files will be stored.

---

## Create Two Sample Charts

```bash
helm create nginx
helm create apache
```

**Explanation:** Generates two sample Helm charts with the default directory structure. 
(like ansible galaxy crate roles directory structure, you may co relate ) 

---

## View the Generated Chart Structure

```bash
tree nginx
tree apache
```

**Explanation:** Displays the folder structure of each chart.

If the `tree` command is unavailable, use:

```bash
ls -R nginx
ls -R apache
```

---

## Validate the Charts

```bash
helm lint nginx
helm lint apache
```

**Explanation:** Checks the charts for syntax errors and Helm best practices.

---

## Preview Kubernetes Manifests

```bash
helm template nginx ./nginx
helm template apache ./apache
```

**Explanation:** Renders Kubernetes manifests locally without installing the charts.

---

## Install Charts Locally

```bash
helm install my-nginx ./nginx
helm install my-apache ./apache
```

**Explanation:** Installs both charts into your Kubernetes cluster.

---

## Verify the Installation

```bash
helm list
kubectl get pods
kubectl get svc
kubectl get deployments
```

**Explanation:** Lists installed Helm releases and the Kubernetes resources created by the charts.

---

## Package the Charts

```bash
helm package nginx
helm package apache
```

**Explanation:** Packages each chart into a compressed `.tgz` archive.

Example output:

```text
nginx-0.1.0.tgz
apache-0.1.0.tgz
```

---

## Create a Repository Folder

```bash
mkdir repo
mv *.tgz repo/
cd repo
```

**Explanation:** Creates a repository directory and moves the packaged charts into it.

---

## Generate the Repository Index

```bash
helm repo index .
```

**Explanation:** Generates the `index.yaml` file, which Helm uses to discover and download charts from the repository.

Repository contents:

```text
repo/
├── apache-0.1.0.tgz
├── nginx-0.1.0.tgz
└── index.yaml
```






















