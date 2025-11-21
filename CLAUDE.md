# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a template repository for ClaudeCode custom slash commands. It provides pre-configured commands for GitHub Issue management and TDD-based development workflows. This repository is designed to be copied into other projects to accelerate development.

## Repository Architecture

### Command System

All custom commands are defined in `.claude/commands/` as Markdown files with frontmatter:

```markdown
---
description: Command description shown in /help
---

Command instructions here.
Use $ARGUMENTS to reference user input.
```

The file name determines the command name (e.g., `gh-issue.md` → `/gh-issue`).

### Command Categories

**GitHub Integration Commands** (`.claude/commands/gh-*.md`):
- Follow a pattern: analyze user input → interact for confirmation → use GitHub CLI → create artifacts → cleanup
- All require GitHub CLI authentication (`gh auth status`)
- All create TDD-friendly documentation in Issue templates

**Code Quality Commands** (`.claude/commands/{explain,improve,test}.md`):
- Stateless analysis commands
- No external dependencies required

### Key Design Patterns

1. **Pre-flight Checks**: GitHub commands check authentication and repository status before execution
2. **User Confirmation Loop**: Commands that create Issues/PRs confirm design with user first
3. **TDD Focus**: All generated templates and Issues are structured for test-driven development
4. **Cleanup Protocol**: Temporary files created during execution must be removed

## Command Customization

When modifying commands for different projects:

1. **Maintain Structure**: Keep the frontmatter + numbered steps format
2. **Error Handling**: Include error conditions and user guidance
3. **Atomic Operations**: Each step should be independently verifiable
4. **Template References**: Use `@.github/PULL_REQUEST_TEMPLATE.md` syntax for file references

## GitHub CLI Integration

Commands use `gh` CLI extensively:
- `gh issue view <number>` - Fetch issue details
- `gh issue create` - Create issues interactively
- `gh pr create` - Create pull requests

Authentication is handled via `gh auth login` and should be checked before operations.

## Permissions Configuration

`.claude/settings.local.json` uses prefix patterns for bash permissions:
- `Bash(gh:*)` - All GitHub CLI commands
- `Bash(git:*)` - All git operations
- Language-specific tools (pytest, npm, etc.)

When adding new tools, use the `:*` suffix for prefix matching.

## Documentation Structure

- `README.md` - User-facing quick start guide
- `how_to_vibe_coding/COMMANDS.md` - Detailed command reference manual
- `how_to_vibe_coding/` directory is intended to be deleted when using as template

## Template Usage Pattern

This repository is designed to be copied into existing projects:

```bash
cp -r .claude /path/to/project/
```

The `how_to_vibe_coding/` directory contains documentation for this template repository and should be removed when using in other projects.

## GitHub Templates

The `/gh-template` command generates:
- `.github/PULL_REQUEST_TEMPLATE.md` - PR description template
- `.github/ISSUE_TEMPLATE/bug_report.md` - Bug report template
- `.github/ISSUE_TEMPLATE/feature_request.md` - Feature request template

All templates are structured to support TDD workflows with clear acceptance criteria and test requirements.

## Workflow Integration

The typical development flow this template enables:

1. Create Issue via `/gh-feature-create` or `/gh-bug-create`
2. Implement via `/gh-issue <number>` which:
   - Fetches issue details
   - Creates feature branch
   - Implements with TDD
   - Runs tests/lints
   - Creates PR with proper references

This automates the GitHub Issue → Branch → Implementation → PR cycle.
