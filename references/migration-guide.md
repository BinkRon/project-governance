# Migration Guide

Protocol for migrating an existing project's documentation system to the governance framework.

## Principles

1. **Never delete user content.** Move, merge, rename — but never discard.
2. **Propose, don't execute.** Always show the migration plan and get confirmation before moving files.
3. **Preserve git history.** Use `git mv` when possible so file history is retained.
4. **One step at a time.** Don't try to migrate everything in one pass.

## Phase 1: Audit

Scan the project to understand what exists.

### 1.1 Discover all documentation files

```bash
# Find all markdown files (excluding deps and git)
find . -name "*.md" \
  -not -path "*/node_modules/*" \
  -not -path "*/.git/*" \
  -not -path "*/vendor/*" \
  -not -path "*/dist/*" \
  | sort
```

### 1.2 Discover existing commands

```bash
ls -la .claude/commands/ 2>/dev/null
```

### 1.3 Check for existing CLAUDE.md

```bash
cat CLAUDE.md 2>/dev/null
```

### 1.4 Classify each file

Read the first 50 lines of each discovered .md file and classify it into one of these categories:

| Category | Criteria | Maps to |
|----------|----------|---------|
| **vision** | Product vision, mission, goals, principles | L0-constitution.md |
| **architecture** | System design, module breakdown, tech stack | L1/architecture.md |
| **standards** | Code style, conventions, linting rules | L1/code-standards.md |
| **decision** | ADRs, tech decision records, RFC docs | L1/tech-decisions-log.md |
| **status** | Module health, system status | L2/module-status.md |
| **debt** | Tech debt tracking, refactor plans | L2/tech-debt.md |
| **requirement** | Feature specs, user stories, PRDs | prd/PRD-*.md |
| **planning** | Sprint plans, roadmaps, backlogs | L3/ (backlog or current-sprint) |
| **discussion** | Open questions, debates, proposals | workplace/ |
| **changelog** | Release notes, version history | archive/ (or keep as-is) |
| **readme** | Project README, setup guides | Keep as-is (not part of governance) |
| **mixed** | Contains content from multiple categories | Needs splitting |
| **other** | Doesn't fit any category | Keep as-is |

### 1.5 Identify mixed files

Mixed files are the hardest to migrate. A file is "mixed" if it contains, for example:
- Architecture description AND code standards in the same file
- Requirements AND technical design in the same file
- Project vision AND sprint planning in the same file

For each mixed file, note which sections map to which categories.

## Phase 2: Map

Generate a migration plan table. This table must be shown to the user for confirmation.

### Migration plan format

```markdown
## Migration Plan

### Files to move (1:1 mapping)

| Current path | → New path | Category | Notes |
|-------------|-----------|----------|-------|
| docs/ARCHITECTURE.md | docs/L1/architecture.md | architecture | Direct move |
| docs/adr/001-db-choice.md | → merge into docs/L1/tech-decisions-log.md | decision | Append as TDR entry |

### Files to split (1:N mapping)

| Current path | → Splits into | What goes where |
|-------------|--------------|-----------------|
| docs/DESIGN.md | docs/L1/architecture.md + docs/prd/PRD-001-xxx.md | Sections 1-3 → architecture, Section 4 → PRD |

### Files to merge (N:1 mapping)

| Current paths | → Merges into | Strategy |
|--------------|--------------|----------|
| docs/adr/001.md, docs/adr/002.md, docs/adr/003.md | docs/L1/tech-decisions-log.md | Each ADR becomes a TDR entry |

### Files to keep as-is

| Path | Reason |
|------|--------|
| README.md | Not part of governance framework |
| CONTRIBUTING.md | Not part of governance framework |

### Files to create (missing from framework)

| Path | Why needed |
|------|-----------|
| docs/L0-constitution.md | No existing vision/principles doc found |
| docs/L2/tech-debt.md | No existing debt tracking found |
| ... | ... |
```

### Mapping rules

