# AI in the News: Topic, Entity & Sentiment Analysis

NLP analysis of ~200,000 AI-related news articles to understand how AI is discussed across industries and companies.
Final project for *Next-Gen NLP*, University of Chicago, March 2026.

## Research questions
1. Which industries and companies are most likely to be impacted by AI over the next several years?
2. How are they impacted (positive, negative, or uncertain)?
3. What factors appear to influence successful or unsuccessful AI adoption?

## Pipeline

| Step | Method | Notebook |
|---|---|---|
| Load & clean | Drop duplicate `text`; keep articles with 800 < length < 100,000 characters | `01` |
| Relevance filter | Keyword-based `rel_score` (AI terms + impact terms); keep `rel_score >= 3` | `01` |
| Preprocessing | Lowercase, strip punctuation/numbers, collapse whitespace → `clean_text` | `02` |
| Topic modeling | TF-IDF (8,000 features) + NMF (10 topics) on a 30,000-article sample; noise topic dropped | `02` |
| Entity extraction | spaCy `en_core_web_sm` NER → `ORG` and `PRODUCT` entities | `02` |
| Sentiment | VADER compound score (thresholds ±0.05) → Positive / Neutral / Negative | `02` |
| Aggregation | Sentiment by topic, by entity, and by month | `02` |

## Key findings
- AI coverage is dominated by large technology companies (Microsoft, Google, IBM, …).
- Innovation- and product-focused topics skew more positive.
- Regulation, risk, and workforce topics skew more cautious or negative.
- Adoption appears linked to clear use cases, strong data infrastructure, and regulatory clarity.

## Repository structure
```
01_data_cleaning_and_filtering.ipynb   # load, dedupe, length + relevance filtering
02_topics_entities_sentiment.ipynb     # NMF topics, NER, sentiment, plots
Final_Project.pptx                     # final presentation
Final_Project_annotated.pdf            # annotated slides
```

## Data
The dataset is loaded directly from a public Google Cloud Storage bucket in notebook 01:
`https://storage.googleapis.com/msca-bdp-data-open/news_final_project/news_final_project.parquet`

Columns used: `text`, `date`, `language` (plus `title` for topic inspection).
Data files are not stored in this repo.

## Running it
The notebooks were developed in **Google Colab** and use Google Drive to pass data between them
(notebook 01 saves `df_rel.parquet` to `MyDrive/NLP final project/`; notebook 02 loads it).
To run locally, replace the `drive.mount(...)` cells and Drive paths with local paths.

```bash
pip install -r requirements.txt
python -m spacy download en_core_web_sm
```

## Author
Sadaf Khan
