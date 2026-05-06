# Personal Skills

This repository contains Codex skills.

## Included Skill

### `address-pr-review-feedback`

Location:

```text
skills/address-pr-review-feedback
```

This skill helps Codex address pull request feedback on the current branch. It collects comments from GitHub review threads, review bodies, issue-style PR comments, and the PR body itself, then fixes only comments that point to real problems.

It is designed for review-fix workflows where Codex should:

- Run `git pull` before making changes.
- Stay on the current branch.
- Inspect PR comments, review bodies, line comments, and PR body feedback.
- Classify comments as actionable, already fixed, non-actionable, false positive, or duplicate.
- Use TDD before production changes.
- Use parallel subagents for independent review comment groups.
- Launch implementation and review subagents with `gpt-5.5` and `reasoning_effort: low`.
- Avoid redundant tests.
- Reply to and resolve GitHub conversations with concrete fix locations or technical explanations.
- Request `@codex review` or `@greptile` only when that bot had actionable feedback.

## Required Companion Skills

For this skill to work correctly, the Codex environment or skills repository should also include these companion skills:

- [`tdd`](skills/tdd): required for test-first implementation and red-green-refactor discipline.
- [`subagent-driven-development`](skills/subagent-driven-development): required for parallel implementation and review workflows using subagents.

If those folders are not present in this repository, install them into your Codex skills directory before using `address-pr-review-feedback`.

Expected installed layout:

```text
~/.codex/skills/address-pr-review-feedback
~/.codex/skills/tdd
~/.codex/skills/subagent-driven-development
```

## Installation

Install the included skill by copying or symlinking it into your Codex skills directory:

```bash
ln -s /path/to/this/repo/skills/address-pr-review-feedback ~/.codex/skills/address-pr-review-feedback
```

Restart Codex after installing so the skill is picked up.

## Usage

Use this skill when asking Codex to address PR review feedback without creating a new branch:

```text
Use address-pr-review-feedback to review the PR comments on this branch and fix the valid ones.
```

The skill assumes the current branch is the branch being reviewed.