1. **If a file maps 1:1 to a framework file** → Move it, adapting the header format if needed
2. **If multiple files map to the same framework file** → Merge them into the framework file
3. **If one file maps to multiple framework files** → Split it, moving each section to the right place
4. **If a file has no mapping** → Keep it where it is
5. **If a framework file has no source** → Create it from template

### Special cases

**Existing PRDs / feature docs:**

If the project already has feature specification documents:
- Rename them to `PRD-{NNN}-{slug}.md` format
- Add the status header if missing (default to `done` if the feature is already shipped, `in-progress` if actively being worked on)
- Register them in backlog.md

**Existing ADRs (Architecture Decision Records):**

If the project has an `adr/` or `decisions/` directory:
- Consolidate into `docs/L1/tech-decisions-log.md`
- Each ADR becomes a TDR entry in the log
- Preserve the original numbering as TDR-{original-number}
- Original files can be moved to an `archive/adr/` directory

**Existing slash commands:**

If `.claude/commands/` already has custom commands:
- Do NOT overwrite commands that serve a different purpose than our five roles
- Only overwrite if a command has the same name as one of ours (conductor, pm, architect, dev, qa)
- If an existing command partially overlaps with one of our roles, show the user the overlap and let them decide

**CLAUDE.md:**

- NEVER overwrite existing CLAUDE.md content
- ONLY append the team coordination section
- If existing CLAUDE.md already has role definitions or team rules, show the user the conflict and propose how to reconcile

## Phase 3: Confirm

Present the migration plan to the user. Use a clear table format.

Explicitly call out:
- How many files will be moved
- How many files will be split (these need user attention)
- How many files will be merged
- How many new files will be created
- Which files will NOT be touched

Ask for confirmation. If the user wants to modify the plan, update and re-present.

## Phase 4: Execute

After confirmation, execute the plan step by step:

### 4.1 Create directory structure

```bash
mkdir -p docs/{L0,L1,L2,L3,prd,templates,workplace/archive,archive/prd}
mkdir -p .claude/commands
```

### 4.2 Move/merge/split files

Execute in this order:
1. Simple moves (1:1)
2. Merges (N:1) — read all sources, combine into target
3. Splits (1:N) — read source, extract sections into targets
4. Create new files from templates (missing framework files)

For each operation, report what was done:
```
✅ Moved docs/ARCHITECTURE.md → docs/L1/architecture.md
✅ Merged 3 ADR files → docs/L1/tech-decisions-log.md
✅ Split docs/DESIGN.md → docs/L1/architecture.md + docs/prd/PRD-001-core-api.md
✅ Created docs/L2/tech-debt.md (from template, no existing source)
```

### 4.3 Install commands

Copy role commands from skill references into `.claude/commands/`.

### 4.4 Update CLAUDE.md

Append team coordination section.

### 4.5 Backfill PRD registry

Scan `docs/prd/` for all PRD files and register them in `docs/L3/backlog.md` with appropriate statuses.

### 4.6 Version stamp

Create `docs/.governance-version`:
```
version: 1.0.0
installed: {YYYY-MM-DD}
mode: migration
migrated_files: {count}
```

## Phase 5: Post-migration checklist

After migration, walk the user through:

- [ ] Review L0-constitution.md — is the vision statement accurate?
- [ ] Review L1/architecture.md — does it reflect the current system?
- [ ] Review L1/code-standards.md — are the standards correct for this project?
- [ ] Review L3/backlog.md — are the PRD statuses correct?
- [ ] Run `/conductor init` to verify the system sees all files
- [ ] Try `/conductor` to see the first project status snapshot

## Common migration edge cases

**Project uses a wiki or external doc tool:**
Only migrate docs that live in the repo. Note in L0-constitution.md where external docs live.

**Project has hundreds of markdown files:**
Don't try to classify all of them. Focus on files in docs/ or similar doc directories.
Other .md files (like component README files) usually stay as-is.

**Project has a monorepo structure:**
The governance framework is per-project, not per-repo. If the monorepo has multiple
products, each product gets its own docs/ directory. Slash commands go in the repo root
`.claude/commands/` but can be scoped via CLAUDE.md instructions.
