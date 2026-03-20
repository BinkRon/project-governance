# Project Governance Skill for Claude Code

A Claude Code skill that sets up a complete **documentation system** and **AI team governance framework** for any project. It creates a layered document hierarchy, role-based slash commands, and cross-session continuity mechanisms.

## What it does

- **Layered Documentation System**: L0 (Constitution) → L1 (Architecture & Standards) → L2 (Module Status) → L3 (Tasks & Sprints) — from stable vision down to daily work
- **5 Role-Based Slash Commands**: `/conductor`, `/pm`, `/architect`, `/dev`, `/qa` — each with a focused perspective and clear responsibility boundary
- **Cross-Session Continuity**: Session handoff docs and conductor scanning ensure no context is lost between conversations
- **Agent Team Support**: Built-in templates for multi-agent collaboration (requirement reviews, architecture debates, etc.)

## Generated Structure

```
project-root/
├── .claude/commands/
│   ├── conductor.md      # Global state scanner + action router
│   ├── pm.md             # Product manager perspective
│   ├── architect.md      # Architecture review perspective
│   ├── dev.md            # Development implementation perspective
│   └── qa.md             # Quality assurance perspective
│
├── docs/
│   ├── L0-constitution.md        # Product vision, priorities, red lines
│   ├── L1/                       # Architecture & standards
│   │   ├── architecture.md
│   │   ├── code-standards.md
│   │   └── tech-decisions-log.md
│   ├── L2/                       # Current state tracking
│   │   ├── module-status.md
│   │   └── tech-debt.md
│   ├── L3/                       # Task layer
│   │   ├── backlog.md
│   │   ├── current-sprint.md
│   │   └── session-handoff.md
│   ├── prd/                      # PRD files
│   ├── templates/
│   │   └── prd-template.md
│   └── workplace/                # Multi-perspective discussion area
│       └── archive/
```

## Installation

Copy the skill into your project's `.claude/skills/` directory:

```bash
# Clone this repo
git clone https://github.com/BinkRon/project-governance.git /tmp/project-governance

# Copy into your project
mkdir -p .claude/skills/project-governance
cp -r /tmp/project-governance/SKILL.md .claude/skills/project-governance/
cp -r /tmp/project-governance/references .claude/skills/project-governance/
cp -r /tmp/project-governance/scripts .claude/skills/project-governance/
```

Or if you have the `skill-installer` skill:

```
Install skill from https://github.com/BinkRon/project-governance
```

## Usage

Once installed, Claude Code will automatically detect when this skill is relevant. Trigger it by saying things like:

- "Set up a documentation system for my project"
- "Help me organize my project governance"
- "Create slash commands for PM, Architect, Dev, QA roles"
- "My Claude Code workflow is messy, help me structure it"

The skill operates in two modes:

- **Mode A (Fresh Setup)**: For new projects or projects without a doc system — generates the full framework
- **Mode B (Migration)**: For projects with existing docs — audits, maps, and migrates to the governance structure

## Roles

| Role | Command | Responsibility |
|------|---------|---------------|
| Conductor | `/conductor` | Global state scanning, action routing. Doesn't execute work. |
| PM | `/pm` | Requirement structuring, backlog management, sprint planning. No tech decisions. |
| Architect | `/architect` | Architecture review, tech proposals, standards evolution. No business code. |
| Dev | `/dev` | Code implementation. Strictly follows PRD and tech proposals. |
| QA | `/qa` | Code review, standards compliance checking. Doesn't fix code. |

## License

MIT
