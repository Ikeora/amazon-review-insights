# 🛢️ Early Warning Signal: Predicting Amazon Product Rating Drops from Complaint Cluster Velocity

## 🚀 Project Overview

This project builds an end-to-end NLP analytics pipeline that monitors product complaints across multiple platforms (Amazon, Reddit, retail review sites) and predicts whether a product's star rating will drop within 30 days — before the decline is visible in aggregate scores.

The core thesis: **negative sentiment clusters on informal platforms (Reddit, forums) surface earlier than they appear in structured review scores.** By tracking the velocity of complaint clusters over time, we can generate an early warning signal for product teams, QA analysts, and supply chain managers.

---

## 🎯 Research Question

> *Can the growth velocity of NLP-derived complaint clusters predict a measurable product rating drop within a 30-day window?*

This transforms a descriptive analytics system into a **predictive intelligence tool** — one with a clear, measurable success criterion and direct business value.

---

## 🧩 Pipeline Components

### 1. Data Ingestion
- **Amazon Reviews** – primary structured review source (rating + text + timestamp)
- **Reddit** – r/BuyItForLife, r/ProductReviews, product-specific subreddits (informal early signals)
- **Retail Sites** – supplementary review sources where accessible
- Local storage for prototype phase; Azure Blob Storage in production

### 2. Text Preprocessing
- Deduplication and language filtering (English focus; bilingual handling where applicable)
- Normalization, stopword removal, tokenization
- Platform tagging to track complaint origin

### 3. Embedding Generation
- Sentence Transformers (`all-MiniLM-L6-v2`) for semantic vectorization
- Batch inference with caching to avoid recomputation

### 4. Unsupervised Clustering
- **BERTopic** (HDBSCAN + c-TF-IDF) for automatic topic/issue extraction
- Cluster labelling and keyword extraction per issue group
- Noise filtering to remove low-signal clusters

### 5. Velocity & Trend Tracking *(key differentiator)*
- Cluster size tracked as a time series per product
- **Velocity score**: rate of change in complaint cluster membership over rolling 7-day and 14-day windows
- Spike detection for emerging negative clusters

### 6. Predictive Model
- Features: velocity score, sentiment polarity, cluster size, platform source weight, recency
- Target: binary label — does the product's Amazon star rating drop ≥ 0.2 stars within 30 days?
- Models evaluated: Logistic Regression (baseline), XGBoost
- Evaluation: precision, recall, F1, and time-to-signal (how many days early was the warning?)

### 7. Sentiment Analysis
- Per-cluster polarity scoring (VADER or fine-tuned DistilBERT)
- Aggregated sentiment trends per product over time
- Severity weighting by platform (Reddit complaints weighted as earlier signals)

### 8. Complaint Classification
Before insights are surfaced, complaints are classified into two distinct buckets:
- **Product complaints** — defects, design flaws, performance failures (e.g. "battery dies in 2 hours")
- **Fulfilment complaints** — shipping, packaging, wrong item, seller errors (e.g. "arrived damaged", "wrong colour sent")

Separating these ensures that velocity spikes caused by a bad shipping batch don't trigger false product defect alerts. This is a data quality layer as much as an analytics one.

### 9. Feature-Level Sentiment
Beyond cluster-level sentiment, individual product attributes are extracted and scored:
- Attribute mention frequency (e.g. battery, screen, packaging, smell, size, durability)
- Sentiment polarity per attribute over time
- Identifies which specific features are driving overall sentiment — not just whether reviews are negative, but *why*

### 10. Review Integrity Detection
Anomalous review patterns are flagged before they contaminate the signal:
- **Review bombing**: sudden velocity spikes that are sentiment-uniform (all negative, no topic diversity) — likely coordinated, not organic
- **Suspicious positivity bursts**: rapid influx of 5-star reviews with low textual variance — potential fake review campaigns
- Flagged reviews are quarantined from the predictive model but preserved for audit

### 11. Insights & Dashboard

The dashboard is split into two views serving two distinct user personas:

**Operational View** *(for QA analysts and product managers)*
- **Watchlist**: products with rising complaint velocity above threshold
- **Issue clusters**: grouped complaint topics with trend lines
- **Prediction outcomes**: historical cases where the model correctly flagged a rating drop
- **Early warning timeline**: how far in advance signals appeared vs. rating change
- **Complaint type split**: product defects vs. fulfilment issues per product

