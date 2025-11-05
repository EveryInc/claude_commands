# Professional Perspective Profiles

> Claude skills representing distinct professional perspectives in software development

**Location:** `~/.claude/skills/`
**Purpose:** Provide multi-perspective reviews of development tasks
**Orchestrator:** `/ta:task-review` command

---

## Overview

This directory contains 12 professional perspective profiles that can review software development tasks from their specific domain expertise. Each profile:

- Has a clear area of focus
- Knows when their input adds value
- Abstains when their feedback would be noise
- Provides constructive, actionable feedback
- Maintains a distinct professional voice

## Available Profiles

### 1. Security Engineer (`security-engineer.md`)
**Focus:** Security vulnerabilities, authentication, data protection

**Provides input on:**
- Authentication/authorization
- Data storage and transmission
- API endpoints and integrations
- Cryptography and secrets
- Input validation

**Abstains on:**
- Pure documentation updates
- UI/styling without security impact
- Internal dev tooling

---

### 2. Performance Engineer (`performance-engineer.md`)
**Focus:** Performance, scalability, optimization

**Provides input on:**
- Database queries
- API endpoints under load
- Algorithmic complexity
- Caching strategies
- Resource-intensive operations

**Abstains on:**
- Simple UI text changes
- Documentation updates
- Minor styling tweaks

---

### 3. UX Designer (`ux-designer.md`)
**Focus:** User experience, usability, workflows

**Provides input on:**
- User-facing features
- UI component changes
- Navigation and routing
- Form interactions
- Error messages and feedback

**Abstains on:**
- Backend API implementation
- Database migrations
- Server infrastructure
- Internal tooling

---

### 4. DevOps Engineer (`devops-engineer.md`)
**Focus:** Infrastructure, deployment, observability

**Provides input on:**
- Deployment configuration
- Environment variables
- CI/CD pipelines
- Monitoring and logging
- Service dependencies

**Abstains on:**
- Pure frontend UI changes
- Documentation updates
- Test file additions
- Internal refactoring

---

### 5. QA Engineer (`qa-engineer.md`)
**Focus:** Testing, edge cases, quality assurance

**Provides input on:**
- New features
- Business logic changes
- API modifications
- Data validation
- User workflows

**Abstains on:**
- Pure documentation
- Build configuration
- Styling changes
- Comment additions

---

### 6. Accessibility Specialist (`accessibility-specialist.md`)
**Focus:** WCAG compliance, inclusive design

**Provides input on:**
- UI components and pages
- Form elements
- Navigation changes
- Images and visual content
- Keyboard interactions

**Abstains on:**
- Backend API implementation
- Database migrations
- Server configuration
- Build tooling

---

### 7. Technical Writer (`tech-writer.md`)
**Focus:** Documentation, clarity, developer experience

**Provides input on:**
- New features needing docs
- API changes
- Configuration changes
- User-facing error messages
- Complex logic

**Abstains on:**
- Internal refactoring
- Bug fixes with no behavior change
- Simple self-explanatory changes

---

### 8. Product Manager (`product-manager.md`)
**Focus:** Business value, user needs, requirements

**Provides input on:**
- New features
- User workflow changes
- Product requirements
- Analytics and tracking
- User-facing changes

**Abstains on:**
- Internal refactoring
- Build tool updates
- Code style changes
- Technical debt cleanup

---

### 9. Backend Engineer (`backend-engineer.md`)
**Focus:** API design, business logic, architecture

**Provides input on:**
- API endpoints
- Database schema/queries
- Business logic
- Authentication/authorization logic
- Service integrations

**Abstains on:**
- Pure frontend/UI changes
- CSS or styling
- Frontend state management
- Frontend build configuration

---

### 10. Frontend Engineer (`frontend-engineer.md`)
**Focus:** Components, state management, UI code

**Provides input on:**
- UI components
- Frontend state management
- Client-side routing
- Form handling
- CSS and styling

**Abstains on:**
- Backend API implementation
- Database changes
- Server configuration
- Backend business logic

---

### 11. Database Architect (`database-architect.md`)
**Focus:** Data modeling, queries, integrity

**Provides input on:**
- Database schema changes
- Data migrations
- Query implementation
- Indexing decisions
- Data relationships

**Abstains on:**
- Pure UI changes
- Frontend logic
- Documentation without DB changes
- Styling or UX

---

### 12. Code Reviewer (`code-reviewer.md`)
**Focus:** Code quality, patterns, best practices

**Provides input on:**
- Any code changes
- Refactoring
- New implementations
- Bug fixes
- Design pattern usage

**Abstains on:**
- Pure documentation updates
- Configuration files with standard patterns
- Simple one-line fixes
- Generated code

---

## Usage

### Individual Profile Invocation

You can invoke a single profile using the Skill tool:

```
Skill: security-engineer
[Task description and context]
```

### Orchestrated Multi-Profile Review

Use the `/ta:task-review` command for comprehensive review:

```
/ta:task-review

# Or with a specific task:
/ta:task-review for: Add user authentication endpoint with OAuth
```

The orchestrator will:
1. Invoke all 12 profiles
2. Collect their feedback (or abstentions)
3. Present a consolidated review
4. Summarize key concerns
5. Offer follow-up actions

---

## Profile Communication Standards

All profiles follow these principles:

