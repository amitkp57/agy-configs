---
name: repo-audit
description: >-
  Inspect repository structure, git status, recent commits, and potential setup
  issues. Use when the user asks for a quick overview or health check of a repo.
---

# Repository Audit

## Steps

1. Check git state (`git status -sb` and `git log -n 5 --oneline`).
2. List top-level project files to identify the language, package manager, and test runner.
3. Flag any uncommitted changes, missing README/gitignore, or obvious configuration gaps.
4. Summarize findings concisely with file paths.
