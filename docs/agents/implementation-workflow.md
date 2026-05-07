# Issue Implementation Workflow

This workflow applies after an issue has entered the tracker and has been triaged.

## Label Model

Issues use three label groups:

1. Category: `bug` or `enhancement`
2. State: `needs-triage`, `needs-info`, `ready-for-agent`, `ready-for-human`, or `wontfix`
3. Implementation progress: `implemented: ready for review` or `implemented: complete`

A triaged open issue must have exactly one category and exactly one state.
Implementation progress labels are additive.

## Ready For Agent

Use this path when the issue is fully specified and can be implemented AFK.

Required workflow:

1. Read the issue, linked PRD/plan, domain glossary, and ADRs.
2. Create or use the requested feature branch.
3. Use TDD for behavior changes.
4. Use subagent-driven development for non-trivial work.
5. Run spec compliance review.
6. Run code quality review.
7. Add `implemented: ready for review` when both reviews pass.

## Ready For Human

Use this path when the issue requires human judgment, credentials, product
approval, manual QA, legal/fiscal review, or external vendor decisions.

Rules:

- Keep `ready-for-human` unless the maintainer explicitly reclassifies it.
- If an agent implements it anyway by maintainer request, document the human
  decision points and verification constraints.
- Add `implemented: ready for review` only after the human-dependent checks are
  satisfied or explicitly waived by the maintainer.

## TDD Expectations

For runtime behavior changes:

1. Write one failing behavior test.
2. Implement the smallest change to pass.
3. Repeat vertically.
4. Refactor only while green.

Tests should verify public behavior, not private implementation details.

## Subagent Expectations

For larger issues:

- Fresh implementer subagent per task.
- Spec compliance review before code quality review.
- Fix and re-review until clean.
- Do not run multiple implementer subagents in one worktree at the same time.
- Parallel exploration/review is allowed when write scopes do not conflict.

### Reasoning Effort

Subagents should use `low`, `medium`, or `high` reasoning effort depending on
the assigned issue complexity.

- Use `low` for mechanical, well-scoped tasks with stable interfaces and low integration risk.
- Use `medium` for normal feature work that touches multiple modules, schemas, APIs, or UI behavior.
- Reserve `high` for issues that are extremely complex, high-risk, ambiguous, or require broad architectural judgment.

The coordinating agent decides the reasoning effort for each subagent based on
the issue's complexity, dependencies, blast radius, and uncertainty. If a
subagent reports `BLOCKED`, `NEEDS_CONTEXT`, or uncovers unexpected complexity,
the coordinating agent may re-dispatch or continue with a higher reasoning
effort.

## Completion

`implemented: ready for review` means implemented but not closed.

`implemented: complete` means accepted or merged.

Only close an issue after `implemented: complete`.

Use [`review-cycle.md`](./review-cycle.md) to review issues with
`implemented: ready for review` before marking them complete or closing them.
