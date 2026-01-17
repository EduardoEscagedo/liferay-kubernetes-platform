# Liferay on Kubernetes – Production-Oriented Platform

This repository contains a production-oriented Kubernetes platform running **Liferay DXP** with **PostgreSQL**, exposed through **NGINX Ingress** and fully observable using **Prometheus and Grafana**.

The goal of this project is to demonstrate real-world **DevOps / Platform Engineering** practices applied to a Java enterprise application, following patterns commonly used in on-premise and cloud-native environments.

---

## Objectives

- Deploy Liferay DXP on Kubernetes
- Use Infrastructure as Code (IaC)
- Expose applications using NGINX Ingress
- Integrate an external Elasticsearch cluster
- Add full observability (metrics & dashboards)
- Follow production-oriented best practices
- Provide a clean, reproducible platform

---

## Architecture Overview

- Kubernetes cluster (1 control-plane, 1 worker)
- Liferay DXP 7.4
- PostgreSQL database
- External Elasticsearch cluster
- NGINX Ingress Controller
- Prometheus + Grafana (kube-prometheus-stack)
- Persistent Volumes for stateful components

### Traffic flow

User  
↓  
NGINX Ingress  
↓  
Liferay Service  
↓  
Liferay Pod  
↓  
PostgreSQL  

### Search flow

Liferay  
↓  
External Elasticsearch cluster  

### Observability flow

Kubernetes / Ingress / Pods  
↓  
Prometheus  
↓  
Grafana  

---

## Exposed Services

| Service   | Access Method | Example URL |
|----------|--------------|-------------|
| Liferay  | Ingress (HTTP) | http://liferay.local |
| Grafana  | Ingress (HTTP) | http://grafana.local |

> In bare-metal environments, services are accessed through the Ingress NodePort.

---

## Elasticsearch Integration

An external Elasticsearch cluster is deployed inside Kubernetes and configured for **remote operation** with Liferay.

Liferay is configured to use Elasticsearch in `REMOTE` mode through OSGi configuration.  
Index creation and reindexing are performed explicitly from Liferay and verified directly in the external Elasticsearch cluster.

> Note:  
> The official Liferay DXP Docker image starts an internal Elasticsearch sidecar process during container initialization for development convenience.  
> Despite this behavior, Liferay operates using the external Elasticsearch cluster for indexing and search operations once properly configured and reindexed.

---

## Observability

This platform includes a full observability stack based on **kube-prometheus-stack**, installed and managed via **Helm**, reflecting real-world operational practices.

Components included:

- Prometheus (metrics collection)
- Grafana (visualization)
- Alertmanager (alerting framework)
- Node Exporter
- kube-state-metrics
- NGINX Ingress metrics

Metrics observed:

- Node CPU and memory usage
- Pod resource consumption
- Liferay application resource usage
- HTTP latency and error rates via Ingress
- Overall cluster health

---

## Monitoring Installation

The monitoring stack is installed using Helm rather than custom Kubernetes manifests.

Example installation:


helm install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring --create-namespace


This approach avoids duplicating complex YAML resources in the repository and aligns with how monitoring stacks are typically managed in production.

Ingress resources for Grafana access are defined separately under `k8s/ingress/`.

---

## Current Status

**Phase 1 – Platform Bootstrap (Completed)**

- Kubernetes cluster ready
- Liferay deployed and connected to PostgreSQL
- External Elasticsearch cluster deployed
- Liferay configured for remote Elasticsearch
- NGINX Ingress configured
- Prometheus & Grafana installed via Helm
- Grafana exposed through Ingress

---

## Next Steps

- JVM metrics via JMX Exporter
- Alerting rules (CPU, memory, latency)
- TLS termination with cert-manager
- PostgreSQL exporter
- CI/CD deployment pipeline
- Architecture diagrams

---

## Notes

This project focuses on **realistic infrastructure patterns** rather than simplified demos.

Components are deployed, integrated and operated as they would be in a real production-like environment, prioritizing clarity, maintainability and operational correctness over minimal examples.
