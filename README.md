# agy-configs

Personal Antigravity customizations: tiered Hydrogen/Carbon worker agents, routing rules, and skills.

## Structure

```text
agy-configs/
├── plugin.json                    # Plugin manifest
├── agents/                        # Worker subagents (.md)
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
* **Or install locally in Antigravity (Windows):**
  Copy the plugin files into `%USERPROFILE%\.gemini\config\plugins\agy-configs\` (Antigravity's plugin scanner requires a real directory rather than a Windows directory junction):
  ```powershell
  Copy-Item -Path "plugin.json","README.md","agents","skills","rules" -Destination "$env:USERPROFILE\.gemini\config\plugins\agy-configs" -Recurse -Force
  ```
