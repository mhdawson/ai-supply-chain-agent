# Supply Chain Dashboard — Helm chart

Umbrella chart for the AI Supply Chain Agent quickstart. It deploys the application tier (backend, frontend, ingest job) plus shared AI/data subcharts from [ai-architecture-charts](https://github.com/rh-ai-quickstart/ai-architecture-charts).

## What it deploys

- **pgvector** — PostgreSQL with the pgvector extension
- **llama-stack** — Llama Stack API (chat, vector stores)
- **llm-service** — Model serving (e.g. Llama 3.2 1B)
- **backend** — Flask API
- **frontend** — React dashboard (nginx)
- **ingest** — Optional post-install Job (`ingest.enabled`)

## Prerequisites

- Helm 3.14+
- OpenShift CLI (`oc`) for cluster deploys
- A [Hugging Face token](https://huggingface.co/settings/tokens) in `values.yaml` at `llm-service.secret.hf_token` (required for gated models)

## Install

From the repository root:

```bash
helm dependency update ./helm
helm upgrade --install supply-chain-dashboard ./helm \
  -f helm/values.yaml \
  --namespace supply-chain-dashboard \
  --create-namespace \
  --wait \
  --timeout 10m
```

Or use `make helm-deps` and `make helm-install` (see root `Makefile`).

## Verify

```bash
oc get pods,route -n supply-chain-dashboard
```

## Customize

- **Models** — `global.models` and `llm-service.models`
- **GPU** — `llm-service.device` and per-model `device` (default is CPU in `values.yaml`)
- **Ingest** — `ingest.strategy` (`llamastack` | `langchain`), `ingest.enabled`, chunking under `ingest.*` for LangChain only

Full operator documentation: [README.md](../README.md) and [docs/WHAT_TO_EXPECT.md](../docs/WHAT_TO_EXPECT.md).
