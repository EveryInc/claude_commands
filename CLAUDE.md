# Project Memory - Claude Code Sessions

> This file preserves context, learnings, and patterns across Claude Code sessions.
> Updated automatically by `/ta:development-analyst` after each significant work session.

**Last Updated:** 2025-10-03
**Repository:** Claude Commands (`~/.claude/commands/`)

---

## Project Overview

A git-tracked repository of command prompt templates and workflow automation for Claude Code to handle common software development tasks systematically.

**Type:** Markdown-based command library
**Purpose:** Provide structured workflows and reusable prompts that compound in value over time
**Key Technologies:** Git, Markdown, YAML, Bash commands

---

## Critical Knowledge

### Architecture Decisions

**Two-tiered command structure:**
1. **Numbered templates (01-06):** Direct prompt templates for specific tasks
   - Used as-is or copied with placeholders replaced
   - Examples: `01_experiment_driven_development.md`, `04_create_github_issue.md`

2. **TA workflows (`/ta:*`):** Automated multi-phase workflows
   - All use `/ta:` prefix to distinguish from built-in commands
   - Filename `command-name.md` → command `/ta:command-name`
   - Examples: `/ta:onboard`, `/ta:exit`, `/ta:commit-and-push`

**Compound Engineering Philosophy (Kieran Klaassen, Every):**
- Build systems that compound in value over time
- Each session should improve the codebase AND the workflows
- Iterative refinement of shared systems, patterns, and documentation
- `~/.claude/commands/` is the interface for developing development contracts

### Patterns Established

**Pattern: Session Lifecycle with Auto-Hygiene**
- `/ta:onboard` at session start (Phase 0: auto-clean workspace with 10s countdown)
- Work on project
- `/ta:exit` at session end (saves learnings, commits work, optional cleanup)
- Creates complete knowledge preservation loop

**Pattern: Symmetric Onboard/Exit**
- If onboarding loads context, exit must save context
- Ensures nothing is lost between sessions
- Each session builds on accumulated knowledge
- Reference: ta/onboard.md, ta/exit.md

**Pattern: Automatic Knowledge Preservation**
- `/ta:development-analyst` automatically updates CLAUDE.md
- No permission needed - core responsibility of the command
- Prevents analysis from being lost in chat history
- Enables compound learning across sessions

### Known Gotchas

**Workspace Hygiene:**
- ⚠️ Untracked files from previous sessions can pollute context
- ✅ Solution: Phase 0 of `/ta:onboard` auto-cleans with 10-second countdown
- Advanced users can press 's' to stash instead of clean

**CLAUDE.md Location:**
- ⚠️ Must be in project root for `/ta:onboard` to find it
- ✅ Created automatically by `/ta:development-analyst` if missing

**Command Naming:**
- ⚠️ Custom commands MUST use `/ta:` prefix (not `/` alone)
- ⚠️ Filenames use kebab-case: `command-name.md`
- ✅ Example: `commit-and-push.md` → `/ta:commit-and-push`

### System Relationships

**Onboarding Flow:**
```
/ta:onboard
  ↓ Phase 0: git status → auto-clean if needed
  ↓ Phase 1: Read CLAUDE.md, README.md, TODO.md
  ↓ Phase 2: git log, git status, git branch
  ↓ Phase 3: Explore ~/.claude/commands/
  ↓ Phase 4: Check dependencies (package.json)
  ↓ Phase 5: Map structure (glob patterns)
  ↓ Phase 6: Present onboarding summary
```

**Exit Flow:**
```
/ta:exit
  ↓ Phase 1: Run /ta:development-analyst (automatic)
  ↓           → Updates CLAUDE.md with learnings
  ↓ Phase 2: Commit work (interactive - ask user)
  ↓           → Optional: Run /ta:commit-and-push
  ↓ Phase 3: Clean workspace (optional - ask user)
  ↓           → Optional: Run /ta:clean-session
  ↓ Phase 4: Session summary
```

**Knowledge Compound Loop:**
```
Session N-1: Work → /ta:exit → Save to CLAUDE.md
                                        ↓
Session N: /ta:onboard → Read CLAUDE.md → Load context
           Work with inherited knowledge
           /ta:exit → Save enhanced CLAUDE.md
                                        ↓
Session N+1: Inherits even more context...
```

