# ifd - Issue First Development

A Claude Code plugin that generates implementation workflows from issue tracker URLs.

## Installation

```bash
claude plugin install thomasindrias/ifd
```

## Usage

```bash
/ifd <issue-url-or-id>
```

Or just `/ifd` to be prompted for the issue.

### Examples

```bash
# With Linear URL
/ifd https://linear.app/my-team/issue/PROJ-123/implement-feature

# With JIRA URL
/ifd https://company.atlassian.net/browse/TEAM-456

# With just an issue ID (will ask which tracker)
/ifd PROJ-123
```

## What It Does

1. Fetches issue details via `issue-dev` plugin
2. Infers frontend vs backend vs full-stack task type from description
3. Asks for confirmation and additional context
4. Generates a structured implementation workflow with:
   - 3+ planning iterations
   - Code review validation
   - Worktree-based development
   - FE/BE-specific verification steps
   - Status tracking integration

## Generated Workflow

The generated workflow includes:

- **Planning Protocol**: Multiple iterations to refine approach before coding
- **Task-Specific Guidance**: Frontend tasks get Playwright/Storybook verification, backend tasks get endpoint testing
- **Quality Gates**: Mandatory code review, typecheck, lint, and build
- **Issue Lifecycle**: Automatic status updates via issue-dev

## Dependencies

This plugin requires:

- [superpowers](https://github.com/claude-plugins-official/superpowers) - For planning and code review
- [issue-dev](https://github.com/thomasindrias/issue-dev) - For issue tracking integration

## Supported Issue Trackers

Via the issue-dev plugin:
- Linear
- JIRA
- (More as issue-dev adds support)

## License

MIT
