## Commands

Every routine task is one command; use these, not ad-hoc invocations. CI gates format, lint, types and tests.

setup `<…>` · test `<…>` · one test `<…>` · lint/format `<…>` · CI `<…>`

## Workflow

1. **Understand:** read the code and `docs/` first. Ask when unclear; never guess.
2. **Contract first:** a change at a boundary updates the contract (API spec, schema) before the code.
3. **Red:** write a failing test for the behaviour. A bug fix starts with a test that reproduces it.
4. **Green:** the least code that passes.
5. **Refactor** while green. Stay in scope: improve only what you touch.
6. **Verify:** run CI. Never bypass hooks or checks; never loosen a threshold to make it pass.
7. **Record:** decisions go in `docs/`, not summary docs. Conventional Commits.

## Design

- **Zen of Python:** simple > complex; explicit > implicit; flat > nested; readability counts; errors never pass
  silently; refuse to guess; one obvious way; hard to explain = bad idea.
- **CUPID:** composable, does one thing well, predictable, idiomatic, domain-based names.
- **Decoupled:** depend on abstractions; inject I/O, clock and randomness. No globals or singletons. One reason
  to change per module.
- **Dependencies point inward,** toward the domain. No cycles; features don't import each other.
- **Pure core, I/O at the edges.** Prefer immutable values.
- **Patterns earn their place:** wait for a second real use (rule of three). Composition over inheritance.
- Less code wins.

## Runtime

- Stateless processes: no shared state in memory or local files. Assume concurrent requests; no lost updates.
- Never block an async path or request thread.
- Structured logs; never payloads, secrets or personal data.
- Production data stays out of the repo; fixtures use placeholders. No credentials in code, config or docs.

## Code

- Standard formatter and linter; static types on every parameter and return. Lines ≤ 100, files ≤ 300,
  functions ≤ 50.
- Enums for codes; exact decimals for money; an explicit absent value, not `""`; a logger, not stdout; guard
  clauses; imports at the top.
- Validate untrusted input at the boundary; harden parsers (no XML entities or DTDs).
- Fail fast, naming the field. No silent defaults.
- Comments explain why, not what.

## Tests

- Unit tests: deterministic, no network. Integration tests: throwaway dependencies, never shared environments.
- Black-box tests drive the running service through its public interface only.

## Findability

- Source stays small enough to fit an agent's context.
- Large data (catalogs, fixtures) lives in data files, not inline in code.
- One concept per file, named for it; its header comment says what it is. Domain names; no `utils`.
- Error codes, log keys and wire fields appear verbatim where produced, so grep finds them.
- No reflection, built names, wildcard imports or re-exports. Each config key read once, by literal.
