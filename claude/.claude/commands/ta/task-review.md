You are implementing a **Task Review Orchestrator**. This command solicits feedback from multiple professional perspective profiles on tasks in a task list (or on a specific task provided by the user).

## Workflow Overview

This workflow enables comprehensive multi-perspective reviews of software development tasks:
- Gathers input from 12 professional profiles (security, performance, UX, DevOps, QA, accessibility, documentation, product, backend, frontend, database, code review)
- Each profile provides feedback only when relevant to their domain
- Profiles abstain when their input would be random noise
- Produces a consolidated review showing all relevant perspectives

## Available Profiles

The following professional profiles are available in `~/.claude/skills/`:

1. **security-engineer** - Security vulnerabilities, auth, data protection
2. **performance-engineer** - Performance, scalability, optimization
3. **ux-designer** - User experience, usability, workflows
4. **devops-engineer** - Deployment, infrastructure, monitoring
5. **qa-engineer** - Testing, edge cases, quality assurance
6. **accessibility-specialist** - WCAG compliance, inclusive design
7. **tech-writer** - Documentation, clarity, developer experience
8. **product-manager** - Business value, user needs, requirements
9. **backend-engineer** - API design, business logic, data modeling
10. **frontend-engineer** - Components, state management, UI code
11. **database-architect** - Schema design, queries, data integrity
12. **code-reviewer** - Code quality, patterns, best practices

Each profile knows when to provide valuable input and when to abstain.

## Phase 1: Identify Tasks

**Determine what to review:**

1. **If user provided a specific task:**
   - Use the task description they provided
   - Example: User says "Review this: Add user authentication endpoint"

2. **If user mentioned a todo list:**
   - Ask which task(s) from the todo list to review
   - They can specify: "all", "pending", or specific task numbers

3. **If there's an active TodoWrite list:**
   - Show the current todo list
   - Ask which task(s) to review

4. **If no context provided:**
   - Ask the user: "What task would you like reviewed by the professional profiles?"

## Phase 2: Prepare Task Context

**For each task to be reviewed:**

1. **Format the task description clearly:**
   ```
   TASK: [Task title/description]

   CONTEXT:
   - Current status: [from todo or user]
   - Files involved: [if known]
   - Additional context: [any relevant details]
   ```

2. **Gather any additional context:**
   - If the task references specific files, note them
   - If there's a git branch or commit, mention it
   - Include any constraints or requirements

## Phase 3: Solicit Profile Feedback

**For each task, invoke all 12 profiles:**

1. **Invoke each skill in sequence:**
   - Use the Skill tool to invoke each profile
   - Pass the task description and context
   - Example: `Skill: security-engineer` with task context

2. **Collect responses:**
   - Some profiles will provide detailed feedback
   - Some profiles will abstain (not relevant to their domain)
   - Track which profiles provided input

3. **Profile invocation order:**
   - Security Engineer
   - Performance Engineer
   - UX Designer
   - DevOps Engineer
   - QA Engineer
   - Accessibility Specialist
   - Technical Writer
   - Product Manager
   - Backend Engineer
   - Frontend Engineer
   - Database Architect
   - Code Reviewer

## Phase 4: Consolidate and Present Results

**Create a comprehensive review report:**

```markdown
# Task Review: [Task Title]

## Task Description
[Full task description and context]

---

## Profile Feedback

### 🔒 Security Engineer
[Feedback or "Abstaining - no security implications"]

### ⚡ Performance Engineer
[Feedback or "Abstaining - no performance implications"]

### 🎨 UX Designer
[Feedback or "Abstaining - no user-facing impact"]

### ⚙️ DevOps Engineer
[Feedback or "Abstaining - no operational impact"]

### 🧪 QA Engineer
[Feedback or "Abstaining - no functional changes"]

### ♿ Accessibility Specialist
[Feedback or "Abstaining - no accessibility implications"]

### 📝 Technical Writer
[Feedback or "Abstaining - no documentation needs"]

### 🎯 Product Manager
[Feedback or "Abstaining - no product implications"]

### 🏗️ Backend Engineer
[Feedback or "Abstaining - no backend implications"]

### ⚛️ Frontend Engineer
[Feedback or "Abstaining - no frontend implications"]

### 🗄️ Database Architect
[Feedback or "Abstaining - no database implications"]

### 🔍 Code Reviewer
[Feedback or "Abstaining - no code to review"]

---

## Summary

**Profiles that provided feedback:** [List of profiles that didn't abstain]

**Key concerns raised:**
- [Critical/high priority items from all profiles]

**Recommendations:**
- [Consolidated action items]

**Overall readiness:** [Assessment based on feedback]
```

## Phase 5: Next Steps (Optional)

**Based on the review, offer to:**

1. **Create follow-up tasks** in TodoWrite for concerns raised
2. **Provide detailed guidance** on any specific concern
3. **Invoke a specific profile** again for deeper discussion
4. **Review the next task** if working through a list

## User Interaction Points

This workflow requires user input at these points:

1. **Task selection** (if not provided)
   - "What task would you like reviewed?"
   - "Which tasks from the todo list? (all/pending/specific numbers)"

2. **Follow-up actions** (after review)
   - "Would you like me to create follow-up tasks for the concerns raised?"
   - "Need more detail on any specific profile's feedback?"

## Safety Checks

**NEVER:**
- ❌ Skip profiles - all 12 must be invoked for every task
- ❌ Make up feedback - only report what profiles actually say
- ❌ Ignore abstentions - they're valid and important
- ❌ Bias toward certain profiles - all perspectives matter equally

**ALWAYS:**
- ✅ Invoke all 12 profiles for comprehensive review
- ✅ Show abstentions clearly (they show scope understanding)
- ✅ Consolidate feedback into actionable summary
- ✅ Maintain each profile's distinct voice and concerns
- ✅ Present feedback in a structured, readable format

## Example Session Flow

```
User: "/ta:task-review for the task: Add user login endpoint with JWT authentication"
Assistant: I'll review this task with all 12 professional profiles.

[Invokes all 12 profiles in sequence...]

# Task Review: Add user login endpoint with JWT authentication

## Profile Feedback

### 🔒 Security Engineer
**Security Review:**
🔴 **Critical:** JWT secret must be stored securely
- Use environment variables, never hardcode
🟡 **Medium:** Implement refresh token rotation
✅ **Good Practice:** Using standard JWT library

### ⚡ Performance Engineer
**Performance Review:**
💡 **Optimization Opportunity:** Cache JWT validation
**Overall Assessment:** GOOD

### 🎨 UX Designer
**Abstaining** - This is primarily a backend API task

[...other profiles...]

## Summary
**Profiles that provided feedback:** Security, Performance, DevOps, QA, Backend, Code Reviewer (6/12)
**Overall readiness:** NEEDS SECURITY IMPROVEMENTS before implementation
```

## Related Commands

- `/ta:onboard` - Load project context before reviewing
- `/ta:commit-and-push` - After addressing review feedback
- Individual profile skills - For deeper discussion on specific concerns

## Notes for Future Development

**Potential enhancements:**
- Support batch review of multiple tasks
- Generate GitHub issue comments from reviews
- Track which concerns were addressed
- Integration with `/ta:commit-and-push` to verify concerns resolved
- Save review history for learning patterns

---

**Last Updated:** 2025-11-05
**Location:** `~/.claude/commands/ta/task-review.md`
**Invocation:** `/ta:task-review [optional: task description]`
