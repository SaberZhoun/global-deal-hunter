# GLOBAL DEAL HUNTER | 全球羊毛情报中心

Deal-hunting app for Chinese-speaking users who chase free AI subscriptions, free VPS / cloud credits, and free domains.

Prototype is live in Muse as a space: first patrol found **31 deals** — AI 5, VPS 4, domains 5, airdrops 5, U-cards 6, other 6 — with Chinese-first cards, favorites, mark-claimed, manual add, search, and patrol logs. Patrol runs every 2 hours.

## Problem
- Deals are scattered across X, expire in hours
- Half are already claimed out or scams
- Almost all info is in English

## What Devin will build (production version)
A real web app + backend, not a static space:
- Patrol X, RSS and official offer pages every 2 hours
- Verify each deal against the original page
- Translate to clear Chinese first
- Flag expired / scam deals
- Save, mark claimed, alerts

## Verification pipeline (tiered, no brute-force scraping)
1. New deal: one lightweight check of the official offer page / original X post
2. Re-check only near expiry, or when a user flags it claimed-out / dead
3. Most deals verified from source link, status text, and community reports
4. Cloud browser only for ambiguous deals, a few at a time

## Database schema
- `deals(id, title_zh, title_en, source_url, category, value, deadline, status, difficulty, created_at)`
  - status: `verified` / `pending` / `expired` / `scam`
- `sources(id, name, type, last_checked)`
- `users(id, saved_deals, alerts)`

## Architecture
- Frontend: Chinese-first deal cards, search, filter by category / status, alerts
- Backend: web app + scheduled worker (every 2h) + Postgres
- Integrations: GitHub, X search, RSS

Built by July / Nolan Zhou. This repo is the proof-of-work for the Poke x Devin Bots promotion: pitch your idea, get a free Devin Bot to build it.
