# 🏁 PersonaLens Sprint Plan for Windsurf

**Timeline:** 5 Weekly Sprints  
**Goal:** Deliver PersonaLens MVP inside Windsurf with a focus on natural language querying, sentiment/persona segmentation, and insights dashboard.

---

## 🌐 Project Setup (Pre-Sprint)

**Goals:**
- Define workspace, modules, schema scaffolding in Windsurf.
- Align dev/design team on data flow and UI/UX expectations.

**Setup Tasks:**
- [ ] Create `personas-lens` workspace in Windsurf.
- [ ] Define GraphQL schema for Users, Queries, Insights, Documents, Personas, Sentiments.
- [ ] Enable source integrations (Twitter/X, Google News, Reddit) via Windsurf's fetchers.
- [ ] Design initial NL query input UI and results stub page.

---

## 🚀 Sprint 1: Natural Language Query System + Schema

**Goal:** Enable natural language input, backend parsing, and initial query handling.

### Tasks:
- [ ] Design typeahead + natural language query input UI block in Windsurf.
- [ ] Build GPT-powered parsing function using `LLM.parseQuery()`.
- [ ] Extract entities, timeframes, and intent into schema: `Query -> ParsedMetadata`.
- [ ] Store `Query` in DB with link to `User`.
- [ ] Unit test common prompts: “What do developers think of GPT-5?”

### Deliverables:
- Natural language query bar live.
- Parsed metadata preview rendered below input.
- GraphQL mutation for `createQuery`.

---

## 📡 Sprint 2: Data Ingestion & Source Mapping

**Goal:** Connect to open data sources and fetch relevant documents on query trigger.

### Tasks:
- [ ] Configure `fetch()` pipelines for:
  - Google News API
  - Twitter/X API
  - Reddit subreddit search
- [ ] Normalize response to `Document` schema with fields: `title`, `content`, `source`, `url`, `author`, `post_date`.
- [ ] Map documents to query_id via trigger.
- [ ] Deduplicate documents by `URL` and `Title`.

### Deliverables:
- Data ingestion works for at least 2 sources.
- Source preview component in frontend.
- GraphQL resolver: `getDocumentsByQuery`.

---

## 🧠 Sprint 3: Sentiment & Persona Engine

**Goal:** Run LLM-based and model-based sentiment/persona classifiers on all documents.

### Tasks:
- [ ] Use GPT-4o to extract:
  - Sentiment polarity (`-1` to `1`)
  - Emotions (`anger`, `joy`, etc.)
  - Persona (`developer`, `investor`, etc.)
- [ ] Store outputs in `Sentiments` and `Personas` tables.
- [ ] Build a confidence scoring function for persona assignment.
- [ ] Set up background job queue (`cron`) for scoring new documents.

### Deliverables:
- 90% of new documents have sentiment/persona in < 15s.
- Test suite for classification accuracy and false positives.
- GraphQL resolver: `getInsightsByQuery`.

---

## 📊 Sprint 4: Insight Dashboard & Report Generator

**Goal:** Build interactive insight dashboard + export feature.

### Tasks:
- [ ] Design dashboard layout:
  - Line graph (sentiment over time)
  - Pie chart (persona breakdown)
  - Word cloud (keywords by persona)
  - List view (documents + tags)
- [ ] Add filters: sentiment threshold, persona, date range.
- [ ] Add "Export Report" button (PDF) using `reportGenerator.downloadPDF()`.

### Deliverables:
- Fully working dashboard on query run.
- User can interactively drill into any persona or keyword.
- PDF export shows summary, charts, and top posts.

---

## 🛎 Sprint 5: Alerts, Auth, Final QA

**Goal:** Enable user preferences, alert subscriptions, and polish UX.

### Tasks:
- [ ] Implement `UserPreferences` schema (default personas, email alerts).
- [ ] Allow “Save Query” + “Alert Me” on changes in sentiment spike.
- [ ] Email digest service via Windsurf `notify()` framework.
- [ ] Implement login/signup using Windsurf Auth (email link or OAuth).
- [ ] Final QA and bug triage from user test group.

### Deliverables:
- Alerts work (spike-based or keyword watch).
- Saved queries persist in user account.
- Login flow + user profile page.
- MVP fully functional and demo-ready.

---

## 🔚 Post-MVP / Phase 2 Backlog

- Custom persona builder (using tags + embeddings)
- Slack/Notion integrations for alerts
- Fine-tuning persona classification models
- GraphQL API for external developer use
- Multi-language support

---

## 🧪 Metrics to Track During Dev

| Metric                       | Target Value |
|------------------------------|--------------|
| Avg query parse time         | < 2s         |
| Data ingest to insight time
