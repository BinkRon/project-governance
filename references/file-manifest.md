# File Manifest

Complete directory structure and file list for the project governance framework.

## Directory structure

```
project-root/
├── .claude/
│   └── commands/
│       ├── conductor.md          # Global state scanner + action router
│       ├── pm.md                 # Product manager perspective
│       ├── architect.md          # Architecture review perspective
│       ├── dev.md                # Development implementation perspective
│       └── qa.md                 # Quality assurance perspective
│
├── CLAUDE.md                     # ← Append team coordination section (don't overwrite existing content)
│
├── docs/
│   ├── .governance-version       # Version stamp for upgrade detection
│   │
│   ├── L0-constitution.md        # Product vision, priorities, red lines (rarely changes)
│   │
│   ├── L1/                       # Architecture & standards (evolves by phase)
│   │   ├── architecture.md       # System architecture, module breakdown
│   │   ├── code-standards.md     # Naming, file org, error handling, forbidden patterns
│   │   └── tech-decisions-log.md # Why we chose X over Y (append-only)
│   │
│   ├── L2/                       # Current state (continuously updated)
│   │   ├── module-status.md      # Health of each module
│   │   └── tech-debt.md          # Known debt registry
│   │
│   ├── L3/                       # Task layer (high frequency changes)
│   │   ├── backlog.md            # Requirement pool + status flow rules
│   │   ├── current-sprint.md     # Active iteration plan
│   │   └── session-handoff.md    # Cross-session continuity memo
│   │
│   ├── prd/                      # One PRD file per requirement
│   │   └── (PRD-001-xxx.md, PRD-002-xxx.md, ...)
│   │
│   ├── templates/
│   │   └── prd-template.md       # Template for new PRDs
│   │
│   ├── workplace/                # Multi-perspective discussion area
│   │   └── archive/              # Archived discussions
│   │
│   └── archive/                  # Completed work archive
│       └── prd/                  # Archived PRDs
```

## File ownership matrix

Which role is primarily responsible for maintaining each file:

| File | Primary owner | Who else writes | Who reads |
|------|--------------|-----------------|-----------|
| L0-constitution.md | User (human) | — | All roles |
| L1/architecture.md | Architect | — | Dev, QA, Conductor |
| L1/code-standards.md | Architect | — | Dev, QA |
| L1/tech-decisions-log.md | Architect | — | All roles |
| L2/module-status.md | Dev | Architect (reviews) | Conductor |
| L2/tech-debt.md | Architect | Dev, QA (register new items) | Conductor |
| L3/backlog.md | PM | All roles (status updates) | Conductor |
| L3/current-sprint.md | PM | Dev (progress updates) | Conductor |
| L3/session-handoff.md | Conductor | All roles (via handoff) | Conductor |
| prd/PRD-*.md | PM (creates) | Architect (tech notes) | Dev, QA |
| workplace/*.md | Any role | Any role | Any role |

## Generation order

When creating files for a fresh project, follow this order to avoid missing references:

1. Create directory structure (all dirs)
2. L0-constitution.md (foundation — everything else references this)
3. L1 files (architecture, code-standards, tech-decisions-log)
4. L2 files (module-status, tech-debt)
5. L3 files (backlog, current-sprint, session-handoff)
6. templates/prd-template.md
7. .claude/commands/* (all five commands)
8. CLAUDE.md team section
9. docs/.governance-version
