You are implementing a test-driven debugging workflow. This command guides you through systematically testing, debugging, and documenting a feature implementation with comprehensive validation.

## Workflow Overview

This workflow is particularly useful when:
- A feature is broken and needs fixing with verification
- Implementing new functionality that requires reliability
- Debugging complex integration issues
- Need comprehensive documentation of the solution process

## Phase 1: Test Infrastructure Setup

First, ensure robust test infrastructure exists:

1. **Verify or Create Test Configuration**
   - Check for test framework (Vitest, Jest, etc.)
   - Ensure test environment matches runtime (browser simulation if needed)
   - Set up required mocks (localStorage, APIs, DOM, etc.)
   - Validate test can actually run in the environment

2. **Write Comprehensive Tests BEFORE Fixing**
   - Unit tests: Test individual functions/methods
   - Integration tests: Test component interactions
   - Edge cases: Error handling, empty states, invalid inputs
   - Real-world scenarios: User workflows

   Example structure:
   ```javascript
   describe('Feature Name', () => {
     it('should handle normal case', () => { ... });
     it('should handle error case', () => { ... });
     it('should validate inputs', () => { ... });
     it('should integrate with dependencies', () => { ... });
   });
   ```

3. **Run Tests to Establish Baseline**
   - Document which tests pass/fail initially
   - Understand failure modes
   - This validates tests work and shows real issues

## Phase 2: Iterative Debugging

Use tests to guide debugging:

1. **Enhanced Logging Strategy**
   - Add detailed console.error/log statements
   - Log all error states, network requests, responses
   - Include context: state, props, parameters
   - Log before/after critical operations

   Example pattern:
   ```javascript
   console.error('Operation failed:', error);
   console.error('Current state:', { state, params, context });
   console.error('Network details:', { status, headers, body });
   ```

2. **Create Diagnostic Tools**
   - Build isolated HTML test pages if needed
   - Create minimal reproductions
   - Add verification scripts
   - Tools should auto-load config from localStorage/env

   Save tools in `experiments/` directory for future debugging

3. **Iterate on Solutions**
   - Make targeted changes based on test failures
   - Run tests after each change
   - Check browser Network tab for API issues
   - Verify error messages match actual problems
   - Start with simplest solution, add complexity only if needed

4. **Common Debugging Patterns**
   - 400 errors → Check parameters and endpoint format
   - 401 errors → Check authentication/tokens
   - 404 errors → Check IDs and URL construction
   - Network errors → Check CORS, connectivity
   - Generic errors → Add more specific logging

## Phase 3: Solution Validation

Ensure the fix is complete:

1. **All Tests Must Pass**
   ```bash
   npm test -- --run
   ```
   - Every single test should be green
   - No skipped tests
   - No warnings or deprecations

2. **Manual Testing Checklist**
   - Test happy path in actual UI
   - Test error cases (network failure, bad data)
   - Test edge cases (empty, null, undefined)
   - Verify error messages are user-friendly
   - Check browser console for unexpected errors

3. **Performance Check**
   - Ensure no memory leaks
   - Check network payload sizes
   - Verify loading states work
   - Test on slower connections if applicable

## Phase 4: Comprehensive Documentation

Create artifacts for future sessions:

1. **Run Development Analyst**
   ```bash
   /ta:development-analyst
   ```
   - Let it analyze the entire session
   - Review its insights
   - Ensure it captures key decisions

2. **Create Session Notes** (`.sessions/YYYY-MM-DD-feature.md`)
   - What problem was encountered
   - Root cause analysis
   - Solution approach and why it worked
   - Failed approaches and lessons learned
   - Code patterns established
   - Test results
   - Files modified/created
   - Next steps or future improvements

3. **Update Project Memory** (`CLAUDE.md`)
   - Add critical API endpoints or patterns
   - Document "gotchas" for future sessions
   - Reference diagnostic tools created
   - Link to session notes

4. **Create Troubleshooting Guide** (if issue is likely to recur)
   - Step-by-step debugging workflow
   - Common error messages and fixes
   - Tools to use for diagnosis
   - Links to relevant code sections

## Phase 5: Commit Strategy

Commit with comprehensive context:

1. **Review All Changes**
   ```bash
   git status
   git diff
   git log --oneline -5
   ```

2. **Commit Message Format**
   ```
   feat/fix: Brief description of what was fixed

   - What the problem was (user-facing symptom)
   - Root cause discovered
   - Solution implemented
   - Tests added (X passing tests)

   Files modified:
   - file1.js - What changed and why
   - file2.test.js - Tests added

   Documentation created:
   - docs/guide.md - What it covers

   🤖 Generated with [Claude Code](https://claude.com/claude-code)

   Co-Authored-By: Claude <noreply@anthropic.com>
   ```

3. **Commit Scope**
   - Include all code changes
   - Include all tests
   - Include all documentation
   - Include development analyst output
   - One comprehensive commit for the feature fix

## Success Criteria

The workflow is complete when:

- ✅ All automated tests pass (100%)
- ✅ Manual testing confirms fix works in real usage
- ✅ Comprehensive documentation created
- ✅ Diagnostic tools available for future debugging
- ✅ Session notes archived
- ✅ Changes committed with detailed messages
- ✅ No new warnings or errors introduced

## Key Principles

1. **Tests First**: Write tests before fixing to validate the fix
2. **Iterate with Tests**: Use test failures to guide debugging
3. **Document Everything**: Future sessions need context
4. **Simple Solutions**: Start simple, add complexity only when needed
5. **Comprehensive Logging**: Make debugging visible
6. **Preserve Learnings**: Failed approaches are valuable knowledge

## Example Session Flow

```
User: "Feature X is broken, videos won't play"

You:
1. "Let me create tests to validate the streaming functionality"
   - Set up test infrastructure (Vitest + happy-dom)
   - Write 15 unit tests + 26 integration tests
   - Run tests → Many fail, confirming issue

2. "Tests are failing. Let me debug the stream URL generation"
   - Add detailed logging to video player
   - Create diagnostic HTML tool
   - User shares console logs → 400 Bad Request on API

3. "Found it - wrong API endpoint. Changing to /Items/{id}/Download"
   - Update stream URL generation
   - Run tests → All 41 tests pass ✅
   - Manual test → Videos play ✅

4. "Let me document this solution comprehensively"
   - Run /ta:development-analyst
   - Create session notes with root cause analysis
   - Create troubleshooting guide
   - Update CLAUDE.md with API endpoint knowledge

5. "Committing all changes with comprehensive context"
   - git commit with detailed message
   - Includes code, tests, docs, analyst output
```

## Anti-Patterns to Avoid

- ❌ Fixing code without tests (can't verify it works)
- ❌ Assuming error messages are accurate (check Network tab)
- ❌ Adding complexity too early (simple solutions often work)
- ❌ Skipping documentation (future sessions will struggle)
- ❌ Committing without running tests (might be broken)
- ❌ Generic commit messages (lose important context)

## Related Commands

- `/ta:development-analyst` - Analyze session and extract learnings
- Standard git workflow - For committing comprehensive changes

## Notes for Future Enhancement

This workflow could potentially be automated further by:
- Chaining commands (test → debug → document → commit)
- Creating templates for session notes
- Auto-running tests before allowing commits
- Generating troubleshooting guides from error logs

However, the iterative debugging phase requires human judgment and cannot be fully automated.