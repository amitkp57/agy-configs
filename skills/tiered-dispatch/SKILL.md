---
name: tiered-dispatch
description: >-
  Route and escalate work across Hydrogen (low, medium, high, xhigh) and Carbon
  (low, medium, high) worker agents. Use when delegating tasks by difficulty,
  running parallel swarm workers pinned to exact gemini-4-hydrogen-* /
  gemini-4-carbon-* models, or escalating failed Hydrogen tasks to Carbon.
---

# Tiered Worker Dispatch & Escalation

Use this playbook to route work **Hydrogen-first** and escalate to **Carbon** automatically on blockers or hard failures.

## 1. Choosing Subagents vs. Swarm

- **Single isolated task:** Invoke the appropriate `hydrogen-*` subagent (`hydrogen-low`, `hydrogen-medium`, `hydrogen-high`, or `hydrogen-xhigh`) via `invoke_subagent`.
- **Parallel tasks or strict model-level pinning:** Use `swarm` with the exact model ID:
  - Hydrogen: `gemini-4-hydrogen-low`, `gemini-4-hydrogen-medium`, `gemini-4-hydrogen-high`, `gemini-4-hydrogen-xhigh`
  - Carbon (escalation only): `gemini-4-carbon-low`, `gemini-4-carbon-medium`, `gemini-4-carbon-high`

## 2. Routing Flow

1. **Classify difficulty** and pick the starting Hydrogen tier (`low` -> `medium` -> `high` -> `xhigh`).
2. **Brief the Hydrogen worker** with:
   - Exact files/directories owned (and what not to touch)
   - Clear acceptance criteria and verification command (tests/build/lint)
3. **Inspect the result:**
   - If verified and complete, accept and summarize to the user.
   - If the worker returns `STATUS: ESCALATE` or fails verification after retries, **automatically escalate** to the recommended tier (upgrading to `carbon-low`, `carbon-medium`, or `carbon-high` when Hydrogen is blocked).

## 3. Escalation Handoff Template

When escalating from Hydrogen to Carbon (via `invoke_subagent` or `swarm`), include:

```text
ESCALATED FROM: <hydrogen-tier>
GOAL: <original user goal and acceptance criteria>
WHAT WAS TRIED: <summary of Hydrogen attempts>
BLOCKER / ERROR: <exact failing command, stack trace, or ambiguity>
FILES TOUCHED: <modified files or "none">
TASK: Diagnose the root cause without repeating failed guesses, implement the fix, and verify with <test/check command>.
```
