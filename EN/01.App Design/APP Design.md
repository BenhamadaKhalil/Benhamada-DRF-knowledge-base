---
title: "APP Design"
section: "01.App Design"
stage: 1
status: evergreen
tags: [drf, django, app-design, architecture, planning]
updated: 2026-08-16
---

# APP Design

## Core Concept

You are building a **Recipe API** in Django REST Framework with:

- user registration and token authentication
- 19 REST endpoints for recipes, tags, and ingredients
- a browsable admin and browsable API as test/fallback interfaces

The point is not just to build endpoints once, but to build a repeatable mental model for API design, implementation, verification, and hardening.

## Why this stage matters

Before Stage 2, this section answers a single question: *what exactly are we trying to serve and why is this shape useful?*

If your API shape is unclear, every test and implementation decision becomes ad hoc. The endpoint design phase is your load-bearing beam.

## Implementation

- Read [[Endpoint API.canvas|Endpoint API]] first and trace the 19 endpoints end to end.
- Capture each user journey:
  - register and authenticate
  - create/read/update/delete recipes, tags, ingredients
  - upload media for recipes
- Decide request/response contracts before coding:
  - status codes
  - payload shape
  - permission expectations

## Why people struggle here

- jumping to implementation without endpoint contracts
- changing endpoint names after tests exist
- forgetting non-functional goals (performance, consistency, security)

## Gotchas

- REST design is the easiest part to get partially right and later regret.
- If you are unsure, create a simple endpoint map first and keep it stable.

## What you should finish before Stage 2

- You can describe each endpoint in one line.
- You can explain each endpoint owner (`user`, `recipe`, `tag`, `ingredient`).
- You can sketch the dependency order for implementation in five-minute notes.
- You can open [[Endpoint API.canvas|Endpoint API]] and point to every endpoint confidently.

## Related

- [[Endpoint API.canvas|Endpoint API]]
