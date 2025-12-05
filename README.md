
# AI Trip Planner

An end-to-end intelligent travel planning assistant that blends large language models, curated tools, and event-driven automation to produce shareable itineraries, live insights, and embedded budgets in minutes. The system orchestrates conversational agents behind a FastAPI/Streamlit experience, so non-technical users can research, optimize, and operationalize trips from a single workspace.

---

## ✨ Highlights
- **Multi-agent reasoning** – a coordinator agent calls specialized research, budgeting, and logistics tools to craft city-level plans automatically.
- **Real-time awareness** – live weather, currency conversion, and place search tools keep output contextualized to the moment of the request.
- **Financial governance** – built-in expense calculator and FX reconciliation ensure every plan ships with an aligned budget summary.
- **Professional-grade observability** – MLflow plus Prometheus/Grafana monitoring are configured to keep model drift under **2%** and latency growth under **5%**, surfacing anomalies before users notice.
- **Reliable releases** – agents ship as Dockerized ECS microservices, and GitHub Actions CI/CD handles fast deployments with automatic rollbacks.

---

## 🧱 Architecture Overview
1. **Experience layer** – Streamlit dashboard for interactive planning and a FastAPI service (`main.py`) for programmatic access.
2. **Agent router** – `agent/agentic_workflow.py` brokers between the user goal and the tool stack, storing context in `config/config.yaml`.
3. **Tools** – Weather, currency, arithmetic, place search, and expense utilities within `tools/` and `utils/`.
4. **Persistence & artifacts** – Plans, budgets, and evaluation metadata flow into MLflow tracking while Prometheus scrapes runtime metrics for Grafana.
5. **Deployment** – Docker images are promoted through GitHub Actions to Amazon ECS; blue/green rollouts provide instant fallback on regressions.

```
User -> Streamlit / FastAPI -> Agent Router -> Domain Tools -> Responses + Metrics
                                              |-> MLflow
                                              |-> Prometheus -> Grafana
```

---

## 🛠️ Getting Started

### Prerequisites
- Python 3.10+
- `uv` or `pip` for dependency management
- Optional: Docker (for ECS parity) and an AWS CLI profile if you plan to deploy

### Installation
```bash
git clone https://github.com/KR2208/AI_Trip_Planner.git
cd AI_Trip_Planner
uv pip install -r requirements.txt         # or: pip install -r requirements.txt
uv venv env --python cpython-3.10.18       # creates a reproducible environment
source env/bin/activate                    # Windows: env\Scripts\activate
```

### Environment
1. Copy `.env.name` to `.env` and fill in your API keys (LLM provider, weather API, currency source, etc.).
2. Adjust defaults in `config/config.yaml` if you want custom destinations, budgets, or logging behavior.

### Run the apps
```bash
# Conversational UI
streamlit run streamlit_app.py

# FastAPI backend
uvicorn main:app --reload --port 8000
```

---

## 🔍 Agents & Tooling
- `tools/weather_info_tool.py` – hourly/daily weather context plus anomaly alerts.
- `tools/place_search_tool.py` – location discovery and ranking via semantic search.
- `tools/currency_conversion_tool.py` – FX normalization with cached mid-market rates.
- `tools/expense_calculator_tool.py` – aggregates categories into trip-level budgets.
- `tools/arthamatic_op_tool.py` – lightweight math helper for itinerary constraints.
- `utils/*` – loader utilities, document persistence, and adapters for third‑party APIs.

Each tool exposes a consistent interface so the orchestrator can request intermediate data, combine results, and validate them before returning the final itinerary.

---

## 📈 Monitoring & Reliability
- **MLflow tracking** captures dataset snapshots, prompts, and agent evaluation runs so experimentation history is auditable.
- **Prometheus + Grafana** dashboards visualize throughput, latency, and error budgets; alerting is tuned to keep drift <2% and latency growth <5%.
- **Automated recovery** – health checks feed ECS so degraded tasks are replaced proactively.

---

## 🚀 CI/CD & Deployment
1. GitHub Actions builds and tests every pull request.
2. On merge, Docker images are published to ECR.
3. An ECS blue/green deployment updates the agent services with automatic rollback triggers tied to Prometheus alerts.

Provisioning scripts (Terraform or CloudFormation) can be layered in to create networking, ALB, and secret stores, but the repo is deployment-ready once AWS credentials are configured.

---

## 🧪 Testing & Quality
- Unit tests target utilities and tools (run with `pytest`).
- Regression prompts are replayed via MLflow to ensure new model versions preserve itinerary quality.
- Synthetic monitoring calls the FastAPI endpoint hourly during releases to confirm stability.

---

## 🤝 Contributing
1. Fork & clone the repository.
2. Create a feature branch from `main`.
3. Run formatting/tests locally.
4. Open a pull request referencing any Jira tickets or roadmap items.

---

## 📬 Support
File GitHub issues for bugs or feature requests, or reach out via the discussions tab to propose enhancements. Happy travels!
