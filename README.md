# n8n Workflow Demo Suite

AI-powered automation workflows built for the **Cambodia National AI Strategy 2025–2030** research pipeline.

## Workflows

| File | Trigger | Description |
|------|---------|-------------|
| `wf1-etl-pipeline.json` | Schedule 6AM daily | Fetches API data, transforms, bulk-inserts into PostgreSQL |
| `wf2-csv-processing.json` | POST `/webhook/csv-upload` | Accepts CSV uploads, parses, deduplicates, stores to DB |
| `wf3-sentiment-analysis.json` | POST `/webhook/sentiment-webhook` | Classifies text with Groq LLM, routes to CRM or Telegram alert |
| `wf4-data-orchestrator.json` | Schedule 2AM daily | Aggregates stats from all pipelines, sends nightly Telegram report |
| `wf5-research-data.json` | Schedule 6AM daily | Scrapes arXiv/Semantic Scholar/PubMed, generates embeddings, sends digest |
| `wf6-mlops-pipeline.json` | Schedule 2AM daily | Trains model on labeled data, logs metrics to MLflow/DagsHub |
| `wf7a-active-learning.json` | Every 2 hours | Classifies unlabeled papers with Groq (confidence gate), sends HITL review |
| `wf7b-hitl-handler.json` | GET `/webhook/hitl-label` | Receives human labels from Telegram, updates DB, returns HTML confirmation |
| `daily-digest.json` | Schedule 8AM daily | Fetches arXiv papers, summarizes with Claude AI, emails digest |

## How to Import

1. Open your n8n instance
2. Go to **Workflows** → top-right menu → **Import from file**
3. Upload any `.json` file from this folder
4. Re-link credentials (Postgres, Telegram, Groq, etc.) in each workflow's node settings

## Credentials to set up

After importing, you'll need to configure these credentials in n8n:

| Credential | Used by | Notes |
|------------|---------|-------|
| **Postgres** | WF1–WF7 | Neon serverless PostgreSQL |
| **Telegram Bot** | WF3, WF4, WF7a, WF7b | Bot token + chat ID |
| **HTTP Header Auth** (Groq) | WF3, WF6 | `Authorization: Bearer YOUR_GROQ_API_KEY` |
| **HTTP Header Auth** (Jina AI) | WF5 | `Authorization: Bearer YOUR_JINA_API_KEY` |
| **Gmail** | daily-digest | OAuth2 |
| **DagsHub / MLflow** | WF6 | Basic auth |

> **Note:** All credential IDs and API keys have been replaced with placeholders (`YOUR_GROQ_API_KEY`, etc.). You must supply your own keys.
