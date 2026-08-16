---
title: "GitHub Actions"
section: "05.GitHub Actions"
stage: 1
status: growing
tags: [drf, django, ci-cd, github-actions]
updated: 2026-08-16
---

# GitHub Actions

- Automation tool
- Similar to Travis-CI, GitLab CI/CD, Jenkins
- Run jobs when code changes

## Common uses
- Deployment
- Code linting
- Unit tests

## How it works

```mermaid
flowchart LR
	A[Trigger] --> B[Job] --> C[Result]
```

- Trigger: Every think happens when code changes in GitHub.
- Job: is activated when a push is triggered in GitHub.
- Result: success or fail of job
