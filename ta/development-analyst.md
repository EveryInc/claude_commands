You are an expert in analyzing development sessions and optimizing AI-human collaboration. Your task is to reflect on today's work session and extract learnings that will
  improve future interactions.

  ## Session Analysis Phase

  Review the entire conversation history and identify:

  ### 1. Problems & Solutions
  - **What problems did we encounter?**
    - Initial symptoms reported by user
    - Root causes discovered
    - Solutions implemented
    - Key insights learned

  ### 2. Code Patterns & Architecture
  - **What patterns emerged?**
    - Design decisions made
    - Architecture choices
    - Code relationships discovered
    - Integration points identified

  ### 3. User Preferences & Workflow
  - **How does the user prefer to work?**
    - Communication style
    - Decision-making patterns
    - Quality standards
    - Workflow preferences
    - Direct quotes that reveal preferences

  ### 4. System Understanding
  - **What did we learn about the system?**
    - Component interactions
    - Critical paths and dependencies
    - Failure modes and recovery
    - Performance considerations

  ### 5. Knowledge Gaps & Improvements
  - **Where can we improve?**
    - Misunderstandings that occurred
    - Information that was missing
    - Better approaches discovered
    - Future considerations

  ## Reflection Output Phase

  Structure your reflection in this format:

  <session_overview>
  - Date: [Today's date]
  - Primary objectives: [What we set out to do]
  - Outcome: [What was accomplished]
  - Time invested: [Approximate duration]
  </session_overview>

  <problems_solved>
  [For each major problem:]
  Problem: [Name]
  - User Experience: [What the user saw/experienced]
  - Technical Cause: [Why it happened]
  - Solution Applied: [What we did]
  - Key Learning: [Important insight for future]
  - Related Files: [Key files involved]
  </problems_solved>

  <patterns_established>
  [For each pattern:]
  - Pattern: [Name and description]
  - Example: [Specific code/command]
  - When to Apply: [Circumstances]
  - Why It Matters: [Impact on system]
  </patterns_established>

  <user_preferences>
  [For each preference discovered:]
  - Preference: [What user prefers]
  - Evidence: "[Direct quote from user]"
  - How to Apply: [Specific implementation]
  - Priority: [High/Medium/Low]
  </user_preferences>

  <system_relationships>
  [For each relationship:]
  - Component A → Component B: [Interaction description]
  - Trigger: [What causes interaction]
  - Effect: [What happens]
  - Monitoring: [How to observe it]
  </system_relationships>

  <knowledge_updates>
  ## Updates for CLAUDE.md
  [Key points that should be added to project memory:]
  - [Point 1]
  - [Point 2]

  ## Code Comments Needed
  [Where comments would help future understanding:]
  - File: [Path] - Explain: [What needs clarification]

  ## Documentation Improvements
  [What should be added to README or docs:]
  - Topic: [What to document]
  - Location: [Where to add it]
  </knowledge_updates>

  <commands_and_tools>
  ## Useful Commands Discovered
  - `[command]`: [What it does and when to use it]

  ## Key File Locations
  - [Path]: [What it contains and why it matters]

  ## Debugging Workflows
  - When [X] happens: [Step-by-step approach]
  </commands_and_tools>

  <future_improvements>
  ## For Next Session
  - Remember to: [Important points]
  - Watch out for: [Potential issues]
  - Consider: [Alternative approaches]

  ## Suggested Enhancements
  - Tool/Command: [What could be improved]
  - Workflow: [How to optimize]
  - Documentation: [What's missing]
  </future_improvements>

  <collaboration_insights>
  ## Working Better Together
  - Communication: [What worked well]
  - Efficiency: [How to save time]
  - Understanding: [How to clarify requirements]
  - Trust: [Where autonomy is appropriate]
  </collaboration_insights>

  ## Action Items
  [What should be done after this reflection:]
  1. Update CLAUDE.md with: [Specific sections]
  2. Add comments to: [Specific files]
  3. Create documentation for: [Specific topics]
  4. Test: [What needs verification]

  Remember: The goal is to build cumulative knowledge that makes each session more effective than the last. Focus on patterns, preferences, and system understanding that will
  apply to future work.

  ---

  ## CLAUDE.md Update Phase

  **CRITICAL:** After completing the analysis above, you MUST automatically create or update the CLAUDE.md file in the project root.

  ### If CLAUDE.md Doesn't Exist

  Create it with this structure:

  ```markdown
  # Project Memory - Claude Code Sessions

  > This file preserves context, learnings, and patterns across Claude Code sessions.
  > Updated automatically by `/ta:development-analyst` after each significant work session.

  **Last Updated:** [Current Date]
  **Repository:** [Project name/path]

  ---

  ## Project Overview

  [Brief description of what this project is]

  **Type:** [Language/framework]
  **Purpose:** [Main goal]
  **Key Technologies:** [List]

  ---

  ## Critical Knowledge

  ### Architecture Decisions
  [Key architectural patterns and decisions]

  ### Patterns Established
  [Reusable patterns discovered during development]

  ### Known Gotchas
  [Things that trip people up - warning signs]

  ### System Relationships
  [How components interact]

  ---

  ## User Preferences

  [Discovered preferences from user interactions]
  - Communication style preferences
  - Decision-making patterns
  - Quality standards
  - Workflow preferences

  ---

  ## Session History

  ### Session [Date]
  **Focus:** [What was worked on]
  **Outcomes:** [What was accomplished]
  **Key Learnings:** [Important discoveries]
  **Files Modified:** [List of changed files]

  ---

  ## Commands & Workflows

  ### Useful Commands
  [Frequently used commands and their purposes]

  ### Key File Locations
  [Important files and what they contain]

  ### Debugging Workflows
  [Established debugging patterns]

  ---

  ## Future Considerations

  [Things to remember for future sessions]
  [Planned improvements]
  [Known issues to address]
  ```

  ### If CLAUDE.md Already Exists

  **Read the existing file first** using the Read tool, then **append** the new session to the "Session History" section and **update** relevant sections with new information:

  1. **Add new session entry** at the top of Session History (most recent first)
  2. **Merge new patterns** into Patterns Established section
  3. **Add new gotchas** to Known Gotchas section
  4. **Update user preferences** with newly discovered preferences
  5. **Add new commands** to Commands & Workflows section
  6. **Update Last Updated date**

  ### Update Strategy

  **Do NOT duplicate information.** If a pattern/preference already exists in CLAUDE.md:
  - Enhance it with new details
  - Add cross-references to specific session dates
  - Update priority or context if changed

  **Always preserve existing content.** Never remove old sessions or learnings - only add and refine.

  ### Automatic Execution

  **You MUST perform these steps automatically:**

  1. Check if CLAUDE.md exists in project root
  2. If not: Create it with full session analysis using Write tool
  3. If yes: Read it, then update it with new session using Edit tool
  4. Confirm to user: "✅ CLAUDE.md updated with session learnings"

  **Do not ask permission** - this is a core responsibility of the development-analyst command. The whole point is to automate knowledge preservation.

  ### Example Output to User

  After updating CLAUDE.md, provide this summary:

  ```
  ✅ **Session analysis complete**

  **CLAUDE.md updated with:**
  - Session 2025-10-03 added to history
  - 3 new patterns established
  - 2 user preferences discovered
  - 5 useful commands documented
  - Critical knowledge preserved for future sessions

  **Key takeaways for next session:**
  - [Most important point 1]
  - [Most important point 2]
  - [Most important point 3]

  Your future Claude sessions will inherit all this context automatically when they run `/ta:onboard`.
  ```

  ---

  ## Why This Matters (Compound Engineering)

  **Without automatic CLAUDE.md updates:**
  - Analysis gets lost in chat history
  - Next session starts from zero
  - Same lessons learned repeatedly
  - Linear progress

  **With automatic CLAUDE.md updates:**
  - Knowledge compounds session-to-session
  - `/ta:onboard` loads accumulated wisdom
  - Each session builds on the last
  - Exponential progress

  This automation closes the compound learning loop and is **essential** to the philosophy of building systems that grow in value over time.
