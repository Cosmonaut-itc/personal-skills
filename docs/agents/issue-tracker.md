# Issue tracker: GitHub

Issues and PRDs for this repo live as GitHub issues. Use the `gh` CLI for all operations.

## Conventions

- **Create an issue**: `gh issue create --title "..." --body "..."`. Use a heredoc for multi-line bodies.
- **Read an issue**: `gh issue view <number> --comments`, filtering comments by `jq` and also fetching labels.
- **List issues**: `gh issue list --state open --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'` with appropriate `--label` and `--state` filters.
- **Comment on an issue**: `gh issue comment <number> --body "..."`
- **Apply / remove labels**: `gh issue edit <number> --add-label "..."` / `--remove-label "..."`
- **Close**: `gh issue close <number> --comment "..."`

Infer the repo from `git remote -v` - `gh` does this automatically when run inside a clone.

## When a skill says "publish to the issue tracker"

Create a GitHub issue.

## When a skill says "fetch the relevant ticket"

Run `gh issue view <number> --comments`.

## Implementation Workflow

This workflow applies to any issue after triage, regardless of whether it is
`ready-for-agent` or `ready-for-human`. See
[`implementation-workflow.md`](./implementation-workflow.md) for the full
workflow.

Issues that have `implemented: ready for review` should follow
[`review-cycle.md`](./review-cycle.md) before they are marked
`implemented: complete` and closed.

Issues with review feedback or requested changes should follow
[`review-feedback-workflow.md`](./review-feedback-workflow.md) before requesting
review again.

### Starting work

- Read the issue body, labels, comments, linked PRD/plan, `CONTEXT.md`, and relevant ADRs.
- Confirm the issue has exactly one category label and one state label.
- If labels conflict, stop and ask the maintainer.
- Create a dedicated branch for related implementation work unless the maintainer explicitly asks to work on the current branch.
- Preserve unrelated local changes.

### For `ready-for-agent`

- Use TDD for runtime behavior changes.
- Use subagent-driven development when the issue is large or has independent slices.
- For grouped ready-for-agent issues, respect dependency order.
- Do not dispatch multiple implementer subagents in the same worktree at the same time.
- Exploratory or review subagents may run in parallel when they do not write overlapping files.

### For `ready-for-human`

- Do not pretend the issue is AFK-ready.
- Implement only if the maintainer explicitly asks the current agent to execute it.
- Document human judgment, external access, design, or manual QA requirements in the PR/issue comment.

### Marking ready for review

Add `implemented: ready for review` only when:

- The requested behavior is implemented in the branch.
- Relevant tests/checks have been run or documented if blocked.
- Spec compliance review has passed.
- Code quality review has passed.
- The PR or branch notes explain what changed and how it was verified.

Do not close the issue at this point.

### Addressing review feedback

When issue or PR review feedback requests changes, follow
[`review-feedback-workflow.md`](./review-feedback-workflow.md). Fix the
feedback, run focused verification, and trigger re-review automatically before
asking the maintainer to accept the issue.

### Marking complete and closing

Add `implemented: complete` only when:

- The PR was merged, or
- The maintainer explicitly confirmed the implementation is complete.

After adding `implemented: complete`, close the issue with a short comment
linking the PR/commit and summarizing verification. See
[`review-cycle.md`](./review-cycle.md) for the required review loop.
