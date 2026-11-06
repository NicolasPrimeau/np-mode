# np-mode

`np-mode` is a fork of [unclebob/ub-mode](https://github.com/unclebob/ub-mode). It keeps ub-mode's principles and changes the tools where a faster or better one exists. What differs from ub-mode:

- **Dependency direction is enforced with [import-linter](https://github.com/seddonym/import-linter)** instead of being viewed in uml-viewer: a broken layer or independence contract fails the build.
- **A project template** (`template/`) sets up the standard Python gates: [ruff](https://github.com/astral-sh/ruff) with a broad rule set, [mypy](https://github.com/python/mypy) strict, [pip-audit](https://github.com/pypa/pip-audit), [gitleaks](https://github.com/gitleaks/gitleaks), [deptry](https://github.com/fpgmaia/deptry) and pre-commit hooks that run them; on push, tests in random order with timeouts, [diff-cover](https://github.com/Bachmann1234/diff_cover) on the changed lines, with coverage from [slipcover](https://github.com/plasma-umass/slipcover), and the audit. [vulture](https://github.com/jendrikseipp/vulture) finds dead code and [Hypothesis](https://github.com/HypothesisWorks/hypothesis) writes property tests.

The rest of this README is ub-mode's.

## ub-mode

`ub-mode` is an agent skill that enforces disciplined software engineering principles when writing or changing code. It is loosely based on portions of [@poteto](https://github.com/poteto)'s rules (`poteto-mode` / `pstack`), reduced and paired with deterministic quality-gate tools:

1. **First Make it Work** — Laziness Protocol, Foundational Thinking, TDD, Redesign From First Principles, Attack the Premise, Subtract Before You Add, Outcome-Oriented Execution, Build the Lever, and Architecture (Model the Domain, Boundary Discipline, Type System Discipline, Make Operations Idempotent, Separate Before Serializing Shared State).
2. **Then Make it Right** — Fix Root Causes, Sequence Work into Verifiable Units, Test Behavior (Not Implementation), [Crapper](https://github.com/unclebob/crapper) (`CRAP < 10`), [Dryer](https://github.com/unclebob/dryer) (duplication `< 0.8`), and Refactoring.
3. **Practice** — Keep a Log, Guard the Context Window, Worktree Cleanup, and Commit and Push in small ordered commits.

## Companion Tool Repositories

`ub-mode` relies on deterministic quality-gate tools to enforce structure, complexity, duplication, and test thoroughness:

- [unclebob/crapper](https://github.com/unclebob/crapper) — Computes cyclomatic complexity, test coverage, and CRAP score per function so every function stays under the CRAP threshold (`< 10`).
- [unclebob/dryer](https://github.com/unclebob/dryer) — Detects structural code duplication so duplicate blocks (`>= 0.8` similarity) are eliminated.
- [seddonym/import-linter](https://github.com/seddonym/import-linter) — Fails the build when an import breaks a declared layer, independence or forbidden contract.

## Installation

Copy `SKILL.md` into your agent's skills directory as `np-mode/SKILL.md` (for example `~/.claude/skills/np-mode/SKILL.md`), then invoke `/np-mode`. Install the tools it runs into the project's environment with `pip install -r requirements.txt` (or `uv pip install -r requirements.txt`). For a new Python project, copy `template/` instead: its `pyproject.toml` lists the same tools as dev dependencies.
