You are implementing a comprehensive commit and push workflow. This command guides you through testing, committing, and optionally pushing/creating PRs with detailed context and documentation.

## Workflow Overview

This workflow standardizes the git commit process to ensure:
- Changes are tested before committing
- Commit messages follow repository conventions
- Full context is preserved for future sessions
- Optional push to remote and PR creation

## Phase 1: Pre-Commit Validation

Before committing anything, validate the changes are ready:

1. **Run All Tests**
   ```bash
   npm test -- --run
   ```
   - All tests MUST pass before proceeding
   - If tests fail, fix issues first
   - Document any new tests added

2. **Run Build (if applicable)**
   ```bash
   npm run build
   ```
   - Verify build succeeds
   - Check for warnings or deprecations
   - Ensure no breaking changes introduced

3. **Review Changes in Parallel**
   Run these commands together in a single message with multiple Bash tool calls:
   ```bash
   git status
   git diff
   git log --oneline -10
   ```
   - `git status`: See all untracked and modified files
   - `git diff`: Review all changes in detail
   - `git log`: Understand recent commit message style

## Phase 2: Commit Message Generation

Analyze all changes and create a comprehensive commit message:

1. **Commit Message Format**
   ```
   <type>: <brief summary (50 chars max)>

   <detailed description - what changed and why>

   - Key change 1 with file reference
   - Key change 2 with file reference
   - Key change 3 with file reference

   Technical details:
   - Implementation approach
   - Patterns established
   - Edge cases handled

   Testing:
   - Test results (X passing, Y failing)
   - Manual testing performed
   - Edge cases verified

   Files modified:
   - path/to/file1.ext - What changed and why
   - path/to/file2.ext - What changed and why

   Documentation:
   - docs/GUIDE.md - What it covers
   - experiments/test.html - How to use it

   🤖 Generated with [Claude Code](https://claude.com/claude-code)

   Co-Authored-By: Claude <noreply@anthropic.com>
   ```

2. **Commit Types**
   - `feat`: New feature
   - `fix`: Bug fix
   - `refactor`: Code restructuring without behavior change
   - `perf`: Performance improvement
   - `docs`: Documentation only
   - `test`: Test additions or fixes
   - `chore`: Build process, dependencies, tooling
   - `style`: Code formatting (not CSS)

