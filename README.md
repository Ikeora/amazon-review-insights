# Amazon Review Issue Discovery System

## 🚀 Project Overview

This repository houses an end-to-end NLP analytics system designed to automatically analyze Amazon product reviews and surface recurring customer issues, sentiment trends, and emerging defects. It transforms unstructured review text into structured business intelligence for product, QA, and customer experience teams.

## 🎯 Business Objectives

- Automatically detect common product defects
- Identify recurring customer pain points
- Track sentiment trends over time
- Detect emerging issues early
- Provide executive‑level dashboards for decision making

This allows the organization to convert vast volumes of reviews into measurable insights and improve product quality.

## 🛠 Technical Objectives

Build a scalable NLP pipeline that:

1. Ingests review data from a public dataset
2. Cleans and preprocesses raw text (deduplication, language filtering, normalization, stopword removal, tokenization)
3. Generates transformer‑based embeddings using Sentence Transformers (e.g., MiniLM)
4. Applies unsupervised clustering (HDBSCAN, KMeans, or BERTopic) to group similar reviews and extract keywords
5. Performs sentiment analysis and aggregates scores per cluster
6. Tracks trends and issue frequency over time
7. Exposes insights via dashboards or API endpoints
8. Supports retraining and monitors for topic drift

## 🧩 Core Components

1. **Data Ingestion** – public Amazon review dataset; local storage for prototype (Azure Blob later).
2. **Text Preprocessing** – quality control and normalization.
3. **Embedding Generation** – semantic vectorization.
4. **Unsupervised Clustering** – automatic issue grouping.
5. **Sentiment Analysis** – polarity scoring and aggregation.
6. **Insights Layer** – recurring issues, trend analysis, emergent negative clusters.
7. **Dashboard** – Power BI view with drill‑down capabilities.

## 📈 Architecture Evolution Plan

| Phase | Description |
|-------|-------------|
| 1 | Local prototype (Python + uv) |
| 2 | Modular production‑style pipeline |
| 3 | Scale preprocessing with Databricks |
| 4 | Model orchestration with Azure ML |
| 5 | Business dashboard with Power BI |


## 💡 Expected Outcomes

- Automated detection of recurring product issues
- Quantifiable sentiment trends
- Early warning for emerging problems
- A portfolio‑ready demonstration of NLP, unsupervised learning, ML engineering, cloud architecture, and business analytics

## 🛠 Proposed Technical Stack

- Python
- Sentence Transformers
- HDBSCAN / BERTopic
- scikit‑learn
- Azure Databricks (scaling)
- Azure ML (model orchestration)
- Power BI (dashboard)
- GitHub (version control)
- uv (environment management)

Feel free to justify alternative tools if necessary.

## 📂 Repository Structure (anticipated)

```
notebooks/       # exploratory analysis and experiments
src/             # reusable modules and ETL code
data/            # raw/processed datasets (gitignored)
tests/           # unit/functional tests
pyproject.toml   # project metadata and dependencies
README.md        # this overview
```

## 📝 Getting Started

1. Install dependencies via `uv` or `pip` using `pyproject.toml`.
2. Activate your Python environment (3.12+).
3. Explore notebooks in `notebooks/` for initial analysis.
4. Move production code into `src/` and add tests under `tests/`.

## 📎 Notes for Contributors

See `.github/copilot-instructions.md` for guidance on working with AI assistance in this repo.

---

This README provides a living project summary. Feel free to expand sections as the system evolves.