# 📊 PersonaLens Data Model

This document outlines the full data model for PersonaLens, an AI-powered natural language sentiment and persona insight tool.

---

## 🧑‍💻 1. Users

Stores registered users who submit queries and access dashboards.

| Field         | Type        | Description                          |
|---------------|-------------|--------------------------------------|
| user_id       | UUID (PK)   | Unique identifier                    |
| name          | Text        | Full name                            |
| email         | Text        | Email address (must be unique)       |
| organization  | Text        | Affiliation (company/org)            |
| role          | Text        | User role (admin, analyst, etc.)     |
| created_at    | Timestamp   | Account creation timestamp           |

---

## 🔎 2. Queries

Each natural language query submitted by a user.

| Field           | Type        | Description                          |
|------------------|-------------|--------------------------------------|
| query_id         | UUID (PK)   | Unique identifier                    |
| user_id          | UUID (FK)   | Linked to `Users`                    |
| query_text       | Text        | Raw user-submitted query             |
| parsed_entities  | JSONB       | Entities/intents extracted by LLM    |
| created_at       | Timestamp   | Timestamp of query submission        |

---

## 💡 3. Insights

Structured summary and analytical output of each query.

| Field             | Type        | Description                          |
|--------------------|-------------|--------------------------------------|
| insight_id         | UUID (PK)   | Unique insight ID                    |
| query_id           | UUID (FK)   | Linked to `Queries`                  |
| summary_text       | Text        | Natural language output summary      |
| trend_graph_url    | Text        | URL of sentiment/trend visualization |
| sentiment_score    | Float       | Overall sentiment (-1 to 1)          |
| emotion_tags       | JSONB       | Top emotion labels & scores          |
| created_at         | Timestamp   | Insight generation timestamp         |

---

## 🌐 4. DataSources

Defines each data source connected to the system.

| Field       | Type        | Description                          |
|--------------|-------------|--------------------------------------|
| source_id    | UUID (PK)   | Unique ID                            |
| name         | Text        | Name of source (e.g., Twitter)       |
| type         | Text        | Type: "social", "news", "open_data"  |
| url          | Text        | Base URL of the source               |

---

## 📝 5. Documents

Stores all raw or pre-processed data retrieved from sources.

| Field           | Type        | Description                          |
|------------------|-------------|--------------------------------------|
| doc_id           | UUID (PK)   | Unique document ID                   |
| source_id        | UUID (FK)   | Linked to `DataSources`              |
| insight_id       | UUID (FK)   | Linked to `Insights`                 |
| url              | Text        | Source URL                           |
| title            | Text        | Document title or first sentence     |
| content          | Text        | Full document/post content           |
| author_handle    | Text        | Author ID, Twitter handle, etc.      |
| post_date        | Timestamp   | Timestamp when published             |

---

## 😊 6. Sentiments

Holds sentiment/emotion scores per document.

| Field             | Type        | Description                          |
|--------------------|-------------|--------------------------------------|
| sentiment_id       | UUID (PK)   | Unique ID                            |
| doc_id             | UUID (FK)   | Linked to `Documents`                |
| sentiment_score    | Float       | Score from -1 (neg) to +1 (pos)      |
| polarity           | Text        | "positive", "negative", or "neutral" |
| emotions           | JSONB       | e.g., {"joy": 0.6, "anger": 0.2}     |

---

## 🧠 7. Personas

Captures inferred or declared personas per document.

| Field         | Type        | Description                          |
|----------------|-------------|--------------------------------------|
| persona_id     | UUID (PK)   | Unique ID                            |
| doc_id         | UUID (FK)   | Linked to `Documents`                |
| persona_label  | Text        | e.g., “developer”, “investor”        |
| confidence     | Float       | Confidence in classification (0–1)   |

---

## 🔑 8. Keywords

Topic and keyword extraction results per document.

| Field         | Type        | Description                          |
|----------------|-------------|--------------------------------------|
| keyword_id     | UUID (PK)   | Unique ID                            |
| doc_id         | UUID (FK)   | Linked to `Documents`                |
| keyword        | Text        | Extracted keyword                    |
| weight         | Float       | Importance (TF-IDF or model-weighted)|

---

## 🔗 9. Entity Relationships

```plaintext
Users (1) ──< Queries (∞)
Queries (1) ──< Insights (∞)
Insights (1) ──< Documents (∞)
Documents (∞) ── Sources (1)
Documents (1) ── Sentiments (1)
Documents (1) ──< Personas (∞)
Documents (1) ──< Keywords (∞)

{
  "query": "What do developers think of GPT-5?",
  "summary": "Positive sentiment overall with concerns about latency and API cost.",
  "personas": [
    {
      "label": "developer",
      "sentiment_score": 0.7,
      "keywords": ["API latency", "code generation", "DevEx"],
      "sample_comments": [
        {
          "source": "Reddit",
          "author": "dev_guy_92",
          "content": "GPT-5 is amazing at writing unit tests. But the API is slower than GPT-4.",
          "sentiment": "positive"
        }
      ]
    },
    {
      "label": "media",
      "sentiment_score": 0.3,
      "keywords": ["ethics", "AI hype", "hallucination"]
    }
  ]
}
