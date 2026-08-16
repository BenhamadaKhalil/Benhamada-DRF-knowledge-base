---
title: Django Project Structure
aliases: ["Django Apps Architecture", "Project Structure Best Practices", "Separation of Concerns", "Django Settings Configuration"]
type: concept
tags: [concept, django, architecture, project-structure]
updated: 2026-08-16
---

# 🏗️ Django Project Structure

**Project** = the whole thing, created once. **App** = a focused module inside it, created whenever you need a new chunk of functionality.

```bash
django-admin startproject app .
```

```bash
docker compose run --rm app sh -c "python manage.py startapp recipe"
```

## The five files, in the order data flows

| File | Job | Analogy |
| --- | --- | --- |
| `models.py` | your data shape as Python classes | the spreadsheet's column headers |
| `migrations/` | generated diffs that become real SQL | git commits, for your schema |
| `serializers.py` | JSON ↔ model, plus validation | the translator at the border |
| `views.py` | what runs when a URL is hit | the person who answers the phone |
| `urls.py` | maps a path to a view | the phone number → which desk it rings |

`startapp` creates `models.py`, `views.py`, `admin.py`, `apps.py`, `tests.py` and `migrations/`. It does **not** create `urls.py` or `serializers.py` — you add those.

> [!WARNING] A new app does nothing until you register it
> ```python
> INSTALLED_APPS = [..., "recipe"]
> ```

## This project's apps

| App | Owns |
| --- | --- |
| `app/` | the project: `settings.py`, root `urls.py`, `wsgi.py` |
| `core` | shared models (User, Recipe, Tag, Ingredient), admin, `wait_for_db` |
| `user` | `/api/user/` — register, token, me |
| `recipe` | `/api/recipe/` — recipes, tags, ingredients |

**Why models live in `core` rather than in each feature app:** it keeps migrations in one place and avoids circular imports between `user` and `recipe`. It's a deliberate simplification for a project this size — a larger codebase would push models into their own apps.

## The dependency rule

```text
urls.py → views.py (thin) → serializers.py (validation) → models.py (truth) → migrations → SQL
```

Keep views thin. Business logic belongs in serializers, models, or a dedicated service module — not in a view method.

![[project-architecture.svg]]

---

## 🔗 Deeper

- [[1.Django Refresher - Projects, Apps and the Five Files]]
- [[9.Create the Django Project]] · [[2.Creating the `user` App]]
- [[Django Models]] · [[3.Architecture Decision Records]]
