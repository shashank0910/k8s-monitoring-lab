# Kubernetes Monitoring Lab

Deploying a complete Kubernetes monitoring stack using **Prometheus**, **Grafana**, **Alertmanager**, and **Node Exporter** on a highly available Kubernetes cluster created with Kind.

## Architecture

```
                     +----------------------+
                     |      Grafana         |
                     +----------+-----------+
                                |
                     +----------v-----------+
                     |     Prometheus       |
                     +----------+-----------+
                                |
              +-----------------+-----------------+
              |                                   |
      +-------v--------+                 +--------v--------+
      | Node Exporter  |                 | Alertmanager    |
      +-------+--------+                 +--------+--------+
              |                                   |
      +-------v-----------------------------------v------+
      |            Kubernetes HA Cluster                 |
      | 3 Control Plane Nodes | 2 Worker Nodes          |
      +-----------------------------------------------+
```

---

## Technologies Used

- Kubernetes (Kind HA Cluster)
- Prometheus
- Grafana
- Alertmanager
- Node Exporter
- Helm
- Prometheus Operator

---

## Lab Objectives

- Deploy kube-prometheus-stack using Helm
- Monitor Kubernetes cluster health
- Visualize CPU, Memory, Disk and Network metrics
- Configure Prometheus alert rules
- Generate CPU load using stress containers
- Validate monitoring dashboards and alerts

---

## Installation

### Add Helm Repository

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

### Create Namespace

```bash
kubectl create namespace monitoring
```

### Install Monitoring Stack

```bash
helm install monitoring prometheus-community/kube-prometheus-stack \
-n monitoring
```

---

## Verify Installation

```bash
kubectl get pods -n monitoring
kubectl get svc -n monitoring
helm list -n monitoring
```

---

## Access Grafana

```bash
kubectl port-forward svc/monitoring-grafana 3000:80 -n monitoring
```

Open

```
http://localhost:3000
```

Get admin password

```bash
kubectl get secret monitoring-grafana -n monitoring \
-o jsonpath="{.data.admin-password}" | base64 -d
```

---

## Access Prometheus

```bash
kubectl port-forward svc/monitoring-kube-prometheus-prometheus \
9090:9090 -n monitoring
```

Open

```
http://localhost:9090
```

---

## Custom Alert Rule

Location

```
alerts/high-cpu-rule.yaml
```

Alert triggers when CPU utilization exceeds 20%.

---

## CPU Stress Test

Deploy CPU stress workload

```bash
kubectl apply -f manifests/stress-deployment.yaml
```

Verify

```bash
kubectl get pods
```

Observe increased CPU utilization in Grafana dashboards.

---

## Repository Structure

```
k8s-monitoring-lab/
│
├── alerts/
│   └── high-cpu-rule.yaml
│
├── manifests/
│   ├── nginx-deployment.yaml
│   └── stress-deployment.yaml
│
├── results/
│   ├── cluster-nodes.txt
│   ├── monitoring-pods.txt
│   ├── monitoring-services.txt
│   ├── prometheus-rules.txt
│   ├── stress-pods.txt
│   └── services.txt
```

---

## Results

✔ Prometheus deployed successfully

✔ Grafana dashboards operational

✔ Alertmanager configured

✔ Node Exporter collecting metrics

✔ CPU stress workload generated high CPU utilization

✔ Kubernetes nodes successfully monitored

---

## Screenshots

- Grafana Dashboard
- Prometheus Targets
- Alertmanager
- CPU Stress Test
- Kubernetes Monitoring Dashboard

---

## Author

**Shashank Gajbhiye**

GitHub: https://github.com/shashank0910
