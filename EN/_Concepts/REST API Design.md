---
title: REST API Design
type: concept
tags: [concept, rest, api-design, http]
updated: 2026-08-16
---

# 🏛️ REST API Design

REST means **resources identified by URLs, manipulated with HTTP verbs.** The URL says *what*; the method says *what to do with it*.

```text
✅ GET    /recipes/          list
✅ POST   /recipes/          create
✅ GET    /recipes/5/        read one
✅ PATCH  /recipes/5/        update
✅ DELETE /recipes/5/        delete

❌ GET  /getRecipes/
❌ POST /createRecipe/
❌ POST /recipes/5/delete/
```

If your URL contains a verb, the method isn't doing its job.

| Principle | In practice |
| --- | --- |
| **Nouns, plural** | `/recipes/`, not `/recipe/` or `/getRecipe/` |
| **Nest to show ownership** | `/recipes/5/upload-image/` |
| **Verbs carry the action** | GET reads, POST creates, PATCH modifies, DELETE removes |
| **Status codes carry the outcome** | `201` on create, `204` on delete, `400` on bad input |
| **Safe methods don't mutate** | `GET`, `HEAD`, `OPTIONS` — never change state |
| **Idempotent methods can repeat** | `PUT`, `DELETE` twice = same as once |
| **Stateless** | every request carries its own credentials |

## Design around the user's task, not your tables

The clearest example in this project is that **there is no `POST /tags/`**. Tags are created inside a recipe payload:

```json
POST /recipes/  {"title": "Curry", "tags": [{"name": "Thai"}]}
```

A separate tag-create endpoint would force the client into three round trips and a race condition, to manage identity it never asked about. The user is writing a recipe — not curating a taxonomy.

> [!TIP] Not offering an action is a design decision
> An endpoint that doesn't exist can't be misused, can't create duplicates, and needs no tests.

## When REST doesn't fit

Some operations genuinely aren't CRUD: `/api/user/token/`, `/export/`, a webhook receiver. Model those as an [[APIView]] and stop worrying about purity — a forced-REST URL for an inherently RPC action is worse than an honest one.

![[api-endpoint-map.svg]]

---

## 🔗 Deeper

- [[1.Recipe API Endpoints]] · [[1.Tag API — Design and Endpoints]]
- [[1.HTTP Status Codes]] · [[PATCH vs PUT]] · [[12.Versioning an API]]
