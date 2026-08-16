---
title: Learning Path
type: moc
tags: [noetix, moc, learning-path]
updated: 2026-08-16
---

# 🧭 Learning Path

> [!TIP] How to use this page
> **First pass:** read top to bottom, building the project as you go. Don't skip Stage 1 — it is the least exciting and the most load-bearing.
> **Every pass after:** ignore this page, use [[Map of Content]] and search.

![[learning-path.svg]]

---

## The shape of the journey

```mermaid
flowchart LR
    A["🧱 Stage 1<br/>Foundations<br/><i>environment + tests</i>"] --> B["🦴 Stage 2<br/>The spine<br/><i>db + users + admin</i>"]
    B --> C["🔌 Stage 3<br/>The API<br/><i>the same loop ×5</i>"]
    C --> D["🚀 Stage 4<br/>Production<br/><i>make it survive</i>"]
    D -.->|"and then you<br/>build your own thing"| E["🌍 Your project"]

    style A fill:#101E3A,stroke:#6EE7F9,color:#E6EAF4
    style B fill:#1A1633,stroke:#A78BFA,color:#E6EAF4
    style C fill:#0F2A24,stroke:#34D399,color:#E6EAF4
    style D fill:#2B1F10,stroke:#FBBF24,color:#E6EAF4
    style E fill:#131A2E,stroke:#24304D,color:#E6EAF4
```

---

## Stage 1 — Foundations

**Goal:** `docker compose up` works, `flake8` is clean, one test runs and fails on purpose.

| #   | Section         | What you actually learn                                          | Done when                              |
| --- | --------------- | ---------------------------------------------------------------- | -------------------------------------- |
| 01  | App Design      | The 19 endpoints you're heading toward                            | You can draw the endpoint map yourself |
| 02  | System Setup    | Docker, Docker Compose, an editor, Git                            | `docker --version` works               |
| 03  | Project Setup   | Dockerfile, compose, requirements, flake8, `startproject`         | The dev server answers on :8000        |
| 04  | GitHub Actions  | CI that runs your tests and linter on every push                  | Green tick on GitHub                   |
| 05  | TDD             | Red → green → refactor, `TestCase`, `SimpleTestCase`, mocking     | You write the test *first* once        |

> [!WARNING] The trap in Stage 1
> Almost everyone rushes this to "get to the real Django". Then they spend Stage 3 debugging their environment instead of their code. **Docker + CI + flake8 first.** It is the whole point.

---

## Stage 2 — The spine

**Goal:** a Postgres-backed project with a custom user model and a working admin.

| #   | Section            | The one idea that matters                                                            |
| --- | ------------------ | ------------------------------------------------------------------------------------ |
| 06  | Configure Database | Postgres starts *slower* than Django → `wait_for_db` is a real production pattern      |
| 07  | Custom User Model  | Swap it on **day one**. Swapping later is a migration nightmare.                       |
| 08  | Django Admin       | Free CRUD UI. Register a model, get a back office for nothing.                          |
| 09  | API Documentation  | `drf-spectacular` reads your code → docs can never go stale                             |

```mermaid
flowchart TD
    S["settings.py<br/>AUTH_USER_MODEL = 'core.User'"] --> M["core/models.py<br/>User + UserManager"]
    M --> MG["makemigrations → migrate"]
    MG --> A["core/admin.py<br/>UserAdmin"]
    A --> AD["/admin/ works"]
    M --> API["user app<br/>serializers + views"]
    API --> EP["/api/user/create/<br/>/api/user/token/<br/>/api/user/me/"]

    style S fill:#1A1633,stroke:#A78BFA,color:#E6EAF4
    style M fill:#0F2A24,stroke:#34D399,color:#E6EAF4
    style EP fill:#101E3A,stroke:#6EE7F9,color:#E6EAF4
```

---

## Stage 3 — Building the API

**Goal:** 19 endpoints, all tested.

Every single one of these five sections is the *same four steps*. Once you see the pattern, the rest is typing:

```mermaid
flowchart LR
    T["1️⃣ Write the test<br/><i>it fails — good</i>"] --> M["2️⃣ Model / serializer"]
    M --> V["3️⃣ View + URL"]
    V --> G["4️⃣ Test passes"]
    G -->|next feature| T

    style T fill:#2B1218,stroke:#FB7185,color:#E6EAF4
    style G fill:#0F2A24,stroke:#34D399,color:#E6EAF4
```

| #   | Section        | New concept introduced                                       |
| --- | -------------- | ------------------------------------------------------------ |
| 10  | User API       | Token auth, `write_only`, `IsAuthenticated`                  |
| 11  | Recipe API     | `ModelViewSet`, routers, `get_serializer_class()`            |
| 12  | Tag API        | Nested serializers, `get_or_create` on write                  |
| 13  | Ingredient API | Refactoring two viewsets into one base class                  |
| 14  | Image API      | `@action`, `MultiPartParser`, media files                     |
| 15  | Filtering      | Query params, `OpenApiParameter`, comma-separated IDs         |

---

## Stage 4 — Production

**Goal:** it survives contact with the internet.

| #   | Section         | What it covers                                                            |
| --- | --------------- | ------------------------------------------------------------------------- |
| 16  | Deployment      | Production compose, uWSGI, nginx proxy, env vars, `collectstatic`          |
| 17  | All Fixes       | The Django refresher + the errors you will actually hit                     |
| 18  | Production DRF  | Pagination, throttling, caching, N+1, permissions, JWT, transactions, Celery |
| 19  | Reference       | Cheat sheets you will open weekly for years                                 |
| 20  | Interview       | Q&A, ADRs, decision trees, war stories                                      |

---

## A realistic schedule

| Pace                        | Stage 1 | Stage 2 | Stage 3 | Stage 4 |
| --------------------------- | ------- | ------- | ------- | ------- |
| **Sprint** (full-time)      | 2 days  | 2 days  | 4 days  | 3 days  |
| **Steady** (2h/day)         | 1 week  | 1 week  | 2 weeks | 2 weeks |
| **Weekend** (Sat mornings)  | 3 weeks | 3 weeks | 6 weeks | 5 weeks |

> [!NOTE] Honest advice
> Stage 3 feels fast because it repeats. Stage 4 feels slow because nothing repeats. Budget accordingly — and do not treat Stage 4 as optional. The gap between "works on my machine" and "works" is exactly Stage 4.

---

## Related

- [[Noetix Home]]
- [[Map of Content]]
- [[DRF Mastery Roadmap]] — what to learn *after* this vault
