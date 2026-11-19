# Architecture — patrol backend

## Flow
Sources (X search, RSS, official offer pages)
  -> Scheduler (every 2 hours)
  -> Ingest + dedupe by source_url
  -> Tiered verification (see README)
  -> Postgres: deals / sources / users
  -> Frontend: Chinese-first cards, search, filter, alerts
  -> Community feedback loop: users flag claimed-out / dead -> status updates

## Tables
```sql
CREATE TABLE deals (
  id SERIAL PRIMARY KEY,
  title_zh TEXT NOT NULL,
  title_en TEXT,
  source_url TEXT UNIQUE NOT NULL,
  category TEXT NOT NULL,
  value TEXT,
  deadline TIMESTAMPTZ,
  status TEXT NOT NULL DEFAULT 'pending',
  difficulty TEXT,
  created_at TIMESTAMPTZ DEFAULT now()
);
CREATE TABLE sources (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  type TEXT NOT NULL,
  last_checked TIMESTAMPTZ
);
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  saved_deals INT[] DEFAULT '{}',
  alerts JSONB DEFAULT '{}'
);
```

## Verification
- Lightweight fetch only, no headless browser for every deal
- Re-check on expiry proximity or user flag
- Cloud browser only for ambiguous cases
