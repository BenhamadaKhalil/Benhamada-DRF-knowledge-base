---
title: Changelog
type: system
tags: [noetix, system, meta]
updated: 2026-08-16
---

# 📜 Changelog

---

## 2026-08-16 — `v1.3` Quality pass

**Fixes**
- Five notes had **backticks in their filenames** (`9.Implement \`wait_for_db\`.md` and friends). Backticks are hostile in URLs, awkward on some filesystems, and made link-checking unreliable. Renamed, and every inbound `[[link]]` rewritten. **Every wikilink in the vault now resolves — with no exceptions to explain.**
- [[Learning Path]] still carried the **pre-restructure section numbers** — it listed Project Setup as 03 when 03 is Test Driven Development, and every stage table was off by one from there. Corrected against the actual folders.
- [[Learning Path]] had no Stage 5, so section 22 was unreachable from the guide.
- [[Noetix Atlas.canvas|Noetix Atlas]] predated Django Core and didn't show it.
- Two notes were both named `11.Section Summary`, so `[[11.Section Summary]]` resolved arbitrarily. Renamed to name their sections.

---

## 2026-08-16 — `v1.2` Arabic edition

- New `AR/` edition — **47 notes**: the Noetix layer, all 19 concepts, all 9 reference sheets, all 13 Production DRF notes, and the Django 6.1 release note
- Translated **highest-value-first** rather than front-to-back: the concept and reference layers are what a reader reopens and searches; course notes are read once alongside a video
- Conventions: Arabic prose, English code and error messages, Latin technical terms with an Arabic gloss on first mention, 🇬🇧 markers on links to untranslated notes
- Arabic filenames guarantee no basename collision with `EN/` — which matters because Obsidian resolves `[[links]]` by filename
- Django 6.1 facts carried through rather than translating stale English

---

## 2026-08-16 — `v1.1` Django 6.1 + restructure

**Restructure**
- Split into `EN/` and `AR/`; `assets/` stays at the root, shared
- Sections renumbered to match the numbering the docs already used, zero-padded so the file explorer sorts correctly (it ran 1, 10, 11, … 2, 3), duplicate numbers resolved
- Typos fixed: `Desing`, `ingredian`, inconsistent `Api`

**Django 6.1** *(released 5 August 2026 — newer than the notes were written against)*
- New section 22 **Django Core**, 14 notes written from the live 6.1 docs
- [[1.What's New in Django 6.1]] — features, breaking changes, deprecations, and an upgrade runbook
- Existing notes corrected: `fetch_mode(FETCH_PEERS)` in [[4.The N+1 Problem]], `DB_CASCADE` in [[7.Model Field Reference]] *(with the warning that it skips delete signals and would orphan this project's recipe images)*, `savepoint_create`, `MAILERS`, PBKDF2 1.5M, Python 3.12, Postgres 16
- Course-record notes keep the versions the course pins and gain a callout stating current reality

**Cleanup**
- Ten notes ended with an AI assistant's closing offer (*"If you want, I can also give you…"*) — trimmed
- Two notes fully superseded by better ones in the same section — deleted

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