### When to Provide Feedback
✅ **DO provide input when:**
- The task touches your domain of expertise
- You can identify specific risks or improvements
- Your perspective adds actionable value
- You see patterns or anti-patterns in your domain

### When to Abstain
❌ **DO abstain when:**
- The task has no relevance to your domain
- Your input would be speculative or generic
- The task is outside your area of focus
- You have nothing specific to add

### Feedback Format
Each profile provides structured feedback:
- Specific concerns with severity (Critical, High, Medium, Low)
- Concrete recommendations, not vague suggestions
- Examples or references when helpful
- Overall assessment of readiness

### Abstention Format
When abstaining, profiles clearly state:
```
**Abstaining** - This task has no [domain-specific] implications.
```

---

## Integration with Workflows

### With TodoWrite
```
1. Create todos for your work
2. Run /ta:task-review to review each task
3. Add follow-up todos for concerns raised
4. Complete tasks with confidence
```

### With Git Workflow
```
1. Review tasks before implementation
2. Address concerns during development
3. Review again before commit
4. Use /ta:commit-and-push with clean feedback
```

### With Development Cycle
```
Planning → Task Review → Implementation → Code Review → Deploy
           ↑______________|                |______________|
           (Prevention)                    (Verification)
```

---

## Best Practices

### For Task Reviews
1. **Review early** - Catch issues before implementation
2. **Review all tasks** - Even "simple" ones can have hidden concerns
3. **Don't skip profiles** - Comprehensive review catches more issues
4. **Act on feedback** - Reviews are valuable only if concerns are addressed

### For Profile Development
When enhancing or adding profiles:
1. **Clear focus** - Each profile has a distinct domain
2. **Smart abstention** - Knowing when NOT to comment is critical
3. **Actionable feedback** - Always provide specific, implementable suggestions
4. **Consistent voice** - Maintain the professional perspective

### For Orchestrator Usage
1. **Provide context** - More context = better feedback
2. **Review iteratively** - After addressing concerns, review again
3. **Track patterns** - Learn which profiles catch which issues
4. **Build muscle** - Regular reviews improve task quality over time

---

## Examples

### Example 1: Backend API Task
```
Task: Add /api/users endpoint to list all users

Profiles that provided feedback:
- Security Engineer: Warn about exposing all user data, suggest pagination + auth
- Performance Engineer: Recommend pagination for scalability
- Backend Engineer: Suggest filtering/sorting parameters
- QA Engineer: List test scenarios (auth, pagination, errors)
- Tech Writer: Note API documentation needs
- Code Reviewer: Suggest standard REST patterns

Profiles that abstained:
- UX Designer, Frontend Engineer, Accessibility Specialist (no UI)
- DevOps Engineer (standard endpoint, no special deployment needs)
- Product Manager (implementation detail, not product decision)
- Database Architect (uses existing schema)
```

### Example 2: Frontend Form Task
```
Task: Create user registration form

Profiles that provided feedback:
- UX Designer: Form validation feedback, error messaging
- Frontend Engineer: Component structure, state management
- Accessibility Specialist: Label associations, keyboard navigation
- QA Engineer: Validation edge cases, error states
- Tech Writer: Help text and field descriptions
- Code Reviewer: Form library best practices

Profiles that abstained:
- Security Engineer: "Frontend validation only, backend security is key"
- Performance Engineer: "Standard form, no performance concerns"
- Backend Engineer: "UI task, backend already done"
- DevOps Engineer: "No deployment implications"
- Product Manager: "Implementation matches requirements"
- Database Architect: "No schema changes"
```

---

## Extending the System

### Adding a New Profile

1. **Create the skill file**
   ```bash
   touch ~/.claude/skills/new-profile.md
   ```

2. **Follow the template**
   - Clear perspective section
   - When to provide input (with examples)
   - When to abstain (with examples)
   - Communication style
   - Example feedback format

3. **Update orchestrator**
   - Add to `/ta:task-review.md` profile list
   - Include in invocation sequence
   - Update this README

4. **Test thoroughly**
   - Verify it provides value when relevant
   - Confirm it abstains appropriately
   - Check feedback is actionable

### Potential Future Profiles
- **Data Scientist** - ML model implications, data pipeline concerns
- **Mobile Engineer** - Mobile-specific considerations (iOS/Android)
- **Compliance Officer** - GDPR, HIPAA, regulatory requirements
- **SRE** - Reliability, incident response, runbooks
- **Architect** - System design, long-term technical direction

---

## Troubleshooting

### Profile Not Providing Expected Feedback
- Check if task description has enough context
- Verify the profile's domain is actually relevant
- Read the profile's "when to provide input" section

### Too Many Abstentions
- Task might be too simple (documentation only, etc.)
- Or too well-scoped (no cross-cutting concerns)
- This is actually good - means clear scope!

### Overwhelming Feedback
- Prioritize Critical and High severity items first
- Create todos for Medium and Low items
- Consider if task scope is too large (break it down)

---

## Related Commands

- `/ta:task-review` - Orchestrate multi-profile review
- `/ta:onboard` - Load project context before reviewing
- `/ta:development-analyst` - Capture learnings from reviews
- `/ta:exit` - Save work after addressing feedback

---

**Last Updated:** 2025-11-05
**Repository:** `~/.claude/commands/` (git-tracked)
**Profile Count:** 12 perspectives
**Maintained By:** User + Claude Code collaboration
