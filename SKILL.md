---
name: np-mode
description: >
  Follow the reduced principles when writing or changing code. Every change
  follows First Make it Work, then Then Make it Right, then Follow Practice.
  Use when the user runs /np-mode or asks for the reduced principles.
---

# np-mode

Every time you are asked to make a change follow:
* First Make it Work.
* Then Make it Right.
* Follow Practice

# First Make it Work

## Laziness Protocol

* One decision lives in one place.
* Ship the smallest edit that solves the problem.

## Foundational Thinking

* Get the data shape right before you write the logic.
* Do first whatever helps every later phase.
* If a concurrent writer would matter, do not share state.

## TDD

* Write the failing test before you change the code.
* See it fail for the reason you expect.
* Make the smallest change that makes it pass.
* If an honest test is impractical, do not write a bad one.

## Redesign From First Principles

* Do not bolt a new requirement onto the old design.
* Ask what you would have built if it had been there from the start, think that through, and deliver it in pieces.

## Attack the Premise

* When two fixes that share one premise fail the same gate, write that premise in one sentence and count which actors hold the imbalance.
* Remove the assignment.
* Do not start another fix until you have done that.

## Subtract Before You Add

* Take complexity out before you add anything.

## Outcome-Oriented Execution

* Aim at the end state you can check.
* A break in the middle is allowed when you planned it and can undo it.

## Build the Lever

* If the work is not obvious at a glance, build the tool that does it.
* Do one unit by hand, then make the tool match that unit.

## Architecture

### Model the Domain

* Put the domain in a structure that makes the illegal states impossible.
* Maintain a glossary of domain terms.
* If the code is already clear and local, leave it.

### Boundary Discipline

* Check the data where it enters.
* Inside, trust it.
* Keep the business logic in a pure function.

### Type System Discipline

* Make the illegal value impossible to write.
* Parse outside data once, where it comes in.
* Do not silence the compiler.

### Make Operations Idempotent

* Running it twice, or running it after a crash halfway through, ends in the same state.

### Separate Before Serializing Shared State

* If they do not need the same mutable thing, give each its own.
* If they do, allow one writer, and enforce that in the structure.

# Then Make it Right

## Fix Root Causes

* Reproduce it.
* Fix the cause.
* A guard that only hides the crash is not a fix.

## Sequence work into verifiable units

* Finish one small unit and check it.
* Then start the next.

## Test Behavior, Not Implementation

* The test calls the code the way its user does and checks the result that user would see.
* If the test would still pass when every function returned nothing, throw it out.

## Lint, Types and Audit

* A new Python project starts from `template/`: `pyproject.toml`, `.pre-commit-config.yaml` and `.gitignore`.
  Rename `pkg`, then run `uv sync` and `pre-commit install --hook-type pre-commit --hook-type pre-push`.
* `ruff check` and `ruff format`: the template's rule set enforces types on every signature (`ANN`), no print
  (`T20`), no commented-out code (`ERA`), security (`S`), complexity 10 (`C90`) and bug patterns (`B`, `PL`).
* `mypy --strict`. Fix the type, never add `# type: ignore` or `Any` to silence it.
* `pip-audit` on every push: a known vulnerability in a dependency is a failure. Upgrade the dependency.
* Hooks run tools through `uv run`, so the locked versions run, the same ones CI runs.
* Do not loosen a rule to pass. A per-line `# noqa: <code>` needs a comment saying why.

## Crapper

* Use the crapper tool
* Reduce each function under  CRAP 10 by splitting it, by testing it, or both.
* Consider whether case statements are worth reducing.

## Dryer

* Use the dryer tool to eliminate duplication that rank 0.8 or above.

## Import Linter

* Use import-linter (`lint-imports`) so dependencies point inward and features stay independent.
* Declare the contracts in `[tool.importlinter]` in `pyproject.toml`: `layers` from the edges in to the domain,
  `independence` for features, `forbidden` for anything else that must not be imported.
* A broken contract is a failure: it exits 1 and names the import and its line. Fix the import, never the contract.
* The root package must be importable (installed, or on `PYTHONPATH`).

## Refactoring

* Pin what the code does.
* Change the structure.
* Prove the behavior did not change.

# Practice

## Keep a Log

* Keep a log of every action you take and the reason you took it.
* Report that log upon completion.

## Guard the Context Window

* Keep the main thread small.
* Send the bulk elsewhere, and keep the summary.

## Worktree Cleanup

* Find what is safe to remove, confirm nothing still in use needs it, then reclaim the disk.
* Uncommitted work waits for a decision.

## Commit and Push

* Commit often, in small ordered commits.
* Each commit says why, what changed, and how you proved it.
* Push to the main line at the end.
