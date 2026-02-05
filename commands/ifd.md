---
description: Generate implementation workflow from any issue tracker URL
argument-hint: <issue-url-or-id>
allowed-tools:
  - AskUserQuestion
  - Skill
  - Read
  - Grep
  - Glob
---

# Issue First Development

Generate a structured implementation workflow from an issue tracker URL or ID.

## Input Handling

1. **If no argument provided**: You MUST use AskUserQuestion to get issue URL or ID:
   - Header: "Issue"
   - Question: "What issue would you like to work on?"
   - Options: Allow free text input

2. **If argument provided**: You MUST use the provided URL or ID directly

## Step 1: Fetch Issue Details

You MUST run `/issue-dev:issue-work <url-or-id>` to:
- Detect provider (Linear, JIRA, or ask if unclear)
- Fetch issue details (title, description, labels)
- Move issue to In Progress state

You MUST extract from the issue:
- **ISSUE_ID**: The issue identifier (e.g., PROJ-123)
- **ISSUE_URL**: Full URL to the issue
- **ISSUE_TITLE**: Issue title/summary
- **ISSUE_DESCRIPTION**: Full issue description

## Step 2: Infer Task Type

You MUST analyze the issue title and description for keywords:

**Frontend indicators**:
- UI, frontend, component, design, button, form, page, screen
- CSS, styling, layout, responsive, animation
- React, Vue, Svelte, Next.js, Nuxt

**Backend indicators**:
- API, endpoint, backend, database, service
- GraphQL, REST, integration, migration
- Server, authentication, authorization

**Design/Exploration indicators**:
- Brainstorm, explore, design, architecture, plan
- Research, investigate, prototype, RFC

**Classification**:
- If ONLY frontend keywords → **Frontend task**
- If ONLY backend keywords → **Backend task**
- If BOTH frontend AND backend keywords → **Full-stack task**
- If design/exploration keywords → **Design/Exploration task**
- If unclear → You MUST ask user

## Step 3: Determine Entry Point Skill

Based on task type:
- **Design/Exploration task** → Entry point MUST be `/superpowers:brainstorm`
- **Implementation task** (FE, BE, or Full-stack) → Entry point MUST be `/superpowers:write-plan`

## Step 4: Gather Additional Context

You MUST use AskUserQuestion with these questions:

**Question 1** - Confirm task type:
- Header: "Task type"
- Question: "This looks like a [detected type] task. Is this correct?"
- Options:
  - "[Detected type] (Recommended)" with description of what was detected
  - "Frontend" - UI, components, styling
  - "Backend" - API, database, services
  - "Full-stack" - Both frontend and backend
  - "Design/Exploration" - Research, planning, architecture

**Question 2** - Additional context:
- Header: "Context"
- Question: "Any additional criteria or context for this task?"
- Options:
  - "No additional context"
  - Other (free text)

**Question 3** - Reference links:
- Header: "References"
- Question: "Any reference links (PRs, docs, APIs, designs)?"
- Options:
  - "No references"
  - Other (free text)

## Step 5: Extract or Ask for Criteria

You SHOULD try to parse success criteria from the issue description (look for sections like "Acceptance Criteria", "Definition of Done", "Requirements", bullet points).

If criteria found in issue:
- You MUST present them to user for confirmation
- You SHOULD ask if any modifications needed

If criteria NOT found:
- You MUST ask: "What are the success criteria for this task?"

You MUST additionally ask:
- "Any negative criteria (what NOT to do)?"

## Step 6: Generate Workflow Prompt

You MUST generate the complete workflow using this template:

---

### Template Start

```
[ENTRY_SKILL] implement [[ISSUE_ID]](ISSUE_URL)

## Task: ISSUE_TITLE

ISSUE_DESCRIPTION

## Planning Protocol
> You MUST iterate through the plan at least 3 times
> Each iteration: explore codebase → refine approach → validate assumptions
> Final iteration: You MUST run /superpowers:code-review to validate the plan
> You MUST NOT proceed to implementation until plan is solid

## Criteria for Success
[SUCCESS_CRITERIA - from issue or user input]

## Negative Criteria
[NEGATIVE_CRITERIA - from user input, or "None specified" if not provided]

## Workflow
> You MUST pull latest main before starting
> You SHOULD clear the context before starting implementation
> You MUST create a separate worktree and branch to work in

[IF FRONTEND or FULL-STACK:]
> You MUST use frontend-design skill and playwright, and project skills for FE
> You MUST verify the UI and everything
> You SHOULD verify in storybook if available

[IF BACKEND or FULL-STACK:]
> You MUST verify the BE changes through testing the endpoint with sample data
> You SHOULD check existing tests for reference

> You SHOULD use ralph-loop (ralph-loop:help) if the task is complex so we can improve the code iteratively
> You SHOULD use find-skills to discover relevant skills
> You SHOULD use necessary skills and MCPs to achieve success
> You MUST check project CLAUDE.md and .claude/skills for project-specific patterns

[IF TASK INVOLVES DATABASE CHANGES:]
## Database Changes
> If this task involves schema changes, You MUST ask how migrations should be handled
> You MUST NOT assume migration strategy - confirm with user

## Verification & Quality
> You MUST do a /superpowers:code-review and fix potential issues and improvements
> You MUST typecheck, lint and build
> You MUST verify changes match acceptance criteria

## Completion
> You MUST commit, push as a PR remotely
> You MUST remove worktree
> You MUST run /issue-dev:issue-done when complete

[IF ADDITIONAL_CONTEXT provided:]
## Additional Context
ADDITIONAL_CONTEXT

[IF REFERENCES provided:]
## References
REFERENCES
```

### Template End

---

## Output

After generating the workflow:

1. You MUST output the complete workflow prompt in a code block
2. You MUST ask the user: "Ready to start? I can copy this workflow and begin, or you can modify it first."

If user confirms:
- You MUST copy the workflow prompt
- You MUST begin execution by invoking the entry skill with the workflow
