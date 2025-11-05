You are implementing a session-start workspace hygiene workflow. This command runs **at the beginning of a new session** to guard against context injection by removing untracked artifacts and ensuring only official git-tracked content is loaded.

## Workflow Overview

This workflow maintains workspace hygiene by:
- Removing untracked files and artifacts from previous sessions
- Reverting uncommitted changes
- Restoring the workspace to a clean git state
- Ensuring only **official, committed content** is available for `/ta:onboard`
- Optionally stashing work in progress for later retrieval

**Key principle:** Separation of concerns - this command is **defensive** (protect against stale context), not analytical. Run this **FIRST** at session start, then run `/ta:onboard` to load only git-tracked knowledge.

## When to Run This Command

**Primary use case:** Session start hygiene
```
New Claude session starts
    ↓
/ta:clean-session (FIRST - defensive hygiene)
    ↓
Clean slate - only git-tracked content
    ↓
/ta:onboard (SECOND - load official context)
    ↓
Begin work on clean foundation
```

**Also useful for:**
- Switching to a different task mid-session
- Clearing experimental changes
- Removing debugging artifacts
- Resetting after exploration

## Phase 1: Assessment

Before cleaning, understand what will be affected:

1. **Check Current Status**
   ```bash
   git status
   ```
   - See all untracked files
   - See all uncommitted changes
   - Identify what will be removed/reverted

2. **Ask User for Confirmation**
   Present the list of files that will be affected and ask:
   - "Found leftover artifacts from previous session. These are not in git and would pollute context if loaded by /ta:onboard."
   - Options:
     - **[s] Stash (DEFAULT)** - Save work in progress for later (SAFE, reversible)
     - **[c] Clean all** - Remove all untracked, revert all changes (DESTRUCTIVE)
     - **[k] Selective** - User chooses what to remove/keep
     - **[x] Cancel** - Keep everything as-is

   **Auto-execute after 10 seconds: Stash** (safest option)

## Phase 2: Cleanup Strategy

**IMPORTANT: Default to stash** - This is the safest, reversible option.

Based on user preference, execute the appropriate cleanup:

### Option A: Stash Work in Progress (DEFAULT - SAFEST)

**This should be the auto-execute option** after countdown, as it's non-destructive and fully reversible.

1. **Stash All Changes**
   ```bash
   git stash push -u -m "Session cleanup - WIP from $(date +%Y-%m-%d-%H:%M)"
   ```
   - `-u` = include untracked files
   - `-m` = add timestamped message
   - Preserves ALL work for later retrieval

2. **Verify Stash Created**
   ```bash
   git stash list
   ```
   - Show all stashes
   - Confirm latest stash exists

3. **Inform User**
   ```
   ✅ Work safely stashed:
   - Stash: stash@{0} - "Session cleanup - WIP from 2025-10-04-14:30"
   - Restore with: git stash pop
   - View contents: git stash show -p stash@{0}
   - Discard if not needed: git stash drop stash@{0}

   Your work is safe and can be recovered anytime!
   ```

### Option B: Clean All (DESTRUCTIVE - Requires explicit confirmation)

**Only use this if user explicitly chooses it.**

1. **Remove All Untracked Files**
   ```bash
   git clean -fd
   ```
   - `-f` = force removal
   - `-d` = remove untracked directories too
   - Removes ALL untracked files and directories
   - **PERMANENT - Cannot be undone!**

2. **Revert All Uncommitted Changes**
   ```bash
   git restore .
   ```
   - Reverts all modified tracked files
   - Returns workspace to last commit state
   - **PERMANENT - Cannot be undone!**

3. **Unstage Everything**
   ```bash
   git restore --staged .
   ```
   - Unstages any staged changes
   - Useful if files were added but not committed

### Option C: Selective Cleanup

1. **Show Interactive List**
   ```bash
   # List untracked files
   git ls-files --others --exclude-standard

   # List modified files
   git diff --name-only
   ```

2. **Remove Specific Untracked Files**
   ```bash
   rm file1.txt file2.txt
   rm -rf directory/
   ```
   - User specifies which files to remove
   - Keep important work in progress

3. **Revert Specific Files**
   ```bash
   git restore file1.js file2.js
   ```
   - User specifies which changes to revert
   - Keep other modifications

### Option D: Cancel (Keep Everything)

**Action:** Exit without making any changes. User keeps all uncommitted and untracked files.

## Phase 3: Verification

After cleanup, confirm the workspace is clean:

1. **Check Git Status**
   ```bash
   git status
   ```
   - Should show "working tree clean"
   - Or only intentionally kept files

2. **List Remaining Files**
   ```bash
   git ls-files --others --exclude-standard
   ```
   - Show any remaining untracked files
   - Verify expected state

3. **Confirm to User**
   ```
   ✅ Workspace cleaned:
   - X files removed
   - Y changes reverted
   - Working tree: clean

   Ready for next session!
   ```

## Phase 4: Optional - Clean Build Artifacts

If the project has build artifacts, offer to clean those too:

