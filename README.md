> [!WARNING]
> **This repository has been deprecated.**
>
> IFD has been integrated into the [issue-dev](https://github.com/thomasindrias/issue-dev) plugin as `/issue-plan`.
>
> **To install:**
> ```bash
> claude plugin marketplace add thomasindrias/issue-dev
> claude plugin install issue-dev@issue-dev
> ```
>
> The `/issue-plan` command provides the same functionality as `/ifd`.

---

# ifd - Issue First Development (Deprecated)

A Claude Code plugin that generates structured implementation workflows from issue tracker URLs.

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

# Interactive mode
/ifd
```

## What It Does

1. **Fetches issue details** via `issue-dev` plugin (provider-agnostic)
2. **Infers task type** (frontend, backend, full-stack, or design) from description
3. **Asks for confirmation** and additional context
4. **Generates a structured workflow** with:
   - 3+ planning iterations before implementation
   - Code review validation of the plan
   - Worktree-based development isolation
   - FE/BE-specific verification steps
   - Status tracking integration
   - Positive and negative criteria sections

## Generated Workflow

The plugin generates a prompt that includes:

- **Planning Protocol**: Three-pass planning with code review validation
- **Criteria Sections**: Both success criteria and what NOT to do
- **Workflow Instructions**: Pre-implementation, implementation, and completion steps
- **Database Awareness**: Prompts for migration strategy if schema changes detected
- **Quality Gates**: Typecheck, lint, build, and code review before completion

## Commands

| Command | Description |
|---------|-------------|
| `/ifd [url]` | Generate implementation workflow from issue |

## Skills

| Skill | Description |
|-------|-------------|
| `ifd:workflow` | Explains the IFD philosophy and workflow standards |

## Prerequisites

**Install these plugins before using ifd:**

```bash
# Required: Planning and code review capabilities
claude plugin install superpowers@claude-plugins-official

# Required: Issue tracking integration (Linear, JIRA)
claude plugin marketplace add thomasindrias/issue-dev
claude plugin install issue-dev
```

| Plugin | Purpose | Required |
|--------|---------|----------|
| [superpowers](https://github.com/anthropics/claude-plugins-official) | `/superpowers:write-plan`, `/superpowers:brainstorm`, `/superpowers:code-review` | Yes |
| [issue-dev](https://github.com/thomasindrias/issue-dev) | Issue tracking integration (Linear, JIRA) | Yes |

## Philosophy

**Issue First Development** means:

1. Always start with a tracked issue
2. Understand requirements fully before coding
3. Plan thoroughly with multiple iterations
4. Validate the plan before implementation
5. Follow a consistent workflow for quality

## License

MIT
