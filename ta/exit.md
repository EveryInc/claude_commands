You are implementing a session-exit workflow. This command runs **at the end of a session** to save all work, preserve learnings, and leave the workspace in a clean state for the next session.

## Workflow Overview

This workflow ensures nothing is lost when ending a session by:
- Capturing session learnings via `/ta:development-analyst`
- Committing all work via `/ta:commit-and-push`
- Optionally cleaning workspace for next session
- Creating a clean handoff point for future sessions

**Key principle:** Mirror of `/ta:onboard` - if onboarding loads context, exit saves context. Complete the compound engineering loop.

## When to Run This Command

**Primary use case:** Session end
```
Finish work
    ↓
/ta:exit (save everything)
    ↓
Session analysis → CLAUDE.md
    ↓
Commit work → git
    ↓
Clean workspace (optional)
    ↓
Ready for next session
```

**Also useful for:**
- Switching to a different project
- Taking a break (save progress)
- End of day (safe checkpoint)
- Before major refactoring

## Session Exit Workflow

### Phase 1: Session Analysis (Automatic)

**Preserve all learnings before doing anything else.**

1. **Run Development Analyst**
   Automatically execute `/ta:development-analyst`:

   ```
   📊 Analyzing session and preserving learnings...
   ```

   This captures:
   - Problems solved and solutions
   - Patterns established
   - User preferences discovered
   - System relationships mapped
   - Updates CLAUDE.md for future sessions

2. **Confirm Analysis Complete**
   ```
   ✅ Session analysis complete
   - CLAUDE.md updated with session learnings
   - X new patterns established
   - Y user preferences discovered
   - Knowledge preserved for future sessions
   ```

**Why this runs first:**
- Captures context while still fresh in memory
- Ensures learnings aren't lost even if commit fails
- CLAUDE.md becomes part of the commit

### Phase 2: Commit Work (STRONGLY RECOMMENDED)

**Save all uncommitted work to git.**

1. **Check for Uncommitted Work**
   ```bash
   git status --short
   ```
   - Count uncommitted changes (tracked files only)
   - Count untracked files
   - Determine if commit is needed

2. **Offer Commit Options with Clear Warning**
   If uncommitted work exists:

   ```
   ⚠️  UNCOMMITTED WORK DETECTED - ACTION REQUIRED

   📦 Your session has uncommitted work:
   - X modified files
   - Y untracked files

   ⚠️  IMPORTANT: Uncommitted work will be stashed and hidden on next /ta:onboard.

   This means:
   ✓ If you commit: Work is saved permanently in git (SAFE)
   ⚠️  If you skip: Work is stashed with timestamp (RECOVERABLE but hidden)
   ❌ If you forget /ta:exit: Work may be lost on next session

   How would you like to proceed?
   [1] Commit all work (RECOMMENDED - work is permanently saved)
   [2] Commit only tracked changes (ignore untracked files)
   [3] Stash for later (work is recoverable via git stash pop)
   [4] Review changes first (show git diff)
   [5] Skip (NOT RECOMMENDED - work will be hidden)

   Choice:
   ```

3. **Execute Commit Workflow**
   Based on user choice:

   - **Option 1 (RECOMMENDED):** Run `/ta:commit-and-push` (stages ALL with `git add -A`)
   - **Option 2:** Run `/ta:commit-and-push` but only commit tracked changes (use `git add -u` instead of `git add -A`)
   - **Option 3:** Create timestamped stash, provide recovery instructions
   - **Option 4:** Show `git diff`, then ask again
   - **Option 5:** Warn user, create timestamped stash, proceed to Phase 3

   **Note:** As of 2025-10-04, `/ta:commit-and-push` uses `git add -A` to stage everything (trusts .gitignore).

4. **If User Chooses Stash (Option 3 or 5)**
   Create a well-documented stash:
   ```bash
   git stash push -u -m "Exit without commit - WIP from $(date +%Y-%m-%d-%H:%M) - [brief description]"
   ```

   Then inform user:
   ```
   ⚠️  Work stashed (not committed):
   - Stash: stash@{0} - "Exit without commit - WIP from 2025-10-04-15:30"
   - Contains: X modified files, Y untracked files

   To recover this work in your next session:
   1. Run: git stash list (to see all stashes)
   2. Run: git stash show -p stash@{0} (to view contents)
   3. Run: git stash pop (to restore work)

   ⚠️  OR: Next /ta:onboard will detect this stash and offer to restore it.

   BETTER APPROACH: Consider committing instead! Run /ta:commit-and-push manually.
   ```

5. **Confirm Commit**
   After commit (if chosen):
   ```
   ✅ Work committed successfully
   - Commit: abc1234
   - Message: "feat: Add feature X"
   - Files: X modified, Y added
   - Pushed to remote: yes/no

   Your work is safely preserved in git!
   ```

