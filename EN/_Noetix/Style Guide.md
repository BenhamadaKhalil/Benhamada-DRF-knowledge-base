---
title: Style Guide
type: system
tags: [noetix, system, meta]
updated: 2026-08-16
---

# ✍️ Noetix Style Guide

> The rules that make 150 notes feel like one book instead of 150 notes.

---

## 1. Every note answers four questions

In this order, always:

| Order | Question                  | Section heading      |
| ----- | ------------------------- | -------------------- |
| 1     | What *is* this thing?     | `## 🎯 Core Concept`  |
| 2     | Why does it exist?        | `## 🤔 Why`           |
| 3     | How do I write it?        | `## 🛠️ Implementation` |
| 4     | How does it bite me?      | `## ⚠️ Gotchas`       |

A note missing #2 and #4 is a transcript, not a note. **The value is entirely in #2 and #4** — anyone can get #1 and #3 from the docs.

---

## 2. Frontmatter

Real YAML frontmatter at the very top, before anything else:

```yaml
---
title: Implementing Tag Creation
section: 13 · Build Tag API
stage: 3
status: evergreen
tags: [drf, serializers, nested-write, tags]
updated: 2026-08-16
---
```

| Field     | Values                                          |
| --------- | ----------------------------------------------- |
| `stage`   | `1` foundations · `2` spine · `3` API · `4` production · `5` Django fundamentals |
| `status`  | `seed` (stub) · `growing` (usable) · `evergreen` (trusted) |
| `tags`    | lowercase, hyphenated, no `#` prefix            |

> [!NOTE] Legacy notes
> Older notes carry metadata in a ```` ```yaml ```` fenced block instead of real frontmatter. Both are fine to read; **new notes use real frontmatter** so Obsidian's tag pane, search filters and graph view actually work.

---

## 3. Callouts carry meaning

Use them consistently or they become decoration:

> [!INFO] Context you'd otherwise have to look up

> [!TIP] A shortcut that saves real time

> [!WARNING] You will get this wrong at least once

> [!CAUTION] This can cost you data or security

> [!EXAMPLE] A concrete case

> [!QUESTION] Something to test yourself on

---

## 4. Code blocks

- **Always** tag the language: `python`, `bash`, `yaml`, `http`, `text`.
- One command per `bash` block — the app renders a Run button per block.
- Show the *file path* as a comment on line one when the file matters:

```python
# app/recipe/serializers.py
class RecipeSerializer(serializers.ModelSerializer):
    ...
```

- Show **the failing version too** when the failure is the lesson:

```python
# ❌ silently returns every user's recipes
queryset = Recipe.objects.all()

# ✅ scoped to the token owner
def get_queryset(self):
    return self.queryset.filter(user=self.request.user)
```

---

## 5. Diagrams

Three tools, three jobs:

| Tool          | Use for                                          | Renders in            |
| ------------- | ------------------------------------------------ | --------------------- |
| **Mermaid**   | flow, sequence, state — anything you'll edit      | Obsidian **and** GitHub |
| **SVG** (`assets/`) | reference diagrams that reward craft       | Obsidian **and** GitHub |
| **Canvas**    | spatial exploration, "everything at once" boards  | Obsidian only         |

Mermaid is the default. Reach for SVG only when the diagram is worth hand-placing.

Mermaid nodes use the Noetix palette so diagrams match the SVGs:

```
style X fill:#101E3A,stroke:#6EE7F9,color:#E6EAF4   %% cyan   — flow / neutral
style X fill:#1A1633,stroke:#A78BFA,color:#E6EAF4   %% violet — DRF internals
style X fill:#0F2A24,stroke:#34D399,color:#E6EAF4   %% green  — success / data
style X fill:#2B1F10,stroke:#FBBF24,color:#E6EAF4   %% amber  — caution / config
style X fill:#2B1218,stroke:#FB7185,color:#E6EAF4   %% rose   — failure / error
```

---

## 6. Linking

- Link on **first mention** of a concept, not every mention.
- Prefer `[[Note Name|readable phrase]]` over bare links inside sentences.
- End every note with a `## 🔗 Related` list of 3–6 links.
- A `[[link]]` to a note that doesn't exist yet is **a feature** — it's a to-do with a location.

---

## 7. Naming

- Files inside a numbered section start with their order: `4.Implementing Tag Creation.md`
- Section folders keep their number prefix so file explorer order matches reading order.
- No two notes share a filename anywhere in the vault. Obsidian resolves `[[links]]` by filename — duplicates create silent mis-links.

---

## 8. Tone

Write the note you wish you'd found at 2am with a red test.

- Second person, present tense: *"You register the model, and `/admin/` exists."*
- Say the thing that's actually true, including when the framework is annoying.
- No filler transitions. Delete "In this section, we will explore…"
- Emoji as **wayfinding** (section markers, status), not as punctuation.

---

## 🔗 Related

- [[Note Template]]
- [[Map of Content]]
- [[Noetix Home]]
