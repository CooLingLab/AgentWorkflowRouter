# Capability Catalog

**Starter, maintainer-curated capability catalog — not exhaustive.**
Unlike [methods.md](methods.md), this file is expected to grow via
routine pull requests as new supporting skills prove useful. It is not
auto-scraped or scored by any script; every entry here was added by a
human reviewing a PR.

Entries are generic and abstract on purpose — they describe a *kind* of
capability, not a specific third-party skill or repository, so this
catalog never implies that any particular unreviewed tool has been
vetted.

| Capability | One-line description | Phase(s) |
|---|---|---|
| research | Explore the problem space, prior art, and existing codebase before deciding an approach. | Frame |
| brainstorming | Structured divergent/convergent thinking to shape a vague idea into a concrete direction. | Frame, Decide |
| prototype / spike | Throwaway exploratory code to de-risk a technical unknown before committing to a design. | Decide |
| spec-writing | Produce a written spec or design doc as the implementation source of truth. | Specify |
| tdd | Red/green/refactor discipline for writing code against tests — usable inside another method's Build phase, not only as its own primary method. | Build |
| database-migration | Plan and execute schema or data migrations safely, with a rollback plan. | Plan, Build |
| code-review | Structured review of a diff for correctness and maintainability before merge. | Verify |
| security-review | Focused review for vulnerabilities, unsafe permissions, secrets handling, and injection risks. | Verify |
| browser-test / e2e-test | Automated or agent-driven browser testing of user-facing flows. | Verify |
| debug / root-cause-analysis | Systematic reproduction and root-causing of a bug, rather than guess-and-patch. | Build, Verify |
| documentation / knowledge-capture | Write up what was learned — patterns, gotchas, decisions — for future tasks. | Compound |
| dependency-upgrade | Safely bump or upgrade dependencies with compatibility checks. | Build, Verify |

## Adding a capability

Open a PR that adds one row: name, a one-line description, and the
phase(s) it supports. Keep the description generic unless you're prepared
to vouch for a specific tool by name — if you do name one, note briefly
why you trust it (actively maintained, known compatibility, used it
yourself), since a reader is deciding whether to trust this list, not just
whether the row is grammatically fine.
