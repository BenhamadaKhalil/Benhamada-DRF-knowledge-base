---
title: Changelog
type: system
tags: [noetix, system, meta]
updated: 2026-08-16
---

# 📜 Changelog

---

## 2026-08-16 — `v1.0` Noetix

The vault becomes a system.

**Identity**
- Rebranded the knowledge base as **Noetix by KB**
- New `_Noetix/` layer: [[Noetix Home]], [[Map of Content]], [[Learning Path]], [[Style Guide]], [[Note Template]], this changelog
- New README built around the Noetix banner

**Visuals**
- New `assets/` SVG library — banner, request lifecycle, view hierarchy, project architecture, data model ERD, endpoint map, token auth flow, learning path
- Mermaid diagrams added throughout the new sections
- Canvas boards: Noetix Atlas, Endpoint API (rebuilt), Request Lifecycle, Decision Trees

**Content added**
- `15.DEPLOYMENT` — was an empty "Soon in summer!" placeholder, now six notes covering production compose, uWSGI, nginx, secrets, deploying, and a post-deploy checklist
- `13.Build tag API` — added the missing notes 1–2 (design + model)
- `12.Build ingredian API` — added the full design walkthrough
- `17.Production DRF` — 13 new notes: pagination, throttling, caching, N+1, permissions, JWT, signals, transactions, Celery, security, logging, versioning
- `18.Reference` — 9 cheat sheets
- `19.Interview and Real World` — interview Q&A, common bugs, ADRs, debugging decision tree
- `16.all fixes` — added "Errors You Will Actually Hit"
- `_Concepts/` — a new concept layer: 29 short definition notes for the ideas that recur across sections, each linking out to the note that treats it properly

**Fixes**
- 78 dangling `[[Related Concepts]]` links in the original notes pointed at pages that were never written. The `_Concepts/` layer now resolves every one of them, using Obsidian `aliases:` so several spellings (`[[TokenAuthentication]]`, `[[Token Authentication]]`, `[[Authentication in DRF]]`) land on a single page
- `Endpoint API.canvas` pointed at an image outside the vault — rebuilt from scratch
- Two different notes were both called `Viewer.md`, which made `[[Viewer]]` links ambiguous — renamed to *Django Refresher* and *DRF Mastery Roadmap*
- `3.nstalling & Enabling DRF Spectacular.md` → `3.installing & Enabling DRF Spectacular.md`
- Real YAML frontmatter added to every note so tags, search filters and graph view work

---

## Before

Personal study notes taken while working through a Django REST Framework course — one note per video, summarised from subtitles.
