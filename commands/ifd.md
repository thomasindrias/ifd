---
name: ifd
description: Generate implementation workflow from any issue tracker URL
argument-hint: <issue-url-or-id>
---

# Issue First Development

Generate a structured implementation workflow from an issue tracker URL.

## RFC 2119 Keywords

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119.

## Process

### Step 1: Get Issue Details

If no argument provided:
1. You MUST use AskUserQuestion to ask: "What issue are you working on? (Paste URL or issue ID)"

Once you have the issue reference:
1. You MUST run `/issue-dev:issue-work <url-or-id>` to:
   - Detect provider (Linear, JIRA, or ask if unclear)
   - Fetch issue details (title, description, labels)
   - Move issue to In Progress

### Step 2: Infer Task Type

You MUST analyze the issue title and description for keywords:

**Frontend indicators**: UI, frontend, component, design, button, form, page, screen, modal, dialog, layout, style, CSS, animation, responsive
**Backend indicators**: API, endpoint, backend, database, service, GraphQL, REST, migration, schema, query, integration, server
**Design/Exploration indicators**: brainstorm, explore, design, architecture, plan, investigate, research, spike

You MUST determine task type:
- **Frontend**: Primarily FE keywords
- **Backend**: Primarily BE keywords
- **Full-stack**: Both FE and BE keywords present
- **Design/Exploration**: Design keywords or unclear implementation path

### Step 3: Determine Entry Point

Based on task type:
- If design/exploration task → you MUST use `/superpowers:brainstorm`
- If implementation task → you MUST use `/superpowers:write-plan`

### Step 4: Gather Additional Context

You MUST use AskUserQuestion to confirm and gather info:
1. "This looks like a [FE/BE/Full-stack] task. Correct?" (with options: Frontend, Backend, Full-stack, Design/Exploration)
2. "Any additional criteria or success conditions?" (open text)
3. "Any negative criteria (what NOT to do)?" (open text)
4. "Any reference links (PRs, docs, APIs)?" (open text)

### Step 5: Generate Workflow Prompt

You MUST output the complete workflow prompt using this template:

```
[ENTRY_POINT] implement [[ISSUE_ID]](ISSUE_URL)

## Task: ISSUE_TITLE

ISSUE_DESCRIPTION

## Planning Protocol
> You MUST iterate through the plan at least 3 times
> Each iteration: explore codebase → refine approach → validate assumptions
> Final iteration: you MUST run /superpowers:code-review to validate the plan
> You MUST NOT proceed to implementation until plan is solid

## Criteria for Success
[From issue description or user input]

## Negative Criteria
[From user input - what you MUST NOT do]

## Workflow
> You MUST pull latest main before starting
> You MUST clear the context before starting implementation
> You MUST create a separate worktree and branch to work in

[If frontend task:]
> You MUST use frontend-design skill and playwright for FE
> You MUST verify the UI thoroughly
> You SHOULD verify in storybook if available

[If backend task:]
> You MUST verify the BE changes through testing the endpoint with sample data
> You SHOULD check existing tests for reference

> You SHOULD use ralph-loop (ralph-loop:help) if the task is complex
> You SHOULD use find-skills to discover relevant skills
> You MUST check project CLAUDE.md and .claude/skills for project-specific patterns

## Database Changes (if applicable)
> If this task involves schema changes, you MUST ask how migrations should be handled
> You MUST NOT assume migration strategy - confirm with user

## Verification & Quality
> You MUST run /superpowers:code-review and fix all identified issues
> You MUST run typecheck, lint and build
> You MUST verify changes match acceptance criteria

## Completion
> You MUST commit and push as a PR remotely
> You MUST remove worktree after PR is created
> You MUST run /issue-dev:issue-done when complete

[If reference links provided:]
## References
[User-provided reference links]

[If additional context provided:]
## Additional Context
[User-provided context]
```

### Step 6: Execute or Present

After generating the prompt:
1. You MUST present the generated workflow to the user
2. You MUST ask: "Ready to execute this workflow?"
3. If yes, you MUST execute the entry point command with the generated prompt

## Variable Substitutions

- `[ENTRY_POINT]` → `/superpowers:write-plan` or `/superpowers:brainstorm`
- `[ISSUE_ID]` → Extracted from URL (e.g., CSX-1234, DJT-64)
- `[ISSUE_URL]` → Original issue URL
- `[ISSUE_TITLE]` → From issue API response
- `[ISSUE_DESCRIPTION]` → From issue API response

## Examples

**With Linear URL:**
```
/ifd https://linear.app/my-team/issue/PROJ-123/implement-feature
```

**With JIRA URL:**
```
/ifd https://company.atlassian.net/browse/TEAM-456
```

**With just issue ID:**
```
/ifd PROJ-123
```

**Interactive (no argument):**
```
/ifd
```
