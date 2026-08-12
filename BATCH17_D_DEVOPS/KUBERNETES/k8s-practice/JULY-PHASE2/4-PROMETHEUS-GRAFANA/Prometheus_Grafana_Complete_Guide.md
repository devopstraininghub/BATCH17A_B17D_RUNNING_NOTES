# Prometheus & Grafana — Complete Guide (Concepts + Hands-on Lab)

> Source video: https://youtu.be/ajSUr2LetoI

This guide explains **what Prometheus and Grafana are, why they're used together, how the pieces fit into a Kubernetes setup**, and then walks through a **full hands-on lab** to actually build the monitoring stack from scratch on AWS EC2 + EKS.

---

## Part 1 — Concepts

### 1.1 What is Prometheus?

Prometheus is a **monitoring and alerting tool**. Think of it as a robot that keeps knocking on the door of your applications and servers every few seconds, asking "how are you doing?" and writing down the answer with a timestamp.

It was built by SoundCloud in 2012 and is now one of the most widely used monitoring tools in the DevOps/Kubernetes world (it's a graduated project of the CNCF, the same foundation that governs Kubernetes).

**Why it matters:** without a tool like Prometheus, you'd have no systematic way of knowing if a server is running out of memory, a pod keeps crashing, or a disk is about to fill up — until a user complains.

**Core capabilities, explained simply:**

| Feature | What it actually means |
|---|---|
| **Time-Series Database (TSDB)** | Every metric Prometheus collects is stored as `(timestamp, value)` pairs. This is what lets you draw a graph of "CPU usage over the last 24 hours" instead of just seeing one snapshot. |
| **PromQL (query language)** | A special language to ask Prometheus questions like "what's the average CPU usage across all my servers in the last 5 minutes?" It's how you build graphs and alerts. |
| **Metrics Collection (scraping)** | Prometheus doesn't wait for data to be sent to it — it actively goes out and **pulls** metrics from each target at a fixed interval (e.g., every 15 seconds). This is called "scraping." |
| **Service Discovery** | Instead of manually listing every server to monitor, Prometheus can auto-detect new targets — for example, automatically finding every pod running in a Kubernetes cluster. |
| **Alerting** | You define rules like "alert me if CPU > 90% for 5 minutes." When true, Prometheus hands the alert off to a separate tool called **Alertmanager**, which decides where to send the notification. |
| **Basic Visualization** | Prometheus has a built-in web UI to run PromQL queries and see simple graphs — but it's not meant for building polished dashboards. That's Grafana's job. |

### 1.2 What is Grafana?

Grafana is a **visualization tool**. Prometheus is good at collecting and storing numbers; Grafana is good at turning those numbers into dashboards, graphs, and gauges that a human can glance at and instantly understand.

**Why you need both:** Prometheus alone gives you raw data and basic graphs. Grafana gives you the polished, shareable dashboards (like the "Node Exporter Full" screenshots later in this guide) that teams actually stare at during an incident.

**Core capabilities, explained simply:**

| Feature | What it actually means |
|---|---|
| **Multiple Data Sources** | Grafana isn't locked to Prometheus — it can also pull data from InfluxDB, Elasticsearch, MySQL, CloudWatch, and many others, all in the same dashboard. |
| **Custom Dashboards** | You build panels (graphs, gauges, tables) and arrange them however makes sense for your team. |
| **Rich Visualizations** | Line graphs, bar charts, heatmaps, gauges, single-stat panels, etc. |
| **Alerting** | Grafana can also trigger alerts and notify you via Slack, email, PagerDuty, etc. |
| **Plugins** | You can extend Grafana with community plugins for new data sources or panel types. |

### 1.3 How Prometheus and Grafana Work Together

Think of it as a simple pipeline:

```
 Targets (apps/servers/pods)
        │  metrics exposed on an HTTP endpoint (e.g. /metrics)
        ▼
   Prometheus  ──── scrapes/pulls data on a schedule
        │
        ▼
  Time-series database (stored on disk)
        │
        ▼
    Grafana  ──── queries Prometheus using PromQL
        │
        ▼
  Dashboards (what you actually look at)
```

1. **Data Collection** — Prometheus scrapes metrics from various sources.
2. **Storage** — Prometheus stores this data in its own time-series database.
3. **Visualization** — Grafana connects to Prometheus as a "data source" and pulls that data.
4. **Dashboards** — You (or the community) build dashboards in Grafana to visualize it.
5. **Alerting** — Either tool can raise alerts when thresholds are breached.

### 1.4 Why Use This Combination? (Benefits)

- **Scalability** — Works for a single laptop-scale project or a fleet of thousands of servers.
- **Flexibility** — Powerful query language + rich visualization options cover almost any monitoring need.
- **Community & Ecosystem** — Huge community means pre-built dashboards (you'll literally just import a dashboard ID — see Part 3), exporters for almost any technology, and lots of documentation.
- **Open Source** — Free to use, self-hostable, no vendor lock-in.

### 1.5 Architecture for Monitoring a Kubernetes Cluster

This is the "big picture" architecture used in the lab below:

```
   ┌───────────────┐   ┌───────────────┐   ┌───────────────┐
   │  K8s Cluster  │   │  K8s Cluster  │   │  K8s Cluster  │
   └───────┬───────┘   └───────┬───────┘   └───────┬───────┘
           │   pulling metrics  │   pulling metrics  │
           └──────────┬─────────┴──────────┬─────────┘
                       ▼                    
                 ┌───────────┐        ┌──────────────┐
                 │ Prometheus│───────▶│   Grafana    │
                 └─────┬─────┘        └──────────────┘
                       │
              ┌────────┴────────┐
              ▼                 ▼
       ┌─────────────┐   ┌───────────────────┐
       │ Persistent  │   │ Prometheus Alert   │
       │ Volume      │   │ Manager            │
       └─────────────┘   └─────────┬──────────┘
                                    │
                         ┌──────────┴──────────┐
                         ▼                     ▼
                     ┌───────┐            ┌────────┐
                     │ Slack │            │ Email  │
                     └───────┘            └────────┘
```

**Component breakdown:**

- **Kubernetes Clusters** — the actual environments being monitored. Prometheus reaches into them and pulls metrics.
- **Prometheus** — the central brain. Scrapes metrics on a schedule and stores them as a time series.
- **Persistent Volume** — storage attached to Prometheus so that if it restarts (crash, upgrade, redeploy), the historical metrics **aren't lost**. Without this, a restart would wipe all your history.
- **Grafana** — connects to Prometheus and turns the raw numbers into dashboards a human can read.
- **Prometheus Alert Manager** — a separate component that receives alerts *fired* by Prometheus (based on rules you define) and is responsible for routing/de-duplicating/sending those alerts to the right channel.
- **Slack / Email** — where the humans actually get notified. Alertmanager can be configured to send to either (or both, or other channels like PagerDuty/Opsgenie).

**Workflow, step by step:**

1. **Pulling Metrics** — Prometheus scrapes each Kubernetes cluster/target at a regular interval (e.g. every 15s).
2. **Storing Data** — Metrics go into Prometheus's built-in time-series database, backed by a persistent volume so data survives restarts.
3. **Visualization** — Grafana queries Prometheus and renders dashboards for engineers to explore.
4. **Alerting** — Prometheus evaluates alert rules continuously; when a condition is met (e.g. "pod CPU > 90% for 5 min"), it fires an alert to Alertmanager, which then notifies Slack/Email.

---

## Part 2 — Hands-on Lab

### 2.1 Lab Goal & Setup

We will build this from scratch using **2 AWS EC2 instances (Amazon Linux)**:

| Server | Purpose |
|---|---|
| **k8s-server** | Runs `eksctl` to create an EKS (Elastic Kubernetes Service) cluster on AWS |
| **monitoring-server** | Runs Prometheus + Grafana to monitor that cluster |

---

### 2.2 On the k8s-server: Configure AWS Access

Before you can create an EKS cluster, the server needs AWS credentials so it's authorized to call AWS APIs.

```bash
aws configure
```

You'll be prompted for:

```
AWS_ACCESS_KEY_ID=AKIA6GBMFJPZxxxxxxx
AWS_SECRET_ACCESS_KEY=oVQJav/VTBzqGwfYJxxxxxxxxxxx
region=us-east-1
```

> ⚠️ **Security note:** Never commit real AWS keys to a file or git repo. Treat access keys like passwords — the values above are placeholders/examples only.

---

### 2.3 Install `kubectl` (Kubernetes CLI)

`kubectl` is the command-line tool used to talk to any Kubernetes cluster (issue commands like "show me all pods," "apply this config," etc.).

Reference: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

**1. Download the latest version:**

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
```

**2. Install it:**

```bash
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

This copies the binary into `/usr/local/bin` (a folder already in your `$PATH`) and sets the right ownership/permissions so it's runnable system-wide.

---

### 2.4 Install `eksctl` and Create the EKS Cluster

`eksctl` is a CLI tool that automates the (otherwise tedious) process of spinning up an AWS EKS cluster — it handles the VPC, subnets, IAM roles, and worker nodes for you with a single command.

Reference: https://docs.aws.amazon.com/emr/latest/EMR-on-EKS-DevelopmentGuide/setting-up-eksctl.html

**1. Download and install `eksctl`:**

```bash
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```

**2. Create the cluster:**

```bash
eksctl create cluster test
```

This single command provisions an entire EKS cluster named `test` on AWS — networking, control plane, and worker nodes. It typically takes **10–20 minutes**. Once done, `kubectl` on this server is automatically configured to talk to the new cluster.

---

### 2.5 On the monitoring-server: Install Prometheus

**1. Download and extract:**

```bash
wget https://github.com/prometheus/prometheus/releases/download/v2.53.1/prometheus-2.53.1.linux-amd64.tar.gz
tar -xvf prometheus-2.53.1.linux-amd64.tar.gz
```

**2. Start it:**

```bash
cd prometheus-2.53.1.linux-amd64/
./prometheus &
```

The `&` runs it in the background so your terminal stays free. By default Prometheus listens on **port 9090**.

**3. Access it in a browser:**

```
http://<public_ip>:9090
```

> ⚠️ Make sure the EC2 security group allows inbound traffic on port 9090.

Once open, go to **Status → Targets** to see everything Prometheus is currently scraping and whether each one is healthy ("UP").

---

### 2.6 On the monitoring-server: Install Grafana

**1. Download, extract, and start:**

```bash
wget https://dl.grafana.com/enterprise/release/grafana-enterprise-11.1.0.linux-amd64.tar.gz
tar -xvf grafana-enterprise-11.1.0.linux-amd64.tar.gz
cd grafana-enterprise-11.1.0/
cd bin/
./grafana-server &
```

Grafana defaults to **port 3000**.

**2. Access it in a browser:**

```
http://<public_ip>:3000
```

Default login is usually `admin` / `admin` (you'll be prompted to change it on first login).

---

### 2.7 Node Exporter vs. kube-state-metrics — What's the Difference?

These are two different "exporters" — small programs that expose metrics in a format Prometheus can scrape. People often confuse them, so here's the distinction in plain terms:

| | **Node Exporter** | **kube-state-metrics** |
|---|---|---|
| **Monitors** | The physical/virtual **machine** itself | The **Kubernetes objects** running on top of it |
| **Example metrics** | CPU usage, RAM usage, disk space, network I/O, system load, temperature | Number of pods running, deployment replica counts, pod status (Pending/Running/Failed), node conditions |
| **Analogy** | Checking the health of the *engine* of a car | Checking the health of the *passengers* riding in it |

You typically need **both** for full visibility: Node Exporter tells you if the underlying server is struggling, while kube-state-metrics tells you if Kubernetes itself is unhealthy (e.g., a deployment stuck with 0/3 replicas ready).

---

### 2.8 Install Node Exporter

Install this on the **monitoring server AND both Kubernetes worker nodes** (so you get infrastructure metrics from every machine, not just one).

**1. Download and extract:**

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
tar -xvf node_exporter-1.7.0.linux-amd64.tar.gz
sudo mv node_exporter-1.7.0.linux-amd64/node_exporter /usr/local/bin/
```

**2. Create a dedicated system user to run it:**

```bash
sudo useradd --system --no-create-home --shell /bin/false node_exporter
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
sudo chmod +x /usr/local/bin/node_exporter
```

> **Why a separate user?** Running services as `root` is risky — if Node Exporter had a vulnerability, an attacker could gain full control of the machine. Instead, we create a locked-down user (`--shell /bin/false` means nobody can actually log in as this user) that only has permission to run this one binary. This is a standard security best-practice for any long-running service.

**3. Run it as a proper systemd service** (so it starts automatically on boot and restarts if it crashes):

```bash
sudo vim /etc/systemd/system/node_exporter.service
```

```ini
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target
StartLimitIntervalSec=500
StartLimitBurst=5

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=on-failure
RestartSec=5s
ExecStart=/usr/local/bin/node_exporter --collector.logind

[Install]
WantedBy=multi-user.target
```

What the key lines mean:
- `Restart=on-failure` + `RestartSec=5s` → if the process dies, systemd automatically restarts it after 5 seconds.
- `User=node_exporter` → runs as the safe, unprivileged user created above, not root.
- `ExecStart=... --collector.logind` → the actual command to launch, with the `logind` collector enabled (adds metrics about logged-in sessions).

**4. Enable and start it:**

```bash
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
systemctl status node_exporter.service
```

- `enable` → makes it start automatically on every server reboot.
- `start` → starts it right now.
- `status` → lets you confirm it's actually running (look for `active (running)`).

Node Exporter now exposes metrics on **port 9100**.

---

### 2.9 Configure Prometheus to Scrape Node Exporter

Prometheus needs to be told *where* to find each Node Exporter. This is done in `prometheus.yml`, its main configuration file.

**1. Open the config file:**

```bash
cd prometheus-2.53.1.linux-amd64/
vim prometheus.yml
```

**2. Add a scrape job for each target:**

```yaml
scrape_configs:
  - job_name: "prometheus"
    static_configs:
      - targets: ["localhost:9090"]

  - job_name: "node_exporter"
    static_configs:
      - targets: ["localhost:9100"]

  - job_name: "node_exporter-k8sworker-1"
    static_configs:
      - targets: ["3.81.25.36:9100"]     # public IP of k8s worker node 1

  - job_name: "node_exporter-k8sworker-2"
    static_configs:
      - targets: ["54.167.217.255:9100"] # public IP of k8s worker node 2
```

**How to read this:** each `job_name` is just a label you choose (it shows up in Grafana/Prometheus so you know which target a metric came from). Under it, `targets` lists `host:port` addresses that Prometheus will scrape every few seconds. So here we're telling Prometheus: "also go check the Node Exporter running on the monitoring server itself, and on both Kubernetes worker nodes."

**3. Restart Prometheus so it picks up the new config:**

```bash
pgrep prometheus          # or: ps -ef | grep prometheus
kill <pid>                # stop the currently running process
./prometheus &             # start it again with the updated config
```

> Note: Prometheus also supports reloading config without a full restart (`SIGHUP` or the `/-/reload` HTTP endpoint), but the kill-and-restart approach shown above is the simplest for a manual lab setup.

---

### 2.10 Install kube-state-metrics

**What it is:** a small service that runs **inside** the Kubernetes cluster, watches the Kubernetes API, and turns object state (pods, deployments, nodes, etc.) into Prometheus-readable metrics.

**1. Clone the repo and deploy the standard manifests:**

```bash
git clone https://github.com/kubernetes/kube-state-metrics.git
kubectl apply -f kube-state-metrics/examples/standard
```

This creates the kube-state-metrics Deployment, Service, and the RBAC permissions it needs to read cluster state, inside the `kube-system` namespace.

**2. Verify it's running:**

```bash
kubectl get all -n kube-system
```

**3. Expose it so Prometheus (running outside the cluster, on the monitoring server) can reach it:**

```bash
kubectl expose service kube-state-metrics --type=NodePort --name kube-state-metrics-ext -n kube-system
```

By default, kube-state-metrics only has a **ClusterIP** service — reachable only from *inside* the cluster. Since our Prometheus lives on a separate EC2 instance outside the cluster, we expose it via **NodePort**, which opens a port on every worker node's public/private IP that forwards traffic into the service. Run `kubectl get svc -n kube-system` to find the assigned NodePort number.

**4. Add it to Prometheus's scrape config:**

```yaml
  - job_name: "kube-state-metrics"
    static_configs:
      - targets: ["public_ip:nodeport"]
```

Then restart Prometheus the same way as before (`pgrep` → `kill` → `./prometheus &`).

---

### 2.11 Import Pre-Built Grafana Dashboards

Instead of building dashboards from scratch, Grafana lets you **import** dashboards shared by the community using just a numeric ID. In Grafana: **Dashboards → New → Import**, then paste the ID.

| Dashboard ID | What it shows |
|---|---|
| **1860** | Node Exporter Full — detailed CPU/memory/disk/network for a single machine |
| **6417** | Kubernetes Cluster (Prometheus) — cluster-wide overview |
| **15451** | Kubernetes / Compute Resources / Node (Groups) — resource usage grouped by node |
| **9964** | Jenkins dashboard — if you're also monitoring a Jenkins server |
| **13105** | K8S Dashboard — general Kubernetes dashboard |

When importing, Grafana will ask you to pick the **data source** (choose your Prometheus instance) — after that, the dashboard populates automatically using the metrics already being scraped.

---

### 2.12 What You Should See at the End

Once everything above is wired up correctly:

- **Prometheus (`:9090`) → Status → Targets** shows all jobs (`prometheus`, `node_exporter`, `node_exporter-k8sworker-1/2`, `kube-state-metrics`) as **UP**.
- **Grafana (`:3000`)** shows live dashboards like "Node Exporter Full" with real-time CPU %, RAM %, disk usage gauges, and time-series graphs for each server.
- The **Kubernetes / Compute Resources / Node (Groups)** dashboard shows cluster-wide numbers: total nodes, total pods, and their running/pending/failed states.

At this point you have a working, self-hosted monitoring stack: Node Exporter and kube-state-metrics generate the raw numbers → Prometheus pulls and stores them → Grafana turns them into dashboards you can actually use to spot problems.

---

## Quick Reference — Ports Used

| Component | Default Port |
|---|---|
| Prometheus | 9090 |
| Grafana | 3000 |
| Node Exporter | 9100 |
| kube-state-metrics (internal) | 8080 |
| kube-state-metrics (NodePort, external) | assigned dynamically (check `kubectl get svc`) |
