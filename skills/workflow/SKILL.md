---
name: workflow
description: Internal skill documenting the Issue First Development workflow philosophy, edge case handling, and template customization. Used by the /ifd command to generate structured implementation workflows.
---

# Issue First Development Workflow

This skill documents the philosophy and patterns behind the ifd workflow generation.

## Core Philosophy

Issue First Development (ifd) is built on these principles:

1. **Issues drive work**: Every implementation MUST start from a tracked issue
2. **Planning before coding**: You MUST complete multiple planning iterations before coding
3. **Isolation**: Work MUST happen in separate worktrees to avoid conflicts
4. **Verification**: You MUST perform code review and quality checks before completion
5. **Closure**: Issues MUST be updated to reflect completion

## Workflow Stages

### 1. Issue Acquisition

The workflow MUST begin by fetching issue details via the `issue-dev` plugin. This:
- Normalizes provider differences (Linear, JIRA, etc.)
- Moves the issue to "In Progress"
- Extracts structured data (title, description, labels)

### 2. Task Classification

Tasks MUST be classified to determine the appropriate workflow:

| Type | Entry Skill | Key Indicators |
|------|------------|----------------|
| Frontend | write-plan | UI, component, styling, React/Vue |
| Backend | write-plan | API, database, service, GraphQL |
| Full-stack | write-plan | Both FE and BE indicators |
| Design/Exploration | brainstorm | Research, prototype, RFC |

### 3. Context Gathering

Before generating the workflow, you MUST gather:
- **Task type confirmation**: User validates the detected type
- **Additional context**: Requirements not in the issue
- **References**: Related PRs, docs, APIs, designs
- **Success criteria**: What "done" looks like
- **Negative criteria**: What to avoid

### 4. Workflow Generation

The generated workflow MUST include:
- Entry skill invocation with issue link
- Planning protocol (3+ iterations)
- Task-type-specific guidance
- Verification requirements
- Completion steps

## Edge Cases

### Unclear Task Type

When keywords do not clearly indicate task type:
1. You MUST present the ambiguity to the user
2. You MUST ask them to classify the task
3. You SHOULD offer "Full-stack" as a safe default

### Missing Criteria

When the issue lacks clear acceptance criteria:
1. You MUST check for sections like "Acceptance Criteria", "Requirements"
2. You SHOULD look for bullet points that might be criteria
3. If nothing found, you MUST explicitly ask the user
4. You MUST document that criteria were user-provided

### Database Changes

When the task involves schema changes:
1. You MUST add the "Database Changes" section to the workflow
2. You MUST NOT assume migration strategy
3. Workflow MUST prompt user about migration handling

### Complex Tasks

For tasks that seem complex:
1. You SHOULD include ralph-loop recommendation
2. You SHOULD suggest using find-skills for discovery
3. You SHOULD emphasize iterative improvement

## Template Customization

The workflow template MUST adapt based on:

### Task Type Sections

**Frontend tasks MUST include:**
- frontend-design skill usage
- Playwright verification
- Storybook verification if available

**Backend tasks MUST include:**
- Endpoint testing with sample data
- Existing test reference

**Full-stack tasks MUST include:**
- Both frontend and backend sections

### Optional Sections

These sections SHOULD only be included when relevant:
- **Database Changes**: Only if task mentions schema/migration
- **Additional Context**: Only if user provided context
- **References**: Only if user provided links

## Integration Points

### issue-dev Plugin

ifd MUST delegate all issue operations to issue-dev:
- `/issue-dev:issue-work` - Fetch and start work
- `/issue-dev:issue-done` - Complete and close

ifd MUST NOT interact with issue tracker MCPs directly.

### superpowers Plugin

ifd MUST use superpowers for workflow execution:
- `/superpowers:brainstorm` - Design/exploration entry
- `/superpowers:write-plan` - Implementation entry
- `/superpowers:code-review` - Verification step

### Project Skills

The generated workflow MUST reference:
- Project CLAUDE.md for conventions
- .claude/skills for project-specific patterns
- find-skills for capability discovery

## Best Practices

1. **You MUST NOT skip planning**: Even "simple" tasks benefit from iteration
2. **You MUST use worktrees**: Isolation prevents conflicts and enables parallel work
3. **You MUST verify before completion**: Code review catches issues early
4. **You MUST update issues**: Keep the tracker in sync with reality
5. **You MUST pull latest main**: Start from a clean, up-to-date base
