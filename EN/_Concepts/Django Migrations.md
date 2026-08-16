---
title: Django Migrations
type: concept
tags: [concept, django, migrations, database]
updated: 2026-08-16
---

# 🔄 Django Migrations

**Version control for your database schema.** Each migration is a Python file describing one change; Django tracks which have been applied in the `django_migrations` table.

```bash
docker compose run --rm app sh -c "python manage.py makemigrations"
```

```bash
docker compose run --rm app sh -c "python manage.py migrate"
```

| Command | Does |
| --- | --- |
| `makemigrations` | diffs your models against the last migration → writes a new file |
| `migrate` | applies unapplied migrations to the database |
| `showmigrations` | which are applied |
| `sqlmigrate core 0002` | the exact SQL a migration will run |
| `migrate core 0001` | roll **back** to that point |

> [!WARNING] Migrations are code — commit them
> A `models.py` change without its migration means everyone else's database silently diverges from yours, and CI fails on a machine that isn't yours.

## The situations you'll hit

| Situation | Do this |
| --- | --- |
| "non-nullable field without a default" | add `null=True`, or `default=…` |
| Need to backfill data | `makemigrations --empty` + a `RunPython` operation |
| Wrong migration, not pushed | `migrate core 0001`, delete the file, regenerate |
| Wrong migration, already pushed | **write a new forward migration** — never rewrite shared history |
| `InconsistentMigrationHistory` after adding a custom user model | you migrated before setting `AUTH_USER_MODEL`. Dev: drop the volume. |

## In production

`run.sh` runs `migrate` on every container start, so each release applies its own schema changes.

> [!CAUTION] Rolling back code does not roll back the schema
> If a deploy included a destructive migration (dropped or renamed column), `git checkout` leaves you with a schema the old code doesn't understand. For anything that matters, do it additively: add a nullable column, backfill, then remove the old one in a *later* release.

---

## 🔗 Deeper

- [[10.Database Migrations]] · [[7.Model Field Reference]]
- [[6.Post-Deploy Checklist]] · [[Django Models]]
