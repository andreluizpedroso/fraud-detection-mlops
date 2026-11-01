# mlops-lab — Credit Card Fraud Detection

![Sprint](https://img.shields.io/badge/Sprint-12%2F12-success)
![CI](https://github.com/andreluizpedroso/fraud-detection-mlops/actions/workflows/ci.yml/badge.svg)
![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)
![MLflow](https://img.shields.io/badge/MLflow-2.15-0194E2?logo=mlflow&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-0.111-009688?logo=fastapi&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.5-F7931E?logo=scikitlearn&logoColor=white)
![pandas](https://img.shields.io/badge/pandas-2.2-150458?logo=pandas&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16-4169E1?logo=postgresql&logoColor=white)
![MinIO](https://img.shields.io/badge/MinIO-S3--compatible-C72E49?logo=minio&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-v3.4-E6522C?logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-12.0-F46800?logo=grafana&logoColor=white)
![Evidently](https://img.shields.io/badge/Evidently-0.7-6A4C93?logoColor=white)
![Feast](https://img.shields.io/badge/Feast-0.64-FF6B35?logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Kind-326CE5?logo=kubernetes&logoColor=white)
![Airflow](https://img.shields.io/badge/Apache%20Airflow-2.10-017CEE?logo=apacheairflow&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-GCP%20IaC%20only-844FBA?logo=terraform&logoColor=white)
![uv](https://img.shields.io/badge/uv-package%20manager-DE5FE9?logo=uv&logoColor=white)
![Pytest](https://img.shields.io/badge/pytest-8.2-0A9EDC?logo=pytest&logoColor=white)
![Ruff](https://img.shields.io/badge/Ruff-lint%2Fformat-D7FF64?logo=ruff&logoColor=black)

🇧🇷 [Leia em português](README.pt-BR.md)

End-to-end credit card fraud detection system, built during the **Machine Learning Engineering Residency**, a 12-sprint program for learning MLOps hands-on, from local experiment to cloud deployment.

---

## The problem

Fraudulent transactions make up less than **0.17%** of the dataset (284,807 transactions, 492 frauds). The challenge goes beyond accuracy: a false negative (undetected fraud) has a real financial cost for the customer and the bank. A false positive (a legitimate transaction blocked) hurts the user experience. The system must balance precision and recall while keeping every decision fully traceable.

---

## Local architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                        Docker Compose                            │
│                                                                  │
│  ┌───────────┐  ┌───────────┐  ┌──────────────────────────────┐ │
│  │ PostgreSQL│  │   MinIO   │  │       MLflow Server          │ │
│  │  :5432    │◄─│  :9000    │◄─│          :5000               │ │
│  │ (backend  │  │ (artifact │  │  (experiments + registry)    │ │
│  │  store)   │  │  store)   │  └──────────────────────────────┘ │
│  └───────────┘  └───────────┘              ▲                    │
│                                            │                    │
│  ┌─────────────────────────────────────────┴────────────────┐   │
│  │              FastAPI — Fraud Detection API  :8000         │   │
│  │  GET  /health    → status + loaded model                 │   │
│  │  POST /predict   → { is_fraud, fraud_probability }       │   │
│  │  GET  /metrics   → Prometheus metrics                    │   │
│  └────────────────────────────┬─────────────────────────────┘   │
│                               │ scrape /metrics every 15s       │
│  ┌────────────────────────────▼───────────┐                     │
│  │         Prometheus  :9090              │                     │
│  │  (time series: latency,                │                     │
│  │   throughput, fraud rate)              │                     │
│  └────────────────────────────┬───────────┘                     │
│                               │ PromQL                          │
│  ┌────────────────────────────▼───────────┐                     │
│  │         Grafana  :3000                 │                     │
│  │  (dashboard: req/s, p99, fraud rate)   │                     │
│  └────────────────────────────────────────┘                     │
└──────────────────────────────────────────────────────────────────┘
```

---

## Sprint Progress

| Sprint | Topic | Status |
|--------|-------|--------|
| 1 | Setup: environment, scaffold, local stack (MLflow + PostgreSQL + MinIO) | ✅ Done |
| 2 | Data ingestion, feature engineering | ✅ Done |
| 3 | Training + Experiment Tracking (MLflow) | ✅ Done |
| 4 | Model Registry + versioning with aliases | ✅ Done |
| 5 | FastAPI serving + Docker + train/serve skew fix | ✅ Done |
| 6 | CI/CD with GitHub Actions + pytest | ✅ Done |
| 7 | Observability: Prometheus + Grafana | ✅ Done |
| 8 | Data Drift + Model Drift (Evidently AI) | ✅ Done |
| 9 | Feature Store (Feast) | ✅ Done |
| 10 | Kubernetes (Kind): declarative deployment, HPA, health probes | ✅ Done |
| 11 | Automated retraining (Airflow): weekly drift→retrain DAG | ✅ Done |
| 12 | Migration to GCP (cloud phase): Cloud SQL, GCS, Cloud Run | ✅ Done |

### Phase 2 — Continuous lab

The project continues as an ongoing lab for staying current: one sprint at a time, each fitting a new practice into the stack that is already running.

| Sprint | Topic | Status |
|--------|-------|--------|
| 13 | Champion/Challenger + gradient boosting (LightGBM/XGBoost), promotion by business cost | 📋 Planned |
| 14 | Incident agent: an LLM reads Prometheus + Evidently and writes the drift report | 📋 Planned |
| 15 | The lab as an MCP server (query and act on the system in natural language) | 📋 Planned |
| 16 | LLM evals and observability (golden set, LLM-as-judge, tracing, CI gate) | 📋 Planned |
| 17 | Serving at scale: shadow, canary and automatic rollback on Kubernetes | 📋 Planned |
| 18 | Real GCP deployment with controlled cost (optional) | 📋 Planned |

---

## Cloud (Sprint 12)

Sprint 12 wrote the complete mapping from the local stack to GCP in Terraform, **without provisioning any real resources** (an explicit decision to avoid costs). Cloud SQL has no permanent "always free" tier, unlike Cloud Run and GCS, so the whole sprint was kept as an architecture exercise: code reviewed line by line, `terraform apply` never executed.

| Local component | GCP component | File |
|---|---|---|
| `postgres` (container) | Cloud SQL | `deployment/gcp/cloud_sql.tf` |
| `minio` (container, S3-compatible) | Cloud Storage | `deployment/gcp/gcs.tf` |
| Docker containers / Kind manifests (Sprint 10) | Cloud Run | `deployment/gcp/cloud_run.tf` |

Details, diagrams and trade-offs (why Cloud Run instead of reusing the Sprint 10 manifests on GKE) in [`docs/sprints/sprint-12.md`](docs/sprints/sprint-12.md).

---

## Quick start

### Prerequisites

- Docker + Docker Compose
- [uv](https://docs.astral.sh/uv/getting-started/installation/) installed

### 1. Set up the environment

```bash
cp .env.example .env
uv sync --extra dev
```

### 2. Bring up the full stack

```bash
docker compose -f docker/docker-compose.yml up -d
```

### 3. Download the dataset

```bash
# Requires a Kaggle account and ~/.kaggle/kaggle.json configured
uv run python pipelines/download_data.py
```

### 4. Run the full pipeline

```bash
# Feature engineering
uv run python pipelines/feature_engineering.py

# Training + MLflow tracking
uv run python pipelines/train.py

# Register and promote the best model
uv run python pipelines/register_model.py
```

---

## Available interfaces

| Interface | URL | Description |
|-----------|-----|-------------|
| MLflow UI | http://localhost:5000 | Experiments, runs, metrics, Model Registry |
| MinIO Console | http://localhost:9001 | Artifacts: models, plots, files |
| MinIO API (S3) | http://localhost:9000 | S3-compatible endpoint used by MLflow |
| API docs (Swagger) | http://localhost:8000/docs | Interactive endpoints with example payloads |
| API Metrics | http://localhost:8000/metrics | Prometheus endpoint (latency, throughput, fraud) |
| Prometheus | http://localhost:9090 | Time-series database — PromQL queries |
| Grafana | http://localhost:3000 | Operational and business dashboards (`admin` / `admin`) |
| API via Kubernetes | http://localhost:8080 | Same API served by the Kind cluster (Sprint 10) |
| Airflow UI | http://localhost:8081 | DAGs, runs and logs (`admin` / `admin`) (Sprint 11) |

---

## Fraud detection API

### `GET /health`

```json
{ "status": "ok", "model_loaded": true, "model_alias": "champion" }
```

### `POST /predict`

**Request:** a transaction with features `V1`–`V28` (PCA components) + `Amount` as a raw value.

```bash
curl -X POST http://localhost:8000/predict \
  -H "Content-Type: application/json" \
  -d '{"V1": -1.36, "V2": -0.07, ..., "Amount": 149.62}'
```

**Response:**

```json
{ "is_fraud": false, "fraud_probability": 0.0012, "model_alias": "champion" }
```

The loaded model is always `fraud-detector@champion` from the MLflow Model Registry. To swap the production model, just move the alias — no code redeploy.

---

## Observability

The **Fraud Detection API** dashboard in Grafana (`Dashboards → MLOps`) shows:

| Panel | What it measures |
|-------|------------------|
| Requests / second | Incoming traffic rate |
| 5xx Error Rate | Share of responses with a server error |
| Detected Fraud Rate | % of transactions classified as fraud (business metric) |
| Latency p50 / p90 / p99 | Response time distribution — p99 = worst case of the slowest 1% |
| Requests per endpoint | Volume split by route and HTTP status |
| Fraud vs Legitimate | Time trend of the two prediction classes |

---

## Commands

```bash
# Infrastructure
make up           # Brings up the full stack (MLflow + MinIO + API + Prometheus + Grafana)
make down         # Stops the containers
make logs         # Follows logs in real time
make ps           # Container status

# Code quality
make lint         # ruff check
make format       # ruff format
make check        # lint + mypy

# Tests
make test         # pytest
make test-cov     # pytest with coverage report

# Setup
make install      # uv sync --extra dev
make env          # creates .env from .env.example
make help         # lists all commands

# Kubernetes (Kind) — Sprint 10
make k8s-create   # creates a local Kind cluster
make k8s-deploy   # build + load image + apply manifests
make k8s-status   # pod/svc/hpa status
make k8s-delete   # removes the cluster

# Airflow — Sprint 11
make airflow-up   # brings up webserver + scheduler + postgres
make airflow-down # stops Airflow
```

---

## Project structure

```
mlops-lab/
├── app/                        # FastAPI — model serving
│   ├── main.py                 #   lifespan, /health, /predict, /metrics
│   └── schemas.py              #   Pydantic contracts (Transaction, PredictionResponse)
├── pipelines/                  # Standalone scripts
│   ├── download_data.py        #   dataset ingestion (Kaggle)
│   ├── feature_engineering.py  #   preprocessing + 80/20 split
│   ├── train.py                #   training with sklearn Pipeline + MLflow tracking
│   ├── register_model.py       #   registration and promotion in the Model Registry
│   ├── detect_drift.py         #   data drift detection with Evidently AI (Sprint 8)
│   └── materialize_features.py #   feature materialization in Feast (Sprint 9)
├── monitoring/                 # Observability
│   ├── prometheus.yml          #   scrape config (job: fraud-api → :8000/metrics)
│   └── grafana/
│       ├── provisioning/       #   datasource + dashboard provider (auto-loaded)
│       └── dashboards/         #   fraud_api.json — 6 PromQL panels
├── tests/                      # pytest
│   ├── conftest.py             #   fixtures: FakeModel, api_client, fraud_api_client
│   ├── test_smoke.py           #   imports and minimum versions
│   ├── test_api.py             #   /health and /predict tests (no real MLflow)
│   ├── test_drift.py           #   drift detection tests (Sprint 8)
│   └── test_features.py        #   Feast materialization tests (Sprint 9)
├── docker/                     # Containerization
│   ├── docker-compose.yml      #   main stack: postgres+minio+mlflow+api+prometheus+grafana
│   ├── docker-compose-airflow.yml  # separate Airflow stack (Sprint 11)
│   ├── Dockerfile.mlflow       #   custom MLflow image
│   └── Dockerfile.api          #   FastAPI image (uv sync --frozen)
├── feature_store/              # Feature Store (Feast) — Sprint 9
│   ├── feature_store.yaml      #   project config (offline=Parquet, online=SQLite)
│   └── features.py             #   entity + FeatureView (V1-V28, Amount)
├── deployment/                 # Kubernetes (Sprint 10)
│   ├── kind-cluster.yaml       #   Kind cluster with port mapping :30800→:8080
│   └── k8s/                    #   manifests: namespace, configmap, secret, deploy, svc, hpa
├── airflow/                    # Apache Airflow (Sprint 11)
│   └── dags/
│       └── fraud_retraining_dag.py  # weekly DAG: drift→branch→retrain→register
├── deployment/gcp/              # Terraform GCP (Sprint 12) — conceptual IaC, never applied
│   ├── cloud_sql.tf             #   Postgres → Cloud SQL
│   ├── gcs.tf                   #   MinIO → Cloud Storage
│   └── cloud_run.tf             #   containers/Kind → Cloud Run
├── docs/sprints/               # Per-sprint documentation
│   └── sprint-01.md … sprint-18.md   # 13–18: phase 2, planned
├── .github/workflows/
│   └── ci.yml                  # CI: lint + mypy + pytest + docker build; CD: image push to GHCR (main)
├── data/                       # Data generated at runtime (gitignored)
├── pyproject.toml              # Dependencies + tool config
├── uv.lock                     # Reproducible lockfile
├── Makefile                    # Dev shortcuts
└── .env.example                # Environment variables template
```

---

## Production model

| Field | Value |
|-------|-------|
| Name | `fraud-detector` |
| Current version | 2 |
| Algorithm | `sklearn.Pipeline` (StandardScaler + RandomForestClassifier) |
| PR-AUC | 0.8681 |
| Recall (fraud) | 80.6% |
| Precision (fraud) | 91.9% |
| Alias | `@champion` |

The model is a self-contained `Pipeline`: it takes `Amount` as a raw value and scales it internally, removing the risk of train/serve skew.

---

## CI/CD

Every push and PR to `main` automatically triggers:

1. **Lint** — `ruff check`
2. **Type check** — `mypy app pipelines`
3. **Tests** — `pytest --cov` (8 tests, ~4s, no external infrastructure)
4. **Docker build** — verifies that `Dockerfile.api` builds without errors

On a direct push to `main` (not on PRs), a fifth job publishes the production image:

5. **Docker publish** — build + push to the GitHub Container Registry, with no cloud account required and no cost:
   ```
   ghcr.io/andreluizpedroso/residencia-mle-mlops-api:latest
   ghcr.io/andreluizpedroso/residencia-mle-mlops-api:<commit sha>
   ```

Current status: ![CI](https://github.com/andreluizpedroso/fraud-detection-mlops/actions/workflows/ci.yml/badge.svg)