**Strategic View** *(for senior leadership and product strategy)*
- **Feature sentiment heatmap**: which product attributes are trending negative across the catalogue
- **Net Sentiment Score**: aggregate sentiment per product over time, analogous to NPS but text-derived
- **Issue resolution tracking**: do complaint clusters shrink after a product update? Did the fix work?
- **Review integrity flags**: products with suspected review bombing or fake review activity

---

## 📈 Architecture Evolution Plan

| Phase | Description |
|-------|-------------|
| 1 | Local prototype — Amazon dataset only, BERTopic clustering, velocity scoring |
| 2 | Add Reddit ingestion; cross-platform complaint alignment |
| 3 | Build and evaluate predictive model; establish labelled ground truth |
| 4 | Scale preprocessing with Azure Databricks |
| 5 | Model orchestration and retraining with Azure ML |
| 6 | Production dashboard in Power BI with drill-down and watchlist views |

---

## 💡 Expected Outcomes

- A validated early warning signal for product rating declines
- Quantified lead time: how many days before a rating drop do clusters spike?
- A reusable cross-platform complaint monitoring pipeline
- Measurable model performance (precision/recall on held-out test window)
- Feature-level sentiment breakdown showing *which* product attributes drive negative reviews
- Clean separation of product defect signals from fulfilment noise
- Review integrity layer that identifies bombing and fake review patterns
- A dual-view dashboard serving both operational and strategic decision-making needs
- A portfolio project that demonstrates NLP, unsupervised learning, time series analysis, predictive modelling, and cloud architecture

---

## 🛠 Technical Stack

| Layer | Tools |
|-------|-------|
| Language & Environment | Python 3.12+, uv |
| Embeddings | Sentence Transformers (`all-MiniLM-L6-v2`) |
| Clustering | BERTopic, HDBSCAN, scikit-learn |
| Sentiment | VADER, DistilBERT (HuggingFace) |
| Prediction | XGBoost, scikit-learn |
| Data Ingestion | PRAW (Reddit API), custom scrapers |
| Cloud Storage | Azure Blob Storage |
| Pipeline Scaling | Azure Databricks |
| Model Orchestration | Azure ML |
| Dashboard | Power BI |
| Version Control | GitHub |

---

## 📂 Repository Structure

```
notebooks/          # exploratory analysis and experiments
src/
  ingestion/        # data collection per platform
  preprocessing/    # cleaning, deduplication, normalization
  embeddings/       # sentence transformer inference
  clustering/       # BERTopic and topic management
  velocity/         # trend tracking and spike detection
  sentiment/        # polarity scoring and aggregation
  classification/   # product vs. fulfilment complaint separation
  features/         # attribute extraction and feature-level sentiment
  integrity/        # review bombing and fake review detection
  prediction/       # early warning model
  dashboard/        # Power BI data prep and export
data/               # raw/processed datasets (gitignored)
tests/              # unit and functional tests
pyproject.toml      # project metadata and dependencies
README.md           # this file
```

---

## 📊 Evaluation Strategy

The predictive model is evaluated on a **held-out temporal test set** — reviews from the most recent time window are reserved for testing to simulate real-world deployment conditions and prevent data leakage.

Key metrics:
- **Precision / Recall / F1** on 30-day rating drop prediction
- **Mean lead time**: average days between cluster velocity spike and confirmed rating drop
- **False positive rate**: how often does the watchlist flag products that don't decline?
- **Cluster coherence**: manual spot-check and silhouette scoring of issue groups

---

## 📝 Getting Started

1. Clone the repo and install dependencies via `uv` or `pip` using `pyproject.toml`
2. Activate your Python environment (3.12+)
3. Configure API credentials for Reddit (PRAW) in `.env`
4. Explore `notebooks/01_data_exploration.ipynb` for initial analysis
5. Run the ingestion pipeline: `python src/ingestion/run.py`
6. Move validated code into `src/` modules and add tests under `tests/`

---

## 📎 Notes for Contributors

See `.github/copilot-instructions.md` for guidance on working with AI assistance in this repo.

---

*Built as a portfolio demonstration of applied NLP, unsupervised learning, time series analysis, and ML engineering on real-world consumer signal data.*