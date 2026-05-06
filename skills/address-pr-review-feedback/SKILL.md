---
name: address-pr-review-feedback
description: Review the pull request for the current branch, gather actionable feedback from GitHub review threads, review bodies, issue comments, and the PR body itself (including bot summaries from Codex or Greptile), decide which comments reflect real issues, implement fixes with TDD and parallel subagents using GPT-5.5 with low reasoning, avoid redundant tests, resolve conversations with concrete replies, and request re-review only from bots that actually left actionable feedback. Use when asked to address PR comments without creating a new branch.
---

# Address PR Review Feedback

## Overview

Work the existing branch, pull latest changes first, inspect every source of PR feedback, and fix only the comments that point to real problems. Use `tdd` and `subagent-driven-development` when they are available in the current session; otherwise follow the same discipline manually.

Prefer GitHub tools when available and fall back to `gh` only when needed. Do not open a new branch.

## Workflow

### 1. Sync the branch

- Confirm the current branch and worktree state.
- Run `git pull` before reading or changing anything.
- Stay on the current branch. Do not create or switch to a new one.

### 2. Collect all feedback before coding

- Find the PR for the current branch.
- Read all of these sources:
- review threads and line comments
- top-level review bodies
- issue-style PR comments
- the PR body itself, because Greptile or similar bots may leave feedback there
- Verify that comments actually exist before starting fix work. Do not assume both Codex and Greptile commented.
- De-duplicate repeated feedback across humans, Codex, Greptile, and summary comments.

### 3. Classify every comment

- Mark each item as one of:
- `actionable`
- `already-fixed`
- `non-actionable preference`
- `false positive`
- `duplicate`
- Implement only comments that make sense and point to a real bug, regression, missing test, unsafe edge case, or maintainability issue worth changing.
- Keep a short technical explanation ready for every comment that does not require code changes.

### 4. Plan the work

- Group independent comments into separate implementation lanes.
- Use `tdd` before writing production code.
- Use `subagent-driven-development` when there are multiple independent comment groups or review tasks that can run in parallel.
- Keep write scopes disjoint when using subagents. If two comments touch the same module or behavior, keep them in the same lane.

## TDD Rules

- Search for existing tests before adding any new one.
- Reuse or extend nearby tests when they already cover the behavior.
- Do not create redundant tests for the same case.
- If a new test is required, check whether the same pattern should also cover adjacent code paths or edge cases elsewhere in the project.
- Write the failing test first, verify the failure, implement the minimal fix, then run the relevant test scope again.
- After the targeted test passes, run the broader relevant suite for the modified area.

## Subagent Rules

- Launch all implementation and review subagents for this workflow with model `gpt-5.5` and `reasoning_effort: low`.
- Use subagents to address independent comment clusters in parallel.
- Give each subagent a bounded scope:
- exact comment(s) it owns
- files or modules it may edit
- required tests
- Tell each subagent not to revert unrelated user changes and to adapt to concurrent edits.
- After implementation, run two independent review subagents over the final diff.
- If either reviewer finds a real issue, fix it and repeat review until both reviewers are clear.

## Resolution Standard

For each actionable comment:
- implement the fix
- add or update tests when justified
- verify locally
- reply in the conversation with the fix location
- resolve the conversation after the reply

For each non-actionable or invalid comment:
- reply with a concise technical explanation
- resolve the conversation if the explanation closes the point

Good resolution replies are concrete:
- `Fixed in src/cache.ts and src/cache.test.ts in commit abc1234.`
- `No code change. I verified the existing guard in src/auth.ts prevents this path, so I am not taking action here.`

## Dynamic Comment Handling

- Refresh the PR state before implementation and again after pushing.
- Build the list of bot sources from the latest actionable feedback, not from assumptions:
- `codex`
- `greptile`
- humans or other reviewers
- If a bot has no comments, no actionable feedback, or only duplicates and false positives, treat it as not requiring a follow-up ping.
- If Greptile put feedback only in the PR body, count that as Greptile feedback.
- If Codex has no remaining actionable feedback but Greptile does, ping only Greptile. Apply the inverse rule as well.

## Dynamic Review Routing

- Avoid unnecessary bot reviews.
- Use the de-duplicated actionable source list to decide who to ping at the end.
- If only Codex had actionable feedback, post only `@codex review`.
- If only Greptile had actionable feedback, post only `@greptile`.
- If both had actionable feedback, post two separate comments: `@codex review` and `@greptile`.
- If one bot had comments but all of them were duplicates, already fixed, or false positives, do not ping that bot again.
- If neither bot produced actionable feedback, skip both bot pings.

Do not spend review credits on bots that did not contribute relevant feedback in the current PR state.

## Suggested Execution Order

1. Sync the branch with `git pull`.
2. Inspect the PR body, review bodies, review threads, and regular PR comments.
3. Build a de-duplicated actionable comment list.
4. Search for existing tests that already cover each report.
5. Split independent work across subagents where safe.
6. Apply fixes with `tdd` and add only non-redundant tests.
7. Run targeted tests, then broader relevant suites.
8. Run two review subagents on the final diff and iterate until both are clean.
9. Commit with a clear message.
10. Push the current branch.
11. Reply to every resolved conversation with either the fix location or the reason no change was needed.
12. Resolve the conversations.
13. Post targeted re-review comments only for the bots that actually need another pass.

## Guardrails

- Do not create a new branch.
- Do not start fixing comments before collecting all feedback sources.
- Do not assume the PR body is informational only.
- Do not treat every bot suggestion as valid. Validate it against the code and tests.
- Do not add tests that merely restate existing coverage.
- Do not stop after code changes; finish the GitHub hygiene work too.
- Do not ask Codex or Greptile for another review unless the prior feedback justified it.
