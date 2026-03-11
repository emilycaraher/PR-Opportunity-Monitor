# PR Opportunity Monitor
Semantic news matching system for PR agencies: dense retrieval, RAG pitch generation, and a web digest UI for four real clients across three industries.

**Read the full write-up on Medium:** [Beyond Keyword Alerts: Building a Semantic PR Opportunity Monitor for a Real Agency](https://medium.com/@ec45335/beyond-keyword-alerts-building-a-semantic-pr-opportunity-monitor-for-a-real-agency-a622f5428c8e)
---

## What It Does

For each client, the system builds an "expertise fingerprint" by embedding their historical article corpus into a ChromaDB vector store. Incoming news headlines are matched against this fingerprint using dense semantic retrieval. Strong matches trigger a RAG-generated media pitch grounded in the client's real prior coverage.

---

## Contents

| File | Description |
|---|---|
| `PR_Monitor_latest.ipynb` | Full pipeline: corpus ingestion, vector store construction, news retrieval, semantic matching, RAG pitch generation, and human feedback logging |
| `pr_monitor.html` | Web-based digest UI for reviewing matches, reading generated pitches, and rating opportunities |
| `Presentation.pdf` | Project presentation slides |
| `Report.pdf` | Full technical report |

---

## System Overview

- **Bi-encoder embeddings:** all-MiniLM-L6-v2 via sentence-transformers, 384-dim dense vectors
- **Vector store:** ChromaDB with one isolated collection per client
- **News ingestion:** Google News RSS feeds, no rate limits, 15-45 headlines per weekly run
- **Retrieval:** Dense-only (α = 1.0) outperformed hybrid search across all four clients
- **Pitch generation:** llama-3.3-70b-versatile via Groq API, K = 3 retrieved articles as context
- **Thresholds:** Strong >= 0.55, Adjacent 0.40-0.55, up to 5 strong matches surfaced per client per run

---

## Clients

| Client | Industry | Corpus |
|---|---|---|
| Client 1 | Personal Injury Law | 125 articles, client-provided drafts |
| Client 2 | Healthcare | 94 scraped + 47 health awareness calendar seeds |
| Client 3 | Telecom | 613 scraped articles |
| Client 4 | Business Law | 194 scraped articles |

---

## Evaluation

Retrieval evaluated using leave-one-out consistency: for each article, its title is used as a query against the remaining corpus. A hit is counted when at least one result scores >= 0.55.

| Client | Consistency (α = 1.0) |
|---|---|
| Client 1 | 0.875 |
| Client 2 | 0.847 |
| Client 3 | 0.798 |
| Client 4 | 0.617 |

Scores increased monotonically with alpha for all four clients, confirming dense-only retrieval as the optimal configuration.

---

## Setup

1. Open `PR_Monitor_latest.ipynb` in Google Colab
2. Add `GROQ_API_KEY` to Colab secrets
3. Upload the four client CSV files when prompted
4. Run cells in order
5. Download the generated `digest.json` and open alongside `pr_monitor.html` to view the digest UI

> Note: client CSV files are not included in this repo for confidentiality reasons.

---

## Authors

Emily Caraher, Emily Lagnese, Sophia Remington, Lena Weissman
