## Commands

Every routine task is one command; use these, not ad-hoc invocations. CI gates format, lint, types and tests.

setup `<…>` · test `<…>` · one test `<…>` · lint/format `<…>` · CI `<…>`

## Workflow

1. **Understand:** read the code, `docs/` and recorded decisions first. Search for existing work before building.
2. **Ask** about intent, money, public text or anything irreversible. Otherwise act; don't guess.
3. **Contract first:** a change at a boundary updates the contract (API spec, schema) before the code.
4. **Red:** a failing test for the behaviour. A bug fix starts with a test that reproduces it.
5. **Green:** the least code that passes. **Refactor** while green. Fix bugs you find; add no unasked features.
6. **Verify:** run CI. Never bypass hooks or checks; never loosen a threshold to make it pass.
7. **Record:** decisions go in the project's decision record, not summary docs. Conventional Commits.

## Design

- **Zen of Python:** simple > complex; explicit > implicit; flat > nested; errors never pass silently; refuse
  to guess; hard to explain = bad idea. **CUPID:** composable, one thing well, predictable, idiomatic, domain names.
- **Decoupled:** depend on abstractions; inject I/O, clock and randomness. No globals or singletons.
- **Dependencies point inward,** toward the domain. No cycles. **Pure core, I/O at the edges;** immutable values.
- **Patterns earn their place** (rule of three). Composition over inheritance. Less code wins.
- When two writers race, remove one; don't coordinate them. A third patch to one bug means a wrong diagnosis.
- Read an authoritative limit from its source; never keep a copy that drifts. Measure before optimizing.
- **Runtime:** stateless processes; assume concurrent requests, no lost updates. Never block an async path.
- Fail fast, naming the field. A best-effort step may continue past a failure, but raises an alertable signal.
- Structured logs; never payloads, secrets or personal data. Deploy via the pipeline; schema via migrations.

## Code

- Standard formatter and linter; static types on every parameter and return. Lines ≤ 100, files ≤ 300,
  functions ≤ 50. Comments explain why, not what.
- Enums for codes; exact decimals for money; an explicit absent value, not `""`; a logger, not stdout; guard
  clauses; imports at the top. No silent defaults.
- Validate untrusted input at the boundary; harden parsers (no XML entities or DTDs). No regex mass edits.

## Tests and verification

- Unit tests: deterministic, no network. Integration tests may read shared services, never write or send.
- A check must be able to fail: put the bug back and watch it go red. A perfect score is a bug until explained.
- Drive the path a user walks; assert on the final output or the request on the wire, not internal calls.
- No real side effects: block outbound sends in every tier. Fakes terminate. Skipped tests count as failures.
- Run checks where the code runs (same environment, same directory). No dependence on locale or wording.
- Verify the claim, not the mechanism: what would the output be if the input were missing? Observe the result
  (render, query the store, read the wire). Repo code isn't necessarily deployed.
- Compare rates, not totals. Benchmark from clean, committed states. Check claims from memory at the source;
  never invent data, and say what you didn't look at.

## Git

- One change per branch and commit. Read the full diff first. Regenerated files ship with their cause.
- After each commit, check that HEAD moved and the tree is clean. Never retry a failed commit with `--amend`.
- Done means committed and pushed; never leave half a change in the tree.
- Check a result before cleaning up after it. Run destructive steps alone, never chained after a fallible one.

## Safety

- No sending, posting, publishing or spending without approval of that specific action. Draft in chat first.
- Never verify through a live write path (billing, sends, provisioning) or with a real user's account.
- Read a script before running it. Bound what you launch: timeouts, memory caps, few parallel agents.
- No secrets in code, config, docs or command-line arguments. Production data stays out; fixtures use placeholders.

## Communication

- Lead with the answer; keep it short. A question is not an instruction: answer it, then stop.
- Pick low-stakes options yourself. Mention a follow-up once. Don't reopen a decision the user has made.
- Report honestly: no overselling, no softening a real problem. No AI writing tells.

## Findability

- Source fits an agent's context. Large data (catalogs, fixtures) lives in data files, not inline in code.
- One concept per file, named for it; its header comment says what it is. Domain names; no `utils`.
- Say a thing once, where it belongs; link to it elsewhere. No reflection, built names, wildcard imports or
  re-exports. Error codes, log keys and wire fields appear verbatim; each config key read once, by literal.
