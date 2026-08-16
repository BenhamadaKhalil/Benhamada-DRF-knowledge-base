---
title: Noetix Home
type: dashboard
tags: [noetix, moc, home]
updated: 2026-08-16
---

![[noetix-banner.svg]]

> 🇸🇦 **العربية** — النسخة العربية قيد الإنشاء: [[الصفحة الرئيسية]]

> [!QUOTE] Noetix
> *noetic* — of or relating to the intellect; knowledge apprehended directly by the mind.
> This vault is not a pile of notes. It is a **system for knowing Django REST Framework.**

---

## 🧭 Start here

| If you are…                                | Go to                                                 |
| ------------------------------------------ | ----------------------------------------------------- |
| brand new — never built a Django API       | [[Learning Path]] → Stage 1                            |
| mid-course, following along                | [[Map of Content]] → find your section                 |
| stuck on a bug right now                   | [[Debugging Decision Tree]]                            |
| shipping something real                    | [[EN/19.Production DRF/0.Production Readiness Checklist\|Production Readiness Checklist]] |
| looking something up fast                  | [[EN/20.Reference/0.Reference Index\|Reference Index]]     |
| preparing for an interview                 | [[EN/21.Interview and Real World/0.Interview Index\|Interview Index]] |

---

## 🗺️ The whole picture

![[learning-path.svg]]

---

## 📚 The four stages

### Stage 1 — Foundations
> Get a reproducible environment and a failing test before you write a line of Django.

- [[APP Design]] — what we are building and why
- [[1.Required Tools]] · [[2.Verify Installation]]
- [[1.overview|Project Setup overview]] → [[5.Dockerfile|Dockerfile]] → [[6.docker-compose.yml]] → [[8.Configure Flake8]]
- [[1.Automated Testing & Linting|GitHub Actions]] → [[3.Create the Workflow Config]]
- [[Test Driven Development]] — the loop everything else runs inside

### Stage 2 — The spine
> Database, identity, admin. The parts every Django project on earth has.

- [[1.Database Configuration with PostgreSQL & Docker Compose|PostgreSQL + Docker Compose]] → [[8.Create wait_for_db|wait_for_db]] → [[10.Database Migrations|Migrations]]
- [[1.Custom User Model in Django|Custom User Model]] → [[4.Implementing a Custom User Model]] → [[7.Add “create superuser”]]
- [[1.Overview & Customization|Django Admin]] → [[4.Fix the User Change Page]]
- [[2.Automated API Documentation in Django REST Framework|drf-spectacular]] → [[4.Enabling Swagger Documentation URLs]]

### Stage 3 — Building the API
> The same loop five times: write the test, write the view, watch it go green.

- [[1.Design & Endpoints|User API]] → [[4.Authentication in Django REST Framework|Token auth]] → [[9.Implement “Manage User”]]
- [[Recipe API – Overview|Recipe API]] → [[2.APIView vs ViewSet|APIView vs ViewSet]] → [[11.Implementing Recipe Creation in DRF]]
- [[Tags Section — Detailed Summary|Tag API]] → [[Nested Serialization]]
- [[Ingredient API]] → [[Refactoring]]
- [[🖼️ Adding Images to Recipe Model (Django)|Image API]] → [[🗂️ Django Static & Media Files (with Docker)]]
- [[🧩 Filtering Recipes by Tags & Ingredients (with API Documentation)|Filtering]]

### Stage 4 — Production
> Where a course ends and a real service begins.

> [!TIP] On Django 6.1?
> Start with [[1.What's New in Django 6.1]] — it lists everything in this vault that the August 2026 release dates, and why.

- [[EN/17.Deployment/1.From Dev Compose to Production|Deployment]]
- [[EN/19.Production DRF/0.Production Readiness Checklist|Production DRF]]
- [[EN/20.Reference/0.Reference Index|Reference & cheat sheets]]
- [[EN/21.Interview and Real World/0.Interview Index|Interview & real world]]

### Stage 5 — Django underneath
> DRF is a layer on top of Django. The course teaches the layer.

- [[0.Django Core Index]] — settings, middleware, querysets, auth, caching, security, async, i18n
- [[1.What's New in Django 6.1]] ⭐ — what the August 2026 release changed

---

## 🔍 The three diagrams worth memorising

### 1. Where your bug lives
![[drf-request-lifecycle.svg]]

### 2. How much abstraction to buy
![[drf-view-hierarchy.svg]]

### 3. What the data actually looks like
![[data-model-erd.svg]]

---

## 🎯 Canvases

Open these in Obsidian for the spatial view:

- [[Noetix Atlas.canvas|🗺️ Noetix Atlas]] — the entire vault on one board
- [[Endpoint API.canvas|🔌 Endpoint map]] — all 19 endpoints
- [[Request Lifecycle.canvas|🔄 Request lifecycle]] — click through each stage
- [[Decision Trees.canvas|🌳 Decision trees]] — "which thing do I use?"

---

## 🧩 How this vault is built

- **[[0.Concept Index]]** — short definitions for every recurring idea
- **[[Map of Content]]** — every note, grouped
- **[[Style Guide]]** — the note anatomy every page follows
- **[[Note Template]]** — copy this when you add a note
- **[[Changelog]]** — what changed, when

---

## ⚡ The five commands

```bash
docker compose up
```

```bash
docker compose run --rm app sh -c "python manage.py test && flake8"
```

```bash
docker compose run --rm app sh -c "python manage.py makemigrations && python manage.py migrate"
```

```bash
docker compose run --rm app sh -c "python manage.py createsuperuser"
```

```bash
docker compose run --rm app sh -c "python manage.py startapp <name>"
```

---

*Noetix by KB · knowledge, engineered.*
