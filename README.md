# agy-configs

Personal Antigravity customizations: tiered Hydrogen/Carbon worker agents, routing rules, and skills.

## Structure

```text
agy-configs/
├── plugin.json                    # Plugin manifest
├── agents/                        # Worker subagents (.md)
│   ├── hydrogen-low.md            # gemini-4-hydrogen-low
│   ├── hydrogen-medium.md         # gemini-4-hydrogen-medium
│   ├── hydrogen-high.md           # gemini-4-hydrogen-high
│   ├── hydrogen-xhigh.md          # gemini-4-hydrogen-xhigh
│   ├── carbon-low.md              # gemini-4-carbon-low (escalation)
│   ├── carbon-medium.md           # gemini-4-carbon-medium (escalation)
│   ├── carbon-high.md             # gemini-4-carbon-high (escalation)
│   └── code-reviewer.md           # General code review subagent
├── skills/
│   ├── repo-audit/SKILL.md        # Repository health check skill
│   └── tiered-dispatch/SKILL.md   # Hydrogen-first routing & Carbon escalation playbook
└── rules/
    └── AGENTS.md                  # Always-on Hydrogen-first & auto-escalation rules
```

## Routing Policy

1. **All initial work goes to Hydrogen** (`hydrogen-low`, `hydrogen-medium`, `hydrogen-high`, or `hydrogen-xhigh`) matched to task difficulty.
2. **Automatic Carbon escalation:** If a Hydrogen worker returns `STATUS: ESCALATE` or remains blocked after retries, work automatically escalates to `carbon-low`, `carbon-medium`, or `carbon-high`.

## Usage

* **Install from GitHub (Antigravity CLI):**
  ```powershell
  agy plugin install https://github.com/amitkp57/agy-configs
  ```
* **Or link locally across all workspaces (Windows):**
  Create a directory junction so edits in this repo take effect globally:
  ```powershell
  New-Item -ItemType Junction -Path "$env:USERPROFILE\.gemini\config\plugins\agy-configs" -Target "C:\Users\amitk\Antigravity"
  ```
