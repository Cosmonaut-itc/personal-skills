# Agent Docs Base

This directory is the reusable base for `docs/agents` in project repos.

## Purpose

Use this as the versioned source of truth for agent workflow docs that should
travel across projects:

- issue tracker conventions
- triage labels
- implementation workflow
- review cycle workflow
- review feedback workflow
- domain documentation expectations

Keep project-specific details outside this base. Project repos should place
their local details in `AGENTS.md`, `CONTEXT.md`, ADRs, or project-specific
docs that link to these base files.

## What Belongs Here

- Generic issue state and implementation flow rules.
- Generic review and feedback loops.
- Generic guidance for using project domain docs.
- References to skills by name, not by local absolute path.

## What Does Not Belong Here

- Project names, product-specific domain terms, or release-specific plans.
- `.env*`, secrets, credentials, URLs with tokens, or vendor account details.
- Local machine paths or other absolute workstation paths.
- Stack-specific commands unless they are clearly examples and marked as
  replaceable by project commands.

## Syncing Into A Project

Copy or sync this directory into the target repo:

```bash
rsync -av --delete docs/agents/ /path/to/project/docs/agents/
```

Then keep project-specific additions in the target repo's `AGENTS.md`,
`CONTEXT.md`, and ADRs.