**Why strongly recommended but not mandatory:**
- User may be experimenting and want to iterate
- Some work may be intentionally temporary
- Stash provides safety net with clear recovery path
- Next /ta:onboard will offer stash recovery (defense in depth)

### Phase 3: Workspace Cleanup (Optional)

**Optionally clean workspace for next session.**

1. **Offer Cleanup**
   ```
   🧹 Clean workspace for next session?

   This will remove any remaining untracked files and ensure
   the next session starts with a clean slate.

   Options:
   1. Yes - Clean now (recommended)
   2. No - Leave workspace as-is
   3. Just show me what would be cleaned

   Choice:
   ```

2. **Execute Cleanup (if chosen)**
   Run `/ta:clean-session` with "clean all" option:
   ```bash
   git clean -fd
   git restore .
   git restore --staged .
   ```

3. **Confirm Clean State**
   ```
   ✅ Workspace cleaned
   - X untracked files removed
   - Working tree: clean
   - Ready for next /ta:onboard
   ```

**Why optional:**
- User may have work-in-progress to preserve
- May be switching projects, not ending session
- Flexibility for different workflows

**DRY consideration:** We're calling `/ta:clean-session` (reuse), not duplicating logic.

### Phase 4: Session Summary

**Provide final summary of what was saved.**

```
═══════════════════════════════════════════════════
SESSION EXIT COMPLETE
═══════════════════════════════════════════════════

📊 Session Analysis
   ✅ CLAUDE.md updated with learnings
   ✅ X patterns established
   ✅ Y preferences discovered

📦 Work Committed
   ✅ Commit: abc1234
   ✅ Message: "feat: Add feature X"
   ✅ Pushed to remote: yes

🧹 Workspace Status
   ✅ Clean (ready for next session)
   OR
   ⚠️  Untracked files remain (run /ta:clean-session manually)

═══════════════════════════════════════════════════
Next session: Run /ta:onboard to load this context
═══════════════════════════════════════════════════
```

## User Interaction Points

This workflow requires user input at these points:

1. **Phase 2:** "How would you like to commit? (all/tracked/skip/review)"
2. **Phase 3:** "Clean workspace for next session? (yes/no/show)"
3. **If reviewing:** "Show changes before commit?"
4. **If pushing:** "Push to remote now?"

## Safety Checks

**NEVER:**
- ❌ Skip session analysis (always run `/ta:development-analyst`)
- ❌ Commit without user confirmation
- ❌ Clean workspace without warning user
- ❌ Delete tracked files
- ❌ Force push to remote

**ALWAYS:**
- ✅ Run `/ta:development-analyst` first (preserve learnings)
- ✅ Show summary of what will be committed
- ✅ Ask before destructive operations
- ✅ Confirm all phases completed
- ✅ Provide clear next steps

## Special Cases

### Case 1: Nothing to Commit
If workspace is already clean:
```
ℹ️  No uncommitted work detected
- Skipping Phase 2 (commit)
- Proceeding to Phase 3 (cleanup)
```

### Case 2: CLAUDE.md Not Committed
If CLAUDE.md was updated but not in commit:
```
⚠️  CLAUDE.md was updated but not committed

CLAUDE.md contains session learnings and should be
committed so future sessions can access it.

Include CLAUDE.md in commit? (Y/n):
```

### Case 3: Unpushed Commits
If commits exist but not pushed:
```
⚠️  You have X unpushed commits

These commits are only on your local machine.

Push to remote now? (Y/n):
```

### Case 4: Stale Untracked Files
If untracked files are very old:
```
ℹ️  Found old untracked files (>7 days):
- experiments/old-test.html (30 days old)
- debug.log (15 days old)

These may be safe to remove. Include in cleanup? (y/N):
```

## Example Session Flow

