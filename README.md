# Liferay on Kubernetes – Production-Oriented Platform

This repository contains a production-oriented Kubernetes platform running **Liferay DXP** with **PostgreSQL**, exposed through **NGINX Ingress** and fully observable using **Prometheus and Grafana**.

The goal of this project is to demonstrate real-world DevOps / Platform Engineering practices applied to a Java enterprise application.

---

## 🎯 Objectives

- Deploy Liferay DXP on Kubernetes
- Use Infrastructure as Code (IaC)
- Expose applications using NGINX Ingress
- Add full observability (metrics & dashboards)
- Follow production-oriented best practices
- Provide a clean, reproducible platform

---

## 🏗 Architecture Overview

- Kubernetes cluster (1 control-plane, 1 worker)
- Liferay DXP 7.4
- PostgreSQL database
- NGINX Ingress Controller
- Prometheus + Grafana (kube-prometheus-stack)
- Persistent Volumes for stateful components

Traffic flow:

User
↓
NGINX Ingress
↓
Liferay Service
↓
Liferay Pod
↓
PostgreSQL

Observability:

Kubernetes / Ingress / Pods
↓
Prometheus
↓
Grafana


---

## 🌐 Exposed Services

| Service   | Access Method | Example URL |
|----------|--------------|-------------|
| Liferay  | Ingress (HTTP) | http://liferay.local |
| Grafana  | Ingress (HTTP) | http://grafana.local |

> In bare-metal environments, services are accessed through the Ingress NodePort.

---

## 📊 Observability

This platform includes a full observability stack based on **kube-prometheus-stack**:

- Prometheus for metrics collection
- Grafana for visualization
- Alertmanager (ready for alerting)
- Node Exporter
- kube-state-metrics
- NGINX Ingress metrics

Metrics observed:
- Node CPU / memory usage
- Pod resource consumption
- Liferay application resource usage
- HTTP latency and error rates via Ingress
- Cluster health

---

## 📁 Project Structure

```text
.
├── k8s/
│   ├── ingress/        # Ingress resources (Liferay, Grafana)
│   ├── liferay/        # Liferay deployment and configuration
│   ├── monitoring/     # Monitoring namespace
│   └── base/           # Base Kubernetes resources
├── cicd/               # CI/CD pipelines (GitHub Actions)
├── docs/               # Architecture and documentation
├── scripts/            # Helper scripts
└── README.md

🚀 Current Status

Phase 1 – Platform Bootstrap (Completed)

Kubernetes cluster ready

Liferay deployed and connected to PostgreSQL

NGINX Ingress configured

Prometheus & Grafana installed

Grafana exposed through Ingress

🔜 Next Steps

JVM metrics via JMX Exporter

Alerting rules (CPU, memory, latency)

TLS with cert-manager

PostgreSQL exporter

CI/CD deployment pipeline

Architecture diagrams

🧠 Notes

This project focuses on realistic infrastructure patterns rather than simplified demos.
All components are deployed and integrated as they would be in a real on-prem or cloud environment.
