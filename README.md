# agy-configs

Personal Antigravity customizations: tiered Hydrogen/Carbon worker agents, routing rules, and skills.

## Structure

```text
agy-configs/
├── plugin.json                    # Plugin manifest
├── agents/                        # Worker & orchestrator subagents (.md)
│   ├── orchestrator.md            # Hydrogen High orchestrator (model: flash)
│   ├── hydrogen-low.md            # model: flash_lite (swarm: gemini-4-hydrogen-low)
│   ├── hydrogen-medium.md         # model: flash      (swarm: gemini-4-hydrogen-medium)
│   ├── hydrogen-high.md           # model: flash      (swarm: gemini-4-hydrogen-high)
│   ├── hydrogen-xhigh.md          # model: flash      (swarm: gemini-4-hydrogen-xhigh)
│   ├── carbon-low.md              # model: pro        (swarm: gemini-4-carbon-low)
│   ├── carbon-medium.md           # model: pro        (swarm: gemini-4-carbon-medium)
│   ├── carbon-high.md             # model: pro        (swarm: gemini-4-carbon-high)
│   └── code-reviewer.md           # General code review subagent
├── skills/
│   ├── repo-audit/SKILL.md        # Repository health check skill
│   └── tiered-dispatch/SKILL.md   # Hydrogen-first routing & Carbon escalation playbook
└── rules/
    └── AGENTS.md                  # Always-on orchestrator, PR-only, and routing rules
```

## Routing & Orchestration Policy

1. **Orchestrator subagent for new/unrelated/complex tasks:** When a new task, unrelated chat topic, or complex task arrives, invoke an **orchestrator** subagent on Hydrogen High (`orchestrator` agent / `flash`) to drive the entire task end-to-end.
2. **All initial work goes to Hydrogen:** The orchestrator delegates implementation to the lowest-tier Hydrogen worker (`hydrogen-low`, `hydrogen-medium`, `hydrogen-high`, `hydrogen-xhigh`) matching the task's complexity.
3. **Automatic Carbon escalation:** If a Hydrogen worker returns `STATUS: ESCALATE` or remains blocked after retries, work automatically escalates to `carbon-low`, `carbon-medium`, or `carbon-high`.

## Usage

* **Install from GitHub (Antigravity CLI):**
  ```powershell
  agy plugin install https://github.com/amitkp57/agy-configs
  ```
* **Or install locally in Antigravity (Windows):**
  Copy the plugin files into `%USERPROFILE%\.gemini\config\plugins\agy-configs\` (Antigravity's plugin scanner requires a real directory rather than a Windows directory junction):
  ```powershell
  Copy-Item -Path "plugin.json","README.md","agents","skills","rules" -Destination "$env:USERPROFILE\.gemini\config\plugins\agy-configs" -Recurse -Force
  ```
