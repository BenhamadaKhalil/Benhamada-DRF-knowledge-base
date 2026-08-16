# Contributing to Noetix

Thanks for reading closely enough to want to change something.

---

## What's most welcome

**Corrections, first and foremost.** A wrong note is worse than no note. If something here is factually incorrect, out of date, or subtly misleading, open an issue or a PR — even a one-line fix.

Also welcome:

- **A gotcha that cost you time.** The `## ⚠️ Gotchas` sections are the most valuable part of this vault. If you hit something that isn't listed, it belongs there.
- **A clearer explanation** of something that's already covered badly.
- **A diagram** that replaces three paragraphs.
- **Version drift** — DRF and Django move; if a snippet no longer works on a current version, say so.

Less useful: reformatting, restructuring folders, or adding notes that restate the official docs without adding the *why* or the *gotcha*.

---

## The house style

Read [`_Noetix/Style Guide.md`](_Noetix/Style%20Guide.md) first — it's short. The essentials:

**Every note answers four questions, in this order:**

1. `## 🎯 Core Concept` — what is this?
2. `## 🤔 Why` — why does it exist? what breaks without it?
3. `## 🛠️ Implementation` — how do I write it?
4. `## ⚠️ Gotchas` — how does it bite me?

A note with only #1 and #3 is a transcript. **The value is in #2 and #4.**

**Frontmatter** — real YAML, at the very top:

```yaml
---
title: Implementing Tag Creation
section: 13 · Build Tag API
stage: 3
status: evergreen
tags: [drf, serializers, nested-write]
updated: 2026-08-16
---
```

**Code blocks** — always tag the language. Show the file path as a comment on line one. Show the failing version too when the failure is the lesson.

**Diagrams** — Mermaid by default (renders in Obsidian *and* GitHub). Hand-authored SVG in `assets/` only when the diagram is worth hand-placing. Use the Noetix palette so everything matches:

```
%% cyan   #6EE7F9 on #101E3A — flow / neutral
%% violet #A78BFA on #1A1633 — DRF internals
%% green  #34D399 on #0F2A24 — success / data
%% amber  #FBBF24 on #2B1F10 — caution / config
%% rose   #FB7185 on #2B1218 — failure / error
```

**Tone** — write the note you wish you'd found at 2am with a red test. Second person, present tense, no filler transitions. Say the thing that's actually true, including when the framework is annoying.

---

## Adding a note

1. Copy [`_Noetix/Note Template.md`](_Noetix/Note%20Template.md)
2. Name it `N.Title.md` matching its position in the section
3. **Check the filename is unique across the whole vault** — Obsidian resolves `[[links]]` by filename, so duplicates create silent mis-links
4. Add it to [`_Noetix/Map of Content.md`](_Noetix/Map%20of%20Content.md)
5. Link it from at least one existing note — an orphan note is a note nobody finds

---

## Opening a PR

- One topic per PR
- Say what was wrong and how you know, if it's a correction
- Cite the docs, a changelog, or a reproduction for factual claims
- Update `_Noetix/Changelog.md` for anything more than a typo

---

## Reporting an error

Open an issue with:

- **Which note**, and the heading
- **What's wrong**
- **What it should say**, and how you know

"This is wrong" without the third part is still useful — file it anyway.

---

## License

By contributing you agree your work is licensed under **CC BY 4.0** (prose and diagrams) and **MIT** (code snippets), matching the rest of the repo.
