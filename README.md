# EcomPK Flash Sale — Observability Stack

> **Cloud Computing Assignment — 20 Marks**  
> Advanced Observability Stack & Self-Healing Architecture for an e-commerce platform based in Islamabad, Pakistan.

## Stack

| Layer | Tool | Purpose |
|-------|------|---------|
| Metrics | Prometheus + Node Exporter + cAdvisor | Scrape app, host, container metrics |
| Visualization | Grafana | Four Golden Signals dashboard |
| Logs | Promtail → Loki | Centralized log collection + LogQL |
| Alerting | Alertmanager | Discord webhook + Email routing |
| Auto-scaling | Kubernetes HPA | Scale 2→10 pods at 70% CPU |
| Security | Trivy | Config scanning + remediation |

## Quick Start

```bash
cp .env.example .env       # fill in Discord webhooks + SMTP
docker compose up -d       # start full stack
open http://localhost:3000  # Grafana (admin/admin)
```

## Load Test (triggers alerts + HPA)

```bash
pip install httpx rich
python scripts/load-test.py --rps 100 --duration 180
```

## Simulate Incidents (Task 2 audit)

```bash
curl http://localhost:5000/simulate/db-error      # DB pool exhausted
curl http://localhost:5000/simulate/null-pointer  # NullPointerError
curl http://localhost:5000/simulate/memory-leak   # ~50MB allocation
```

## LogQL Queries (Grafana Explore → Loki)

```logql
# ERROR/CRITICAL filter
{service="ecompk-app"} | json | level =~ "ERROR|CRITICAL"

# Trace by request_id
{service="ecompk-app"} | json | request_id = "<uuid>"

# DB errors
{service="ecompk-app"} | json | message =~ ".*pool exhausted.*"
```

## Repository Structure

```
observability-stack/
├── app/                    # Flask app (instrumented + hardened Dockerfile)
├── prometheus/             # prometheus.yml + alert-rules.yml
├── grafana/                # Dashboard JSON + provisioning
├── loki/                   # loki-config.yaml
├── promtail/               # promtail-config.yaml
├── alertmanager/           # alertmanager.yml (Discord + Email)
├── k8s/                    # Deployment + HPA + PDB
├── trivy-reports/          # Security scan findings + remediations
├── scripts/                # load-test.py
└── docker-compose.yml
```

## Task Coverage

| Task | Marks | Status |
|------|-------|--------|
| Task 1: Metrics & Grafana Dashboard | 6 | ✅ |
| Task 2: Log Analytics & Incident Debugging | 5 | ✅ |
| Task 3: Alerting & Auto-Scaling | 6 | ✅ |
| Task 4: Security Auditing | 3 | ✅ |
| **Total** | **20** | ✅ |
