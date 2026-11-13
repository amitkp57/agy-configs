# agy-configs

Personal Antigravity customizations: custom agents, skills, and rules.

## Structure

```text
agy-configs/
├── plugin.json              # Plugin manifest
├── agents/                  # Custom subagents & main agents (.md)
│   └── code-reviewer.md
├── skills/                  # Reusable on-demand skills
│   └── repo-audit/
│       └── SKILL.md
└── rules/                   # Always-on rules (when loaded as a plugin)
    └── AGENTS.md
```

## Usage

* **Install from GitHub (Antigravity CLI):**
  ```powershell
  agy plugin install https://github.com/amitkp57/agy-configs
  ```
* **Or use locally in Antigravity:**
  * Copy or clone this repo into `%USERPROFILE%\.gemini\config\plugins\agy-configs\`, **or**
  * Reference `skills\` from `%USERPROFILE%\.gemini\config\skills.json` via `"entries"`.
