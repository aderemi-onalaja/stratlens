# StratLens

**StratLens** turns a single company name into an evidence-backed **SWOT**, **competitor landscape**, and **market outlook** — with citations and confidence scoring.

> Enter a business → get signal, not vibes.

## What it does
- **Company snapshot** (who they are, what they sell, where they operate)
- **SWOT analysis** (Strengths / Weaknesses / Opportunities / Threats)
- **Competitor set** (direct + adjacent) + “why these are competitors”
- **Competitor financials** (public where available; estimates clearly labelled)
- **Forecasts & drivers** (macro + industry indicators, plus implications)
- **Actionable recommendations** (prioritised next steps)

## Core principles
- **Citations-first**: every claim should be grounded in a source.
- **Confidence scoring**: High / Medium / Low per section.
- **Transparent assumptions**: especially for private-company financials.
- **Neutral tone**: no defamatory or speculative language.

---

## MVP user flow
1. User enters **company name** (+ optional website / country / industry)
2. StratLens resolves the entity (disambiguates if needed)
3. StratLens fetches sources (registries, filings, trusted web, news, macro data)
4. RAG pipeline produces:
   - Overview
   - SWOT
   - Competitors
   - Financials
   - Forecasts
   - “Actions” (next best moves)
5. User exports a one-page brief (PDF later)

---

## Tech stack (Replit-friendly)
- **App**: Node/Next.js *or* Python/FastAPI (choose one)
- **LLM**: OpenAI Responses API (tool/function calling)  [oai_citation:0‡OpenAI Platform](https://platform.openai.com/docs/api-reference/responses?utm_source=chatgpt.com)
- **Embeddings**: `text-embedding-3-large`  [oai_citation:1‡OpenAI Platform](https://platform.openai.com/docs/models/text-embedding-3-large?utm_source=chatgpt.com)
- **Storage**: Postgres (or SQLite for MVP) + vector store (pgvector/Supabase later)
- **Caching**: simple TTL cache for expensive fetches

---

## Data sources (suggested)
### Company identity / registries
- **Companies House (UK)**  [oai_citation:2‡developer.company-information.service.gov.uk](https://developer.company-information.service.gov.uk/overview?utm_source=chatgpt.com)
- **OpenCorporates (global entity lookup)**  [oai_citation:3‡api.opencorporates.com](https://api.opencorporates.com/documentation/API-Reference?utm_source=chatgpt.com)

### Public-company filings & fundamentals
- **SEC EDGAR APIs (submissions + XBRL/extracted data)**  [oai_citation:4‡SEC](https://www.sec.gov/search-filings/edgar-application-programming-interfaces?utm_source=chatgpt.com)
- **Financial Modeling Prep (peers + statements)**  [oai_citation:5‡site.financialmodelingprep.com](https://site.financialmodelingprep.com/developer/docs/stable/peers?utm_source=chatgpt.com)

### News / “what’s happening”
- **NewsAPI**  [oai_citation:6‡newsapi.org](https://newsapi.org/docs?utm_source=chatgpt.com)
- **GDELT (global events + full-text search options)**  [oai_citation:7‡gdeltproject.org](https://www.gdeltproject.org/?utm_source=chatgpt.com)

### Forecast indicators (macro + market drivers)
- **World Bank Indicators API**  [oai_citation:8‡World Bank Data Help Desk](https://datahelpdesk.worldbank.org/knowledgebase/articles/889392-about-the-indicators-api-documentation?utm_source=chatgpt.com)
- **FRED API (economic time series)**  [oai_citation:9‡FRED](https://fred.stlouisfed.org/docs/api/fred/?utm_source=chatgpt.com)
- **OECD SDMX API**  [oai_citation:10‡OECD](https://www.oecd.org/en/data/insights/data-explainers/2024/09/api.html?utm_source=chatgpt.com)

### Web search (for citations)
- **Brave Search API** (recommended default)  [oai_citation:11‡Brave](https://api-dashboard.search.brave.com/app/documentation?utm_source=chatgpt.com)

> Note: Bing Search APIs were scheduled to be decommissioned (Aug 11, 2025), so don’t build your core on them.  [oai_citation:12‡The Verge](https://www.theverge.com/news/667517/microsoft-bing-search-api-end-of-support-ai-replacement?utm_source=chatgpt.com)  
> Also, be careful with services scraping Google results: Google recently sued SerpApi over scraping allegations.  [oai_citation:13‡Reuters](https://www.reuters.com/legal/litigation/google-lawsuit-says-data-scraping-company-uses-fake-searches-steal-web-content-2025-12-19/?utm_source=chatgpt.com)

---

## Environment variables
Create a `.env` (or Replit Secrets) with:

- `OPENAI_API_KEY`
- `BRAVE_SEARCH_API_KEY`
- `COMPANIES_HOUSE_API_KEY` (if UK support)
- `OPENCORPORATES_API_KEY` (optional)
- `FMP_API_KEY` (public company peers + fundamentals)
- `NEWSAPI_KEY` (optional)
- `DATABASE_URL`

---

## Method (how outputs are generated)
- **Retrieve**: gather sources via APIs + search.
- **Rank**: keep highest quality sources; dedupe.
- **Extract**: pull facts + quotes/snippets into a “Evidence Pack”.
- **Generate**: LLM produces structured output using only the Evidence Pack.
- **Validate**:
  - every bullet has at least one citation
  - missing data becomes “Unknown” (not guessed)
  - private financials = ranges + labelled assumptions

---

## Roadmap
### v0 (MVP)
- Single input → full report (Overview, SWOT, Competitors, Actions)
- Citations + confidence scoring
- Export: copy-to-clipboard markdown

### v1
- PDF export
- Saved reports + refresh
- Alerts (news / pricing changes)

### v2
- Scenario planning (base/bull/bear)
- ICP + GTM plan generator
- Team workspaces

---

## Disclaimer
StratLens provides research assistance, not financial advice.  
Data can be incomplete or outdated; always verify with primary sources.
