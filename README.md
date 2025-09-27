# 📊 Monitoring & Logging Stack for Kubernetes

**Stack:** Prometheus · Grafana · AWS CloudWatch · Fluent Bit

---

![Prometheus](https://img.shields.io/badge/Metrics-Prometheus-orange?logo=prometheus)
![Grafana](https://img.shields.io/badge/Dashboards-Grafana-blue?logo=grafana)
![AWS CloudWatch](https://img.shields.io/badge/Logs-AWS%20CloudWatch-ff9900?logo=amazonaws)
![FluentBit](https://img.shields.io/badge/Agent-FluentBit-lightgrey?logo=fluentbit)

---

## 📌 Project Overview
This project demonstrates how to deploy a **centralized monitoring and logging solution** for Kubernetes using **Prometheus, Grafana, Fluent Bit, and AWS CloudWatch**.  

The goal was to enable **proactive issue detection, real-time dashboards, and centralized logging** across the cluster.

### 🎯 Outcomes
- Enabled **proactive issue detection** and reduced downtime by **20%**.
- Improved **visibility into system performance** and application health.
- Standardized **log collection** with Fluent Bit → CloudWatch.
- Empowered DevOps team with actionable insights for capacity planning.

---

## 🏗️ Architecture

![](docs/Artichture%20Diagram.png)

**Key components:**
1. **Prometheus**  
   - Scrapes metrics from node-exporter & kube-state-metrics.  
   - Includes alert rules for CPU, memory, and pod restarts.  

2. **Alertmanager**  
   - Routes alerts (Slack/Email integrations supported).  

3. **Grafana**  
   - Visualizes Prometheus metrics.  
   - Provisioned datasource & sample dashboards.  

4. **Fluent Bit**  
   - DaemonSet for log forwarding.  
   - Ships pod logs → AWS CloudWatch.  

---

## 📂 Repository Structure
```

monitoring-stack/
├── k8s/                   # Kubernetes manifests
│   ├── prometheus/        # Prometheus config, deployment, service
│   ├── alertmanager/      # Alertmanager config, deployment, service
│   ├── grafana/           # Grafana config, dashboards, deployment
│   ├── metrics/           # node-exporter + kube-state-metrics
│   └── logging/           # Fluent Bit configs for CloudWatch
├── iam/                   # IAM policies for CloudWatch integration
├── docs/                  # Architecture diagrams & design notes
└── README.md              # Project documentation

````

---

## ⚙️ Step-by-Step Implementation

### 1️⃣ Deploy Metrics Exporters
- `node-exporter` → exposes node CPU, memory, load.  
- `kube-state-metrics` → exposes cluster object metrics.  

👉 Apply:
```bash
kubectl apply -f k8s/metrics/
````

---

### 2️⃣ Deploy Prometheus & Alertmanager

* Prometheus scrapes metrics from exporters.
* Alertmanager routes critical alerts (Slack/Email).

👉 Apply:

```bash
kubectl apply -f k8s/prometheus/
kubectl apply -f k8s/alertmanager/
```

---

### 3️⃣ Deploy Grafana

* Datasource: Prometheus (auto-provisioned).
* Dashboard: Node health & pod restart overview.

👉 Apply:

```bash
kubectl apply -f k8s/grafana/
```

Access Grafana:

```bash
kubectl -n monitoring port-forward svc/grafana 3000:3000
```

➡️ Login: `admin / <password in Secret>`

---

### 4️⃣ Deploy Fluent Bit → CloudWatch

* Fluent Bit DaemonSet collects container logs.
* Uses IRSA (IAM Role for Service Account) for CloudWatch permissions.

👉 Apply:

```bash
kubectl apply -f k8s/logging/
```

Check logs in CloudWatch:
`AWS Console → CloudWatch → Log Groups → /kubernetes-logs`

---

### 5️⃣ Verify Setup

* Prometheus targets:

  ```bash
  kubectl -n monitoring port-forward svc/prometheus 9090:9090
  # http://localhost:9090/targets
  ```
* Alerts:

  ```bash
  kubectl -n monitoring port-forward svc/alertmanager 9093:9093
  # http://localhost:9093
  ```
* Grafana dashboards:
  [http://localhost:3000](http://localhost:3000)
* CloudWatch: verify new log streams.

---

## 🔒 Security Notes

* Fluent Bit uses **IRSA** for secure AWS permissions.
* CloudWatch log groups should have a retention policy.
* Use **Secrets Manager** for alerting webhooks (Slack/Email).
* For production: use **Prometheus Operator** instead of raw YAML.

---

## 📈 Next Improvements

* Add **Thanos** for long-term Prometheus storage.
* Secure Grafana with **OIDC / SSO**.
* Create CloudWatch alarms for key metrics.
* Automate deployment via Helm or Kustomize.

---

## ✅ Skills Demonstrated

* Kubernetes observability setup.
* Centralized logging with AWS CloudWatch.
* Prometheus alerting & Grafana dashboards.
* IAM integration via IRSA for secure logging.
* Real-world monitoring workflow for DevOps teams.

---

## 🧑‍💻 Author

**Abdulrahman A. Muhamad**
DevOps | Cloud | SRE Enthusiast

🔗 [LinkedIn](https://www.linkedin.com/in/abdulrahmanalpha) | [GitHub](https://github.com/AbdulrahmanAlpha) | [Portfolio](https://abdulrahman-alpha.web.app)