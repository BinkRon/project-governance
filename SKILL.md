---
name: project-governance
description: >
  Build or upgrade a project's documentation system and AI team governance framework
  for Claude Code. Use this skill whenever a user wants to: set up a documentation system
  for a new project, migrate or restructure an existing project's docs to follow best practices,
  create slash commands for AI role-based workflows (PM, Architect, Dev, QA, Conductor),
  improve their Claude Code project management process, or establish an "AI team" governance
  structure. Also trigger when users mention: "set up docs", "documentation system",
  "project governance", "AI team", "slash commands for roles", "conductor mode",
  "need a PM/architect/QA command", "organize my Claude Code workflow",
  "document system best practices", or any request to systematize how they work with Claude Code
  across sessions. Even if the user just says "help me organize my project" or
  "my Claude Code workflow is messy", this skill likely applies.
---

# Project Governance Skill

This skill sets up a complete documentation system and AI team governance framework
for Claude Code projects. It creates a layered document hierarchy, role-based slash commands,
and cross-session continuity mechanisms.

## When to use this skill

- New project needs a documentation system and governance framework from scratch
- Existing project needs its docs restructured to follow governance best practices
- User wants to create or improve role-based slash commands (PM, Architect, Dev, QA, Conductor)
- User's Claude Code workflow lacks cross-session continuity or quality control

## Two operating modes

### Mode A: Fresh setup (new project or project with no doc system)

Run the full setup flow: diagnose → confirm → generate all files.

### Mode B: Migration (project with existing docs)

Run the migration flow: audit → map → propose → confirm → execute.

Read `references/migration-guide.md` before starting Mode B.

## Step 1: Diagnose the current state

Before generating anything, understand what exists:

```bash
# Check for existing doc structures
ls -la docs/ 2>/dev/null
ls -la .claude/commands/ 2>/dev/null
cat CLAUDE.md 2>/dev/null | head -50

# Check for common doc patterns
find . -name "*.md" -not -path "*/node_modules/*" -not -path "*/.git/*" | head -30
```

Based on findings, determine:
- **No docs directory, no commands** → Mode A
- **Has docs but unstructured** → Mode B
- **Has docs AND commands already** → Mode B with extra care (preserve customizations)

Tell the user which mode you've detected and why. Get confirmation before proceeding.

## Step 2: Gather project context (both modes)

Ask the user these questions (skip any already answered in conversation):

1. **Product**: What does this product do? One sentence.
2. **Stage**: MVP / growth / mature?
3. **Tech stack**: What languages, frameworks, databases?
4. **Team model**: Is the user the sole decision-maker, or are there other humans involved?
5. **Current pain points**: What's not working now? (helps prioritize which parts of the framework to emphasize)

This information feeds into L0 (constitution) and L1 (architecture, code standards) generation.

## Step 3: Generate the framework

### Mode A: Fresh setup

Generate all files in this order:

**Phase 1 — Structural scaffolding (no user input needed beyond Step 2)**

Read `references/file-manifest.md` for the complete file list and directory structure.

Create directories and generate these files using the templates in `references/templates/`:

1. `docs/L0-constitution.md` — Pre-fill with user's answers from Step 2
2. `docs/L1/architecture.md` — Pre-fill tech stack, leave module details for user
3. `docs/L1/code-standards.md` — Read `references/code-standards-presets.md` and generate defaults matching the project's tech stack
4. `docs/L1/tech-decisions-log.md` — Empty template
5. `docs/L2/module-status.md` — Empty template
6. `docs/L2/tech-debt.md` — Empty template
7. `docs/L3/backlog.md` — Full template with status flow rules
8. `docs/L3/current-sprint.md` — Empty template
9. `docs/L3/session-handoff.md` — Empty template
10. `docs/templates/prd-template.md` — Full template
11. `docs/workplace/README.md` — Copy from `references/templates/workplace-readme.md`
12. `docs/workplace/archive/` — Create directory only

**Phase 2 — Role commands**

