# TDD Codex Skill

This repository contains a Codex skill for test-driven development.

The skill lives in:

```text
skills/tdd
```

## What It Does

The `tdd` skill guides Codex through a red-green-refactor workflow for feature work and bug fixes. It emphasizes behavior-focused tests through public interfaces, incremental tracer-bullet development, and refactoring only after tests are green.

Use it when you want Codex to:

- Build a feature test-first.
- Fix a bug with a failing regression test before implementation.
- Use the red-green-refactor loop.
- Prefer integration-style tests over implementation-coupled tests.
- Avoid writing all tests upfront before learning from the first implementation slice.

## Core Principles

- Tests should verify observable behavior, not implementation details.
- Write one failing test, implement the minimum code to pass, then repeat.
- Avoid horizontal slicing: do not write all tests first and all code afterward.
- Use public interfaces so tests survive internal refactors.
- Refactor only after the current behavior is green.

## Included References

The skill includes supporting reference files:

- `tests.md`: examples of behavior-focused tests.
- `mocking.md`: guidance for when mocking is appropriate.
- `deep-modules.md`: guidance for small interfaces with deep implementations.
- `interface-design.md`: interface design for testability.
- `refactoring.md`: refactor candidates after green tests.

## Installation

Install by copying or symlinking the skill folder into your Codex skills directory:

```bash
ln -s /path/to/this/repo/skills/tdd ~/.codex/skills/tdd
```

Restart Codex after installing so the skill is picked up.

## Usage

Ask Codex to use TDD, test-first development, or red-green-refactor. For example:

```text
Use tdd to fix this bug with a regression test first.
```

```text
Build this feature with red-green-refactor.
```
