# General Workspace Rules

- Keep changes focused, minimal, and well-tested.
- Never commit secrets, API keys, or personal tokens.
- **Pull Requests only (no direct commits to main):**
  - Never commit or push directly to `main` (or `master`).
  - Always create a feature branch (`git checkout -b <type>/<short-name>`), commit changes there, push the branch (`git push -u origin <branch>`), and open a Pull Request (`gh pr create` / `& "C:\Program Files\GitHub CLI\gh.exe" pr create`).
  - Wait for review/merge before deleting the branch and updating `main`.
- **Sync `agy-configs` plugin edits:** Whenever you edit `plugin.json`, `README.md`, `agents/`, `skills/`, or `rules/` in `C:\Users\amitk\Antigravity`, always sync the installed global plugin copy in `%USERPROFILE%\.gemini\config\plugins\agy-configs` before finishing (and remove any deleted/renamed files from the destination copy if applicable):
  ```powershell
  Copy-Item -Path "C:\Users\amitk\Antigravity\plugin.json","C:\Users\amitk\Antigravity\README.md","C:\Users\amitk\Antigravity\agents","C:\Users\amitk\Antigravity\skills","C:\Users\amitk\Antigravity\rules" -Destination "$env:USERPROFILE\.gemini\config\plugins\agy-configs" -Recurse -Force
  ```

# Tiered Worker Routing (Hydrogen-First, Automatic Carbon Escalation)

## 1. Always Start on Hydrogen
- Delegate all implementation, debugging, investigation, and file-editing work to the **Hydrogen** workers (`hydrogen-low`, `hydrogen-medium`, `hydrogen-high`, `hydrogen-xhigh`) rather than doing heavy work inline or jumping straight to Carbon.
- Only quick read-only context lookups (a couple of file reads or searches to classify difficulty and write a clear brief) should happen in the coordinator before delegating.

## 2. Difficulty Selection Rubric
Choose the lowest Hydrogen tier that fits the task's complexity:
- **`hydrogen-low`** (`gemini-4-hydrogen-low`): File discovery, log/code summaries, typo or doc updates, formatting, boilerplate, trivial single-file edits.
- **`hydrogen-medium`** (`gemini-4-hydrogen-medium`): Self-contained helpers/components, adding unit tests, straightforward bug fixes with a clear repro.
- **`hydrogen-high`** (`gemini-4-hydrogen-high`): Multi-file features, interface/API refactors, non-trivial cross-module debugging.
- **`hydrogen-xhigh`** (`gemini-4-hydrogen-xhigh`): Hardest Hydrogen work—stubborn test failures, deep edge cases, complex multi-module changes.

## 3. Automatic Escalation to Carbon
- **Never send initial work directly to Carbon.**
- **Automatically escalate** to Carbon without waiting for user confirmation whenever:
  1. A Hydrogen worker returns `STATUS: ESCALATE`, gets blocked, or fails verification after retries.
  2. Stepping up within Hydrogen (`hydrogen-high` / `hydrogen-xhigh`) still fails to resolve the issue.
- Pick the Carbon escalation tier based on failure complexity:
  - **`carbon-low`** (`gemini-4-carbon-low`): First escalation—subtle logic bug, ambiguous spec, or second-opinion fix after a Hydrogen miss.
  - **`carbon-medium`** (`gemini-4-carbon-medium`): Cross-cutting architecture problems, concurrency/state bugs, or multi-subsystem redesigns.
  - **`carbon-high`** (`gemini-4-carbon-high`): Final escalation—repeated failures at lower tiers, deep root-cause debugging, security-critical or high-risk changes.
- Always pass the full handoff context when escalating (original goal, files modified, exact commands/errors, and what Hydrogen already tried) so Carbon does not repeat failed steps.
