# Update GitHub Issue with Work Summary

You are tasked with updating a GitHub issue with a comprehensive comment that summarizes completed work and provides context for the next developer or AI agent.

## Instructions

1. **Analyze the current state:**
   - Review the git log for recent commits related to the issue
   - Check the current branch and its relationship to the issue
   - Identify all files changed as part of this work
   - Review any relevant documentation updates

2. **Create a comprehensive GitHub comment** that includes:

   ### Summary Section
   - Clear, concise overview of what was accomplished
   - Reference the original issue requirements and confirm completion
   - Highlight any important architectural decisions made

   ### Implementation Details
   - List all files created/modified with brief explanations
   - Key technical changes organized by category (Backend, Frontend, Database, Tests, Documentation)
   - Any deviations from the original plan with justification

   ### Testing & Verification
   - Test coverage added (unit, integration, E2E)
   - Manual testing steps performed
   - Known issues or edge cases discovered

   ### Next Steps & Handoff Notes
   - **Current State:** Where exactly does this work leave the project?
   - **What's Ready:** What can be reviewed/tested immediately?
   - **What's Pending:** Any follow-up work required before merging?
   - **Blockers:** Any dependencies or issues blocking completion?
   - **Review Checklist:** Specific items for reviewers to verify
   - **Resume Point:** If incomplete, exact steps to continue work

   ### Documentation & References
   - Links to new documentation files
   - Migration guides or upgrade instructions
   - Related PRs, issues, or discussions
   - External resources referenced during implementation

   ### Environment & Setup
   - Any new environment variables required
   - New dependencies added
   - Breaking changes that need migration
   - Commands to test the changes locally

3. **Post the comment using the GitHub CLI:**
   ```bash
   gh issue comment <issue-number> --body "$(cat <<'EOF'
   [Your formatted comment here]
   EOF
   )"
   ```

4. **Update issue labels if appropriate:**
   - Add "ready-for-review" if complete
   - Add "needs-testing" if manual testing required
   - Add "in-progress" if work continues
   - Remove "needs-implementation" if done

5. **Format the comment with:**
   - Clear markdown headers (##, ###)
   - Code blocks with syntax highlighting
   - Checkboxes for actionable items
   - Links to commits, files, and documentation
   - Collapsible sections for lengthy details (use `<details>` tags)

## Best Practices

- **Be specific:** Don't just say "added tests" - specify how many, what they cover
- **Include commands:** Provide exact terminal commands for testing/verification
- **Link everything:** Commits, files, docs - make it easy to navigate
- **Think handoff:** Write as if you're never coming back and someone needs to pick this up
- **Highlight risks:** Call out anything that needs careful review or could break
- **Celebrate wins:** Acknowledge particularly elegant solutions or improvements

## Example Structure

```markdown
## ✅ Work Completed

[High-level summary with issue reference]

### Implementation Summary
- **Backend:** [Changes]
- **Frontend:** [Changes]
- **Database:** [Changes]
- **Tests:** [Changes]
- **Documentation:** [Changes]

### Files Changed
<details>
<summary>17 files changed (click to expand)</summary>

- `server/auth.py` - New authentication module with...
- `server/app.py` - Added 3 auth endpoints...
[etc.]

</details>

### Testing Performed
- ✅ [X] unit tests passing
- ✅ [Y] integration tests passing
- ✅ Manual testing: [specific scenarios]

---

## 🚀 Next Steps for Reviewer/Next Agent

### Current State
Branch `feature/X-description` is ready for review. All acceptance criteria met.

### What to Test
1. Generate JWT secret: `python3 -c "import secrets; print(secrets.token_urlsafe(32))"`
2. Add to .env: `JWT_SECRET=...`
3. Start services: `docker compose up -d --build`
4. Test registration: [specific steps]

### Review Checklist
- [ ] Security: Password hashing uses bcrypt
- [ ] Security: JWT tokens have expiry
- [ ] Tests: All existing tests updated for auth
- [ ] Tests: New auth tests cover edge cases
- [ ] Docs: Migration guide is clear
- [ ] Breaking: Old data migration strategy documented

### Known Issues / Future Work
- Password reset flow not implemented (out of scope for this issue)
- Rate limiting on login endpoint recommended but deferred
- Token refresh mechanism could be added in future

### Resume Point (if incomplete)
N/A - Work is complete

---

## 📚 Documentation

- [MIGRATION_AUTH.md](link) - Upgrade guide
- [Commit eb338be](link) - Full implementation
- Related: Issue #X, PR #Y

---

🤖 Summary generated with Claude Code
```

## Notes

- Use `gh issue view <number>` first to see current state
- Use `git log --oneline --graph` to see recent commits
- Check if there's already a PR linked to the issue
- If work is incomplete, be crystal clear about the exact resume point