Copy all command files from `references/commands/` into `.claude/commands/`:

- `conductor.md`
- `pm.md`
- `architect.md`
- `dev.md`
- `qa.md`

**Phase 3 — CLAUDE.md integration**

Read `references/claude-md-section.md`. Append the team coordination section
to the project's existing CLAUDE.md (or create CLAUDE.md if it doesn't exist).

**Phase 4 — Version stamp**

Create `docs/.governance-version` with content:

```
version: 1.0.0
installed: {YYYY-MM-DD}
mode: fresh
```

This allows future Skill runs to detect the installed version and do incremental upgrades.

### Mode B: Migration

Read `references/migration-guide.md` for the full migration protocol.

Summary of the flow:

1. **Audit**: Scan all existing .md files, categorize their content by L0/L1/L2/L3/PRD/other
2. **Map**: Generate a migration plan showing: what moves where, what gets merged, what gets split, what stays as-is
3. **Present**: Show the migration plan to the user as a table. DO NOT execute until confirmed.
4. **Execute**: Move/merge/split files according to the confirmed plan
5. **Backfill**: Generate any missing framework files (commands, templates, etc.)
6. **Version stamp**: Create `docs/.governance-version` with `mode: migration`

## Step 4: Validate

After generation, run a self-check:

```bash
# Verify all expected files exist
for f in \
  "docs/L0-constitution.md" \
  "docs/L1/architecture.md" \
  "docs/L1/code-standards.md" \
  "docs/L1/tech-decisions-log.md" \
  "docs/L2/module-status.md" \
  "docs/L2/tech-debt.md" \
  "docs/L3/backlog.md" \
  "docs/L3/current-sprint.md" \
  "docs/L3/session-handoff.md" \
  "docs/templates/prd-template.md" \
  ".claude/commands/conductor.md" \
  ".claude/commands/pm.md" \
  ".claude/commands/architect.md" \
  ".claude/commands/dev.md" \
  ".claude/commands/qa.md"; do
  [ -f "$f" ] && echo "✅ $f" || echo "❌ MISSING: $f"
done
```

Report results to the user.

## Step 5: Onboarding guide

After setup, give the user a brief onboarding:

1. **First thing to do**: Fill in L0-constitution.md — this is YOUR product definition, the Skill can't write it for you
2. **Second thing**: Fill in L1/architecture.md module details and L1/code-standards.md rules
3. **Then try**: Run `/conductor init` to verify the system sees all files
4. **Daily workflow**: Start each session with `/conductor`, follow its action list
5. **For complex tasks**: Use Agent Teams with the templates in CLAUDE.md

## Upgrading an existing installation

If `docs/.governance-version` exists, this is an upgrade scenario:

1. Read the version file to determine what's installed
2. Compare with current Skill version
3. For commands (`.claude/commands/`): **overwrite** — these should always match the latest Skill version, user customizations belong in CLAUDE.md, not in commands
4. For templates (`docs/templates/`): **overwrite** — new PRDs should use the latest template
5. For doc content (`docs/L0-constitution.md`, `docs/L1/*`, etc.): **never overwrite** — these contain project-specific content
6. For structural additions (new directories, new framework files that didn't exist before): **add**
7. Update `docs/.governance-version` with new version number

## Reference files

All reference files are in the `references/` directory:

| File | When to read | Content |
|------|-------------|---------|
| `references/file-manifest.md` | Mode A Step 3 | Complete file list with directory structure |
| `references/migration-guide.md` | Mode B only | Full migration protocol for existing projects |
| `references/claude-md-section.md` | Step 3 Phase 3 | CLAUDE.md team coordination section to append |
| `references/code-standards-presets.md` | Step 3 Phase 1 | Tech-stack-specific code standards defaults |
| `references/commands/*.md` | Step 3 Phase 2 | All five role slash commands |
| `references/templates/*.md` | Step 3 Phase 1 | Document templates for L0-L3, PRD, workplace |