3. **Message Guidelines**
   - Focus on **why** not **what** (git diff shows what)
   - Reference issue numbers if applicable (#123)
   - Include context for future sessions
   - Mention breaking changes prominently
   - List all files with brief explanations

## Phase 3: Stage and Commit

1. **Verify .gitignore is Properly Configured**
   Check that .gitignore exists and covers common patterns:
   ```bash
   cat .gitignore
   ```

   **Expected patterns (add if missing):**
   - `node_modules/` - Dependencies
   - `dist/` or `build/` - Build outputs
   - `.env*` - Environment variables
   - `*.log` - Log files
   - `.DS_Store` - OS files
   - Coverage/test output directories

   **Important:** If .gitignore is missing or incomplete, warn user and suggest adding patterns.

2. **Stage ALL Unignored Files**
   ```bash
   git add -A
   ```

   **Philosophy:** Trust .gitignore to define what should be committed.
   - Adds all modified tracked files
   - Adds all new untracked files (unless in .gitignore)
   - Removes deleted files from staging
   - Simple, predictable, no conditional logic

   **This ensures:**
   ✅ New files Claude creates are automatically included
   ✅ No files are forgotten or left behind
   ✅ .gitignore is the single source of truth
   ✅ User doesn't have to manually specify files

3. **Show What Was Staged**
   ```bash
   git status --short
   ```
   Confirm with user what will be committed:
   - Green files (staged)
   - Red files (not staged - should be in .gitignore)

4. **Create Commit with HEREDOC**
   Always use HEREDOC for proper formatting:
   ```bash
   git commit -m "$(cat <<'EOF'
   feat: Add comprehensive text overflow handling

   Implements multi-layered CSS approach to prevent layout breaking
   from long strings (hashes, URLs, corrupt metadata).

   - Main titles: Multi-line wrapping with word-break
   - List items: Single-line ellipsis with proper flex handling
   - Previews: Multi-line clamping with -webkit-line-clamp

   Technical details:
   - Uses word-wrap, overflow-wrap, word-break for compatibility
   - Critical min-width: 0 fix for flex items
   - Tested with 8 edge cases including hash strings

   Testing:
   - Build passes successfully
   - Created visual test suite (experiments/text-overflow-test.html)
   - All 8 test cases validated

   Files modified:
   - src/views/ItemDetail.vue - Added overflow handling to 5 sections
   - experiments/text-overflow-test.html - Comprehensive test suite
   - docs/TEXT_OVERFLOW_PATTERNS.md - Implementation guide

   🤖 Generated with [Claude Code](https://claude.com/claude-code)

   Co-Authored-By: Claude <noreply@anthropic.com>
   EOF
   )"
   ```

5. **Verify Commit**
   ```bash
   git status
   git log -1 --stat
   ```
   - Confirm commit succeeded
   - Verify all expected files included
   - Check commit message formatting
   - Working tree should be clean (no uncommitted changes)

## Phase 4: Optional Push & PR

Ask the user if they want to push and/or create a PR:

### Option A: Push Only
```bash
# First time pushing this branch
git push -u origin branch-name

# Subsequent pushes
git push
```

### Option B: Create Pull Request
1. **Push to remote first** (if needed)
2. **Generate PR description** based on commits:
   ```bash
   gh pr create --title "Title from commit" --body "$(cat <<'EOF'
   ## Summary
   - Brief overview of changes
   - Key improvements
   - Problems solved

   ## Changes
   - List of modifications with file references
   - Implementation details
   - Patterns established

   ## Testing
   - [ ] All automated tests pass
   - [ ] Manual testing completed
   - [ ] Edge cases verified
   - [ ] Build succeeds

   ## Documentation
   - Links to new docs
   - Usage examples
   - Related files

   🤖 Generated with [Claude Code](https://claude.com/claude-code)
   EOF
   )"
   ```

3. **Return PR URL** to user

## Phase 5: Post-Commit Actions

1. **Verify Nothing Left Behind**
   ```bash
   git status
   ```
   - Check for any uncommitted changes
   - Verify clean working directory

2. **Update Project Memory (if significant)**
   If this commit introduces new patterns or important knowledge:
   - Suggest updating CLAUDE.md
   - Reference the commit in session notes
   - Document new patterns for future sessions

## User Interaction Points

This workflow requires user input at these points:

1. **After test failures**: "Tests failed. Should I fix them or proceed anyway?"
2. **After generating commit message**: "Here's the commit message I drafted. Approve or modify?"
3. **After committing**: "Commit successful. Push to remote? Create PR?"
4. **If secrets detected**: "Warning: File X may contain secrets. Still commit?"

## Safety Checks

**NEVER:**
- ❌ Commit without running tests (unless user explicitly overrides)
- ❌ Force push to main/master
- ❌ Amend commits from other developers
- ❌ Skip pre-commit hooks (--no-verify)
- ❌ Use generic commit messages ("fix", "updates", "wip")

**ALWAYS:**
- ✅ Run tests before committing
- ✅ Review full diff before staging
- ✅ **Verify .gitignore exists and is configured properly**
- ✅ **Use `git add -A` to stage everything (trust .gitignore)**
- ✅ Show `git status` after staging (confirm what will be committed)
- ✅ Use HEREDOC for commit messages
- ✅ Include detailed context in commits
- ✅ Verify commit succeeded with git status
- ✅ Check authorship before amending: `git log -1 --format='%an %ae'`

**Philosophy Change (2025-10-04):**
- OLD: Selective staging (git add file1 file2)
- NEW: Trust .gitignore, stage everything (git add -A)
- Reasoning: Simpler, more reliable, ensures nothing is forgotten
- .gitignore is the single source of truth for what to exclude

## Example Session Flow

```
User: "Commit these text overflow changes"
Assistant: I will run /ta:commit-and-push

Phase 1: Pre-Commit Validation
[Runs npm test -- --run in parallel with npm run build]
Tests: 16 passed, 25 failed (video streaming tests - pre-existing)
Build: ✓ Success

[Runs git status, git diff, git log in parallel]

Phase 2: Commit Message
Based on changes, I propose:

```
feat: Add comprehensive text overflow handling

Implements multi-layered CSS approach...
```

Phase 3: Staging and Commit
[Checks .gitignore]
✓ .gitignore exists and covers: node_modules/, dist/, .env*, *.log

[Stages ALL files]
git add -A

[Shows what will be committed]
git status --short
M  src/views/ItemDetail.vue
A  experiments/text-overflow-test.html
A  docs/TEXT_OVERFLOW_PATTERNS.md

[Creates commit with HEREDOC]

✓ Commit created: abc1234

Phase 4: Push Decision
Commit successful! Would you like me to:
1. Push to remote?
2. Create a pull request?
3. Nothing more (local commit only)?
```

## Related Commands

- `/ta:development-analyst` - Run before committing to extract session learnings
- `/ta:test-driven-debug` - Use when debugging before commit
- Standard git workflow - This command enhances built-in capabilities

## Notes for Future Development

This workflow can be enhanced with:
- Automatic CLAUDE.md updates for significant changes
- Integration with /ta:development-analyst for automatic session documentation  
- Pre-commit hook verification
- Automated changelog generation
- Semantic version bumping based on commit type

