# Claude Code Configuration

> Professional perspective profiles and workflow commands for Claude Code

**Installation Location:** `~/.claude/`
**Installed by:** `setup.sh` (auto-detected package)

---

## Overview

This package provides Claude Code configurations that enable multi-perspective task reviews through professional profile skills. It includes 12 distinct professional perspectives that can evaluate development tasks and provide domain-specific feedback.

## Contents

### Skills (`~/.claude/skills/`)

12 professional perspective profiles, each with a specific domain focus:

1. **Security Engineer** - Security vulnerabilities, authentication, data protection
2. **Performance Engineer** - Performance, scalability, optimization
3. **UX Designer** - User experience, usability, workflows
4. **DevOps Engineer** - Deployment, infrastructure, monitoring
5. **QA Engineer** - Testing, edge cases, quality assurance
6. **Accessibility Specialist** - WCAG compliance, inclusive design
7. **Technical Writer** - Documentation, clarity, developer experience
8. **Product Manager** - Business value, user needs, requirements
9. **Backend Engineer** - API design, business logic, data modeling
10. **Frontend Engineer** - Components, state management, UI code
11. **Database Architect** - Schema design, queries, data integrity
12. **Code Reviewer** - Code quality, patterns, best practices

Each profile:
- Provides feedback only when relevant to their domain
- Abstains when input would be noise
- Uses structured feedback with severity levels
- Maintains a distinct professional voice

See `skills/README.md` for complete documentation.

### Commands (`~/.claude/commands/ta/`)

**`/ta:task-review`** - Multi-perspective task review orchestrator

Invokes all 12 professional profiles to review a task, collecting feedback from each relevant perspective and presenting a consolidated review.

**Usage:**
```bash
# Review a specific task
/ta:task-review for: Add user authentication endpoint with OAuth

# Review from todo list
/ta:task-review
```

**Output:** Comprehensive review showing:
- Feedback from each relevant profile
- Profile abstentions (when not relevant)
- Consolidated summary of concerns
- Overall readiness assessment

## Installation

This package is automatically installed by the dotfiles `setup.sh` script:

```bash
cd ~/dotfiles
./setup.sh
```

The script will:
1. Detect the `claude/` package
2. Copy `claude/.claude/` contents to `~/.claude/`
3. Set proper permissions
4. Back up existing configurations if present

## Usage

### Individual Profile Review

Invoke a single profile for focused feedback:

```
Skill: security-engineer
[Describe your task]
```

The profile will either provide detailed feedback or abstain if not relevant.

### Comprehensive Multi-Perspective Review

Use the orchestrator command for full review:

```
/ta:task-review for: Implement user registration with email verification
```

All 12 profiles will be invoked, and you'll receive:
- Detailed feedback from relevant profiles
- Clear abstentions from non-relevant profiles
- Consolidated summary of all concerns
- Actionable recommendations

### Integration with TodoWrite

The task-review command integrates with Claude's TodoWrite tool:

```
# Create todos for your tasks
TodoWrite:
1. Add user authentication endpoint
2. Implement password reset flow
3. Add 2FA support

# Review specific todo item
/ta:task-review
[Select which task to review]

# System creates follow-up todos for concerns raised
```

## Philosophy

This system embodies **prevention over correction**:

- Catch issues before implementation (not after)
- Multiple perspectives reduce blind spots
- Smart abstention keeps signal-to-noise ratio high
- Structured feedback enables action
- Compound knowledge across sessions

## Customization

### Adding New Profiles

1. Create new skill file in `skills/`:
   ```bash
   touch ~/.claude/skills/new-profile.md
   ```

2. Follow the existing profile template:
   - Clear perspective definition
   - When to provide input (with examples)
   - When to abstain (with examples)
   - Communication style
   - Example feedback format

3. Update `skills/README.md` to document the new profile

4. Add to `/ta:task-review` invocation list

### Modifying Existing Profiles

Edit the profile file in `~/.claude/skills/` and adjust:
- Focus areas
- Abstention criteria
- Feedback structure
- Communication tone

Changes take effect immediately for new reviews.

## File Structure

```
claude/
└── .claude/
    ├── skills/
    │   ├── README.md                    # Skills documentation
    │   ├── accessibility-specialist.md
    │   ├── backend-engineer.md
    │   ├── code-reviewer.md
    │   ├── database-architect.md
    │   ├── devops-engineer.md
    │   ├── frontend-engineer.md
    │   ├── performance-engineer.md
    │   ├── product-manager.md
    │   ├── qa-engineer.md
    │   ├── security-engineer.md
    │   ├── tech-writer.md
    │   └── ux-designer.md
    └── commands/
        └── ta/
            └── task-review.md           # Orchestrator command
```

## Integration with Dotfiles

This package integrates seamlessly with the dotfiles ecosystem:

- **Automatic installation**: Detected and installed by `setup.sh`
- **Backup handling**: Existing configs backed up before replacement
- **Permission management**: Proper ownership set automatically
- **Cross-platform**: Works on macOS, Linux, and FreeBSD

## Related Documentation

- `skills/README.md` - Complete profile system documentation
- `/ta:task-review.md` - Orchestrator command specification
- Individual profile `.md` files - Each profile's focus and guidelines

## Examples

### Example 1: Security-Critical Task

```
Task: Add JWT authentication endpoint

Profiles providing feedback:
- Security Engineer: Critical concerns about secret storage, token rotation
- Performance Engineer: Recommend caching validation
- Backend Engineer: API design suggestions
- QA Engineer: Test scenarios for auth edge cases
- Tech Writer: API documentation needs
- Code Reviewer: Pattern recommendations

Result: 6/12 profiles engaged, caught security and design issues early
```

### Example 2: UI Component Task

```
Task: Create user profile edit form

Profiles providing feedback:
- UX Designer: Form validation UX, error messaging
- Frontend Engineer: Component structure, state management
- Accessibility Specialist: Keyboard navigation, screen reader support
- QA Engineer: Validation edge cases
- Tech Writer: Help text for fields

Result: 5/12 profiles engaged, ensured inclusive, well-tested UI
```

### Example 3: Documentation Task

```
Task: Update API documentation for new endpoints

Profiles providing feedback:
- Tech Writer: Documentation structure and clarity
- Backend Engineer: API contract accuracy
- Product Manager: User-facing value communication

Profiles abstaining:
- Security, Performance, DevOps, QA, Accessibility, Frontend, Database, Code Reviewer
  (No code changes, pure documentation)

Result: 3/12 profiles engaged, focused feedback without noise
```

---

## Troubleshooting

### Skills not found
**Problem:** Claude Code can't find skills
**Solution:** Verify files exist in `~/.claude/skills/` and run `ls ~/.claude/skills/`

### Command not found
**Problem:** `/ta:task-review` not recognized
**Solution:** Verify file exists at `~/.claude/commands/ta/task-review.md`

### All profiles abstaining
**Problem:** Every profile abstains from reviewing
**Solution:** Provide more context about the task or verify it has reviewable aspects

### Too much feedback
**Problem:** Overwhelming amount of concerns raised
**Solution:** This indicates the task may be too large - consider breaking it down

---

**Last Updated:** 2025-11-05
**Maintained By:** User + Claude Code collaboration
**License:** Same as parent dotfiles repository