```
User: "/ta:exit"
Assistant: Starting session exit workflow...

═══════════════════════════════════════════════════
PHASE 1: SESSION ANALYSIS
═══════════════════════════════════════════════════

📊 Analyzing session and preserving learnings...

[Executes /ta:development-analyst]

✅ Session analysis complete
- CLAUDE.md updated with session learnings
- 2 new patterns established (hybrid command organization, auto-clean)
- 1 user preference discovered (session-start hygiene)
- Knowledge preserved for future sessions

═══════════════════════════════════════════════════
PHASE 2: COMMIT WORK
═══════════════════════════════════════════════════

📦 Uncommitted work detected:
- 3 modified files (ta/exit.md, ta/README.md, CLAUDE.md)
- 0 untracked files

How would you like to proceed?
1. Commit all work (runs /ta:commit-and-push)
2. Commit only tracked changes (ignore untracked)
3. Skip commit (work stays uncommitted)
4. Review changes first

Choice: 1

Running /ta:commit-and-push...

[Executes commit workflow]

✅ Work committed successfully
- Commit: a1b2c3d
- Message: "feat: Add /ta:exit session-end workflow"
- Files: 3 modified
- Pushed to remote: yes

═══════════════════════════════════════════════════
PHASE 3: WORKSPACE CLEANUP
═══════════════════════════════════════════════════

🧹 Clean workspace for next session?

This will remove any remaining untracked files and ensure
the next session starts with a clean slate.

Options:
1. Yes - Clean now (recommended)
2. No - Leave workspace as-is
3. Just show me what would be cleaned

Choice: 1

Running /ta:clean-session...

✅ Workspace cleaned
- 0 untracked files removed
- Working tree: clean
- Ready for next /ta:onboard

═══════════════════════════════════════════════════
SESSION EXIT COMPLETE
═══════════════════════════════════════════════════

📊 Session Analysis
   ✅ CLAUDE.md updated with learnings
   ✅ 2 patterns established
   ✅ 1 preferences discovered

📦 Work Committed
   ✅ Commit: a1b2c3d
   ✅ Message: "feat: Add /ta:exit session-end workflow"
   ✅ Pushed to remote: yes

🧹 Workspace Status
   ✅ Clean (ready for next session)

═══════════════════════════════════════════════════
Next session: Run /ta:onboard to load this context
═══════════════════════════════════════════════════
```

## Related Commands

- `/ta:onboard` - **Mirror command** - Run at session start to load context
- `/ta:development-analyst` - **Automatically executed in Phase 1**
- `/ta:commit-and-push` - **Executed in Phase 2** (if user confirms)
- `/ta:clean-session` - **Optionally executed in Phase 3**

## Session Lifecycle

**Complete compound engineering loop:**

```
Session Start
    ↓
/ta:onboard
    ↓ Phase 0: Auto-clean workspace
    ↓ Phase 1-6: Load context
    ↓
Work on project
    ↓ (Build features, fix bugs, etc.)
    ↓
/ta:exit
    ↓ Phase 1: Capture learnings → CLAUDE.md
    ↓ Phase 2: Commit work → git
    ↓ Phase 3: Clean workspace
    ↓
Session End
    ↓
[Next session]
    ↓
/ta:onboard (loads saved context)
    ↓
Knowledge compounds!
```

**Result:** Each session builds on the last, nothing is lost, context grows exponentially.

## Defense in Depth: Hybrid Commit Workflow

**This command works with `/ta:onboard` to prevent data loss:**

### Layer 1: `/ta:exit` (Strong Encouragement)
- Phase 2 clearly warns about uncommitted work
- Shows consequences of skipping commit
- Provides easy commit path with `/ta:commit-and-push`
- If user skips: Creates timestamped stash with recovery instructions

### Layer 2: `/ta:onboard` (Safety Net)
- Phase 0.5 detects stashes from previous sessions
- Shows stash contents and age
- Offers easy recovery: Restore / View / Keep / Discard
- Reminds user to commit recovered work

**Result:** User can skip commit at exit, but work is never lost. Next session offers recovery.

**Philosophy:**
- Education > Enforcement (teach proper workflow)
- Flexibility > Rigidity (allow iteration and exploration)
- Safety > Convenience (multiple protection layers)
- Recovery > Restriction (make mistakes recoverable)

**Proper Workflow:**
```
Work on project
    ↓
/ta:exit
    ↓ Phase 1: Session analysis → CLAUDE.md
    ↓ Phase 2: Commit work → git (CHOOSE OPTION 1)
    ↓ Phase 3: Clean workspace (optional)
    ↓
Session ends cleanly
    ↓
Next /ta:onboard: No stashes to recover (clean start)
```

**Recovery Workflow (if user forgets):**
```
Work on project
    ↓
[User forgets /ta:exit, just closes window]
    ↓
Files left uncommitted
    ↓
Next session: /ta:onboard
    ↓ Phase 0: Auto-stash uncommitted files
    ↓ Phase 0.5: Detect stash, offer recovery
    ↓
User restores work → Commits properly this time
```

## Notes for Future Development

This workflow can be enhanced with:
- ✅ **Hybrid commit workflow with /ta:onboard** (IMPLEMENTED 2025-10-04)
- Automatic detection of forgotten TODOs in code
- Reminder to update documentation if code changed
- Check for uncommitted secrets before commit
- Automatic PR creation for feature branches
- Session duration tracking (time spent)
- Productivity metrics (commits, files changed, tests added)
- Integration with time tracking tools
- Automatic branch cleanup (merged branches)
- Reminder to run tests before exit
- Check for dependency vulnerabilities before commit
- Warning if user has run `/ta:exit` multiple times without /ta:onboard

---

**Credits:** Compound Engineering concept by Kieran Klaassen (Every)
**Philosophy:** Save everything, lose nothing, compound knowledge