1. **Check for Build Directories**
   Common patterns:
   - `dist/`, `build/`, `.next/`, `.nuxt/`
   - `node_modules/` (if re-installing)
   - `.cache/`, `.turbo/`

2. **Clean Build Artifacts**
   ```bash
   npm run clean  # if script exists
   # OR
   rm -rf dist/ build/ .next/
   ```

3. **Optionally Re-install Dependencies**
   ```bash
   npm ci  # clean install from lockfile
   ```

## User Interaction Points

This workflow requires user input at these points:

1. **After assessment**: "Found X files. Clean all, selective, stash, or cancel?"
2. **If selective**: "Which files should I remove/revert? (provide list or patterns)"
3. **After cleanup**: "Workspace cleaned. Also clean build artifacts?"
4. **If stashing**: "Add a description for this stash? (optional)"

## Safety Checks

**NEVER:**
- ❌ Clean without showing user what will be affected
- ❌ Remove tracked files (only untracked and uncommitted)
- ❌ Delete .git directory or any git internals
- ❌ Remove .env files without explicit confirmation (may contain important config)
- ❌ Run `/ta:development-analyst` automatically (separation of concerns)

**ALWAYS:**
- ✅ Show full list of files to be affected
- ✅ Ask for confirmation before destructive actions
- ✅ Offer stash option for work in progress
- ✅ Verify git status after cleanup
- ✅ Warn if removing large files or many files
- ✅ Respect .gitignore patterns when cleaning

## Special Cases

### Case 1: Unpushed Commits
If user has local commits not pushed to remote:
```bash
git log origin/main..HEAD --oneline
```
**Action:** Warn user and ask if they want to push first or proceed anyway

### Case 2: Untracked .env Files
If `.env`, `.env.local`, etc. are untracked:
**Action:** "Found .env files. These may contain important configuration. Keep them?"

### Case 3: Large Untracked Files
If untracked files are very large (>10MB):
**Action:** "Found large files (X MB). List them before removing?"

### Case 4: Empty Workspace
If already clean:
```
✓ Workspace is already clean
- No untracked files
- No uncommitted changes
- Ready for work!
```

## Example Session Flow

```
[New Claude Code session starts]

User: "/ta:clean-session"
Assistant: Starting workspace hygiene check...

Phase 1: Assessment
Checking workspace status...

Found leftover artifacts from previous session:
- 3 untracked files:
  - temp-notes.txt (untracked)
  - experiments/test.html (untracked)
  - debug.log (untracked)

- 2 modified files:
  - src/index.js (modified - uncommitted changes)
  - README.md (modified - uncommitted changes)

⚠️  These artifacts are not in git and would pollute context if loaded by /ta:onboard.

How would you like to proceed?
[s] Stash (DEFAULT) - Save for later (SAFE, reversible)
[c] Clean all - Remove everything (DESTRUCTIVE)
[k] Selective - Choose what to remove/keep
[x] Cancel - Keep everything as-is

Auto-stashing in 10 seconds... (press [s] or Enter to stash now)

[User presses Enter or timeout expires]

Phase 2: Stashing
[Runs git stash push -u -m "Session cleanup - WIP from 2025-10-04-14:30"]
Saved working directory and index state...

[Runs git stash list]
✅ Work safely stashed:
- Stash: stash@{0} - "Session cleanup - WIP from 2025-10-04-14:30"
- 3 untracked files preserved
- 2 modified files preserved

Phase 3: Verification
[Runs git status]
✅ Workspace cleaned - hygiene check complete:
- Working tree: clean
- Your work is safe in stash@{0}
- Only git-tracked content remains

To recover your work later:
- Restore: git stash pop
- View contents: git stash show -p stash@{0}
- Discard: git stash drop stash@{0}

Ready for onboarding! Run /ta:onboard to load official project context.
```

## Related Commands

- `/ta:onboard` - **Run AFTER this command** to load official git-tracked context
- `/ta:development-analyst` - **Run separately** (manual step) when ready to commit learnings
- `/ta:commit-and-push` - Commit work to make it official for future sessions
- `git stash` - Built-in git command for temporary storage

## Session Start Workflow

**Recommended order for new sessions:**

```
1. /ta:clean-session
   ↓ (Guards against context injection)
   ↓ (Ensures only git-tracked content)
   ↓
2. /ta:onboard
   ↓ (Loads official project knowledge)
   ↓ (Reads CLAUDE.md, docs, git history)
   ↓
3. Begin work
   ↓ (Clean foundation, no stale artifacts)
```

**Separation of Concerns:**
- `/ta:clean-session` = **Defensive hygiene** (session start)
- `/ta:development-analyst` = **Knowledge capture** (manual, when ready)
- `/ta:onboard` = **Context loading** (official content only)

## Notes for Future Development

This workflow can be enhanced with:
- Automatic backup before destructive operations
- Smart detection of important untracked files (docs, configs)
- Cleanup profiles (aggressive, conservative, custom)
- Dry-run mode to preview what would be removed
- Timestamp tracking (detect very old untracked files vs recent)
- Integration with .gitignore to respect intentional untracked files
- Warning if there are unpushed commits (potential work loss)
