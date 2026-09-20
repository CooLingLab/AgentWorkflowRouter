# Method Registry

A small, stable list of engineering methodologies. This list changes
rarely, and only via a deliberate maintainer PR — contrast with
[capability-catalog.md](capability-catalog.md), which is broad and
expected to grow more often.

Every method is judged against the same six signals used in
[decision-guide.md](decision-guide.md): scale, ambiguity, risk,
spec-exists, recurrence, duration.

The shared backbone every method maps onto: **Frame → Decide → Specify →
Plan → Build → Verify → Compound**. A method is a different way of
running through these phases, not a different backbone.

---

## Lightweight / ad-hoc

**What it is:** No formal method. Restate the task in one line, then go
straight to Build → Verify.

**Signal pattern:** scale trivial–small; ambiguity none; risk low; no
spec needed because none is warranted.

**Phases emphasized:** Build, Verify only. Frame collapses to a one-line
restatement; Decide, Specify, Plan, and Compound are skipped.

**Canonical reference:** none — this is the deliberate absence of formal
process, not an omission. If a citation is wanted for the underlying
philosophy, see general "do the simplest thing that could possibly work" /
YAGNI folklore; there is no single canonical source.

---

## TDD (Test-Driven Development)

**What it is:** Red → green → refactor: write a failing test, write the
minimal code to pass it, then refactor.

**Signal pattern:** the "what" is already decided or can be captured as
acceptance criteria quickly; scale small–medium (a function, module,
endpoint, or bug fix); correctness and regression-safety matter. Pairs
poorly with heavy product-shape ambiguity.

**Phases emphasized:** Build (primary — defines how Build happens), Verify
(folded into Build via the red/green/refactor loop). Assumes Frame,
Decide, and Specify are already resolved.

**Canonical reference:** Kent Beck, *Test-Driven Development: By Example*
(Addison-Wesley, 2002).

---

## Spec-Driven Development (SDD)

**What it is:** A written spec is the source of truth the agent implements
against, rather than a throwaway prompt. Canonicalized by GitHub's
open-source Spec Kit workflow: `Specify → Plan → Tasks → Implement`.

**Signal pattern:** the product/feature shape is still being defined, or
has multiple stakeholders; scope spans multiple files or services; a
reviewable artifact is needed before code is written; scale medium–large.

**Phases emphasized:** Specify (primary), Decide, Plan. Build and Verify
follow from the spec.

**Canonical reference:** <https://github.com/github/spec-kit>

---

## Compound Engineering

**What it is:** A loop — Plan → Work → Review → Compound — where every
unit of work is expected to make the next unit easier. Bugs, reviews, and
decisions become durable, reusable knowledge instead of one-off effort.

**Signal pattern:** a recurring or long-lived codebase/product, not a
one-off script; the team or agent will repeat similar tasks over time;
there's value in institutional memory; scale medium–large, with enough
time to write things down.

**Phases emphasized:** all phases, but distinctively gives Compound
(capturing learnings for next time) real weight as a first-class phase
rather than an afterthought.

**Canonical reference:** <https://github.com/EveryInc/compound-engineering-plugin>

---

## BMAD (Breakthrough Method for Agile AI-Driven Development)

**What it is:** An open-source framework of specialized AI agent personas
(analyst, PM, architect, dev, QA, and others) plus guided workflows that
carry a product idea through structured phases into an implemented app.
Role- and persona-driven.

**Signal pattern:** greenfield work, or a major feature, where "what
should we build, for whom" is still genuinely open; benefits from distinct
planning/architecture/dev/QA personas; wants explicit PRD and architecture
artifacts before coding; high ambiguity about product shape.

**Phases emphasized:** Frame, Decide, Specify (via PM/architect personas),
Plan; hands off to Build and Verify via dev/QA personas.

**Canonical reference:** <https://github.com/bmad-code-org/BMAD-METHOD>

---

## GSD (context-engineering / long-task management)

**What it is:** A phase loop — Discuss → Plan → Execute → Verify → Ship —
built to fight context degradation on long-running agent tasks, by running
heavy work in fresh-context subagents and breaking a project into small,
atomic, commit-sized steps.

**Signal pattern:** the task or project is long-running or spans multiple
sessions; a single context window would otherwise fill up and degrade
quality; the work decomposes into atomic steps; duration is the dominant
risk factor, more so than ambiguity.

**Phases emphasized:** Plan (heavily), Build (via fresh-context
execution), Verify (per step). Assumes Frame and Decide are largely
settled already.

**Canonical reference:** <https://github.com/open-gsd/gsd-core> — an
actively maintained community continuation. Note for future maintainers:
the original repository this method traces back to has been inactive
since around April 2026; link to the active fork above, not a dead one.

---

## Superpowers

**What it is:** A composable skills library (brainstorming, TDD,
systematic debugging, git worktrees, code review, and more) plus
orchestration instructions that make an agent actually use them in
sequence — strong-constraint execution: it prescribes not just what to do,
but how the agent must behave at each step.

**Signal pattern:** the task benefits from strict process discipline and
verification gates; the agent is prone to skipping steps or cutting
corners on this kind of task; correctness/discipline risk is high.

**Phases emphasized:** Build, Verify (primary — the strong constraints
live here), plus Frame and Plan via its own brainstorming/planning skills.

**Canonical reference:** <https://github.com/obra/superpowers>

---

## Phase-coverage quick reference

`primary` = the method's main lever in that phase · `secondary` = present
but not the main lever · `minimal` = touched only lightly · `skip` = not
used by this method.

| Method | Frame | Decide | Specify | Plan | Build | Verify | Compound |
|---|---|---|---|---|---|---|---|
| Lightweight | minimal | skip | skip | minimal | primary | primary | skip |
| TDD | assumed | assumed | assumed | minimal | primary | primary | skip |
| Spec-Driven Development | secondary | secondary | primary | primary | secondary | secondary | skip |
| Compound Engineering | primary | secondary | secondary | primary | primary | primary | primary |
| BMAD | primary | primary | primary | primary | secondary | secondary | minimal |
| GSD | minimal | assumed | secondary | primary | primary | primary | minimal |
| Superpowers | secondary | secondary | secondary | secondary | primary | primary | minimal |

"assumed" marks a phase the method doesn't actively work through but
presupposes has already been resolved elsewhere.