---

## User Preferences

**Session Management:**
- Prefers automated workflows over manual steps
- Values comprehensive onboarding summaries
- Expects symmetric session start/end patterns

**Workflow Style:**
- Test-driven approach (evidenced by `/ta:test-driven-debug`)
- Documentation-first for compound knowledge
- Git-tracked everything for reproducibility

---

## Session History

### Session 2025-10-03
**Focus:** Testing `/ta:onboard` and `/ta:exit` workflow automation
**Outcomes:**
- Successfully executed `/ta:onboard` to load project context
- Workspace was clean (no Phase 0 cleanup needed)
- Learned about two-tiered command structure (numbered + TA)
- Understanding compound engineering philosophy
- Executing `/ta:exit` for proper session handoff

**Key Learnings:**
- Onboarding workflow effectively provides comprehensive project understanding
- Phase 0 auto-clean prevents context injection from previous sessions
- TA commands create symmetric session lifecycle (enter/exit)
- This repository IS the meta-project - commands for developing with commands

**Files Modified:**
- CLAUDE.md (created by this session analysis)

---

## Commands & Workflows

### Available TA Commands

**Session Management:**
- `/ta:onboard` - Project onboarding at session start
- `/ta:exit` - Session-end workflow (saves everything)
- `/ta:clean-session` - Workspace hygiene reset

**Development Workflows:**
- `/ta:commit-and-push` - Standardized git commit workflow
- `/ta:development-analyst` - Session analysis and CLAUDE.md updates
- `/ta:test-driven-debug` - Systematic debugging with tests
- `/ta:deployment-agent` - Deployment workflows

### Numbered Command Templates

1. `00_meet_claude.md` - Introduction
2. `01_experiment_driven_development.md` - Learn-from-failure approach
3. `02_generate_codebase_context.md` - Create comprehensive docs
4. `03_analyze_github_issue.md` - Issue analysis and planning
5. `04_create_github_issue.md` - Structured issue creation
6. `05_resolve_pr_comments.md` - Systematic PR feedback handling
7. `06_update_issue_summary.md` - Issue summary updates

### Specialized Agents

Located in `agents/` directory:
- `codebase-researcher` - Analyze architecture and extract patterns
- `design-implementation-reviewer` - Verify UI matches Figma
- `prompt-engineer` - Create/optimize AI prompts
- `react-figma-ui-engineer` - Implement Figma → React

### Useful Commands
- `git log --oneline -20` - Recent commit history with style
- `git status --short` - Compact status for artifact detection
- `ls -la ~/.claude/commands/ta/` - List available TA workflows

### Key File Locations
- `~/.claude/commands/README.md` - Main project overview
- `~/.claude/commands/ta/README.md` - Comprehensive TA documentation
- `~/.claude/commands/ta/*.md` - Individual workflow definitions
- `~/.claude/commands/CLAUDE.md` - This file (project memory)

### Debugging Workflows
- When starting a session: Run `/ta:onboard`
- When ending a session: Run `/ta:exit`
- When workspace is messy: Run `/ta:clean-session`
- When forgetting command details: Read `ta/README.md`

---

## Future Considerations

### For Next Session
- This was a minimal test session
- Real sessions will have actual code changes to commit
- Phase 2 of `/ta:exit` (commit work) will be more substantive
- Watch for compound knowledge growth in subsequent sessions

### Planned Improvements
Potential new `/ta:` commands mentioned in ta/README.md:
- `/ta:refactor-plan` - Plan and execute safe refactoring
- `/ta:performance-audit` - Analyze and optimize performance
- `/ta:security-scan` - Check for security vulnerabilities
- `/ta:dependency-update` - Update and test dependencies
- `/ta:api-design` - Design API endpoints systematically
- `/ta:database-migration` - Create and test DB migrations

### Known Issues to Address
- None identified in this test session
- Future sessions will populate this as real work is done

---

**Philosophy:** Build systems that compound in value over time. Each session improves both the project AND the workflows used to build it.
