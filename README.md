# Claude Commands

Prompt templates for Claude Code to handle common software development tasks.

## Overview

This repository contains prompts that help Claude Code work through software engineering tasks systematically. Each template provides a structured approach for specific development scenarios.

## Available Commands

### 1. Experiment-Driven Development (`01_experiment_driven_development.md`)

A learn-from-failure approach to implementing code changes. Tracks attempts and learnings through an iterative process.

**Use for:**
- Complex features requiring exploration
- Bug fixes with unclear root causes
- Performance optimizations
- Experimental refactoring

**Structure:**
- Goal setting with impact focus
- Code analysis and planning
- Implementation tracking with success/failure logs
- Iterative improvement cycles

### 2. Generate Codebase Context (`02_generate_codebase_context.md`)

Creates comprehensive documentation files (like `llms.txt`) that provide complete context about a codebase.

**Use for:**
- Onboarding documentation
- Codebase reference materials
- Pre-refactoring analysis
- AI assistant context

**Output includes:**
- File purposes and relationships
- Function signatures with parameters
- Architecture diagrams
- Code conventions
- Data formats

### 3. Analyze GitHub Issue (`03_analyze_github_issue.md`)

Analyzes GitHub issues and creates implementation plans before coding begins.

**Use for:**
- New issue assessment
- Implementation planning
- Stakeholder alignment
- Risk evaluation

**Process:**
1. Review issue details
2. Examine codebase
3. Create feature branch
4. Develop implementation plan
5. Request approval

### 4. Create GitHub Issue (`04_create_github_issue.md`)

Creates well-structured GitHub issues with varying detail levels based on complexity.

**Use for:**
- Feature requests
- Bug reports
- Architecture proposals
- Improvement suggestions

**Detail levels:**
- **MINIMAL**: Simple bugs or clear features
- **MORE**: Standard issues with technical details
- **A LOT**: Major features or architectural changes

**Features:**
- Repository convention research
- Best practices integration
- Stakeholder analysis
- Proper formatting and cross-references

### 5. Resolve PR Comments (`05_resolve_pr_comments.md`)

Systematically addresses all PR feedback, comments, and requested changes.

**Use for:**
- Code review feedback
- Multiple PR comments
- Pre-merge cleanup
- Review response management

**Workflow:**
1. Research - Find all comments and feedback
2. Planning - Prioritize and track with TodoWrite
3. Implementation - Address each item
4. Verification - Test and validate changes

**Features:**
- Parallel processing for independent changes
- Comment priority classification
- GitHub API command examples
- Resolution tracking

## Understanding Claude Integration Files

### Claude Configuration (`00_meet_claude.md`)

This comprehensive document serves as Claude Code's self-documentation and capability reference. While not a command template like the numbered files, it provides essential context for:

**For New Users:**
- Understanding Claude's full toolkit and capabilities
- Learning optimal prompting strategies for better results
- Discovering advanced features like parallel tool execution
- Getting insights into Claude's cognitive architecture

**For Development Teams:**
- Reference for Claude's available tools and integrations
- Best practices for AI-assisted development workflows
- Performance optimization techniques
- Understanding limitations and constraints

**Note:** This file represents Claude's internal knowledge about its own capabilities. While you don't need to read it to use the command templates, it's valuable for understanding how to work most effectively with Claude Code and unlock advanced features.

**Important:** This is different from the standard `CLAUDE.md` memory file that stores project-specific instructions. For information about project memory files, see the [Memory documentation](https://docs.anthropic.com/en/docs/claude-code/memory).

## Usage

Templates can be used as slash commands or copied directly:

```bash
# As slash command
/research Create OAuth authentication feature

# Or copy template content and replace placeholders
```

## Template Structure

Most templates include:
- Clear objectives
- Step-by-step processes
- Decision criteria
- Output formats
- Usage examples

## Best Practices

1. Select templates based on task type and complexity
2. Provide complete context when using templates
3. Follow all phases in multi-step workflows
4. Use TodoWrite for complex task tracking
5. Run tests and linting after changes

## Contributing

When adding templates:
- Focus on specific use cases
- Include clear documentation
- Provide examples
- Test thoroughly
- Keep formatting consistent

## Customization

- Adapt templates to match team conventions
- Save project-specific versions in `.claude/commands/`
- Combine templates for complex workflows
- Share modifications with your team