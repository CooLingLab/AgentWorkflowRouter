# Decision Guide

## Signals glossary

Six dimensions, shared with [methods.md](methods.md):

| Signal | Values | Meaning |
|---|---|---|
| Scale | trivial / small / medium / large | How much surface area the change touches |
| Ambiguity | none / some / high | How settled the "what" and "how" already are |
| Risk | low / medium / high | Chance of data loss, security issues, production breakage, or an irreversible mistake |
| Spec-exists | yes / no | Whether a written spec/design/product shape already exists |
| Recurrence | one-off / recurring | Whether this is a throwaway task or part of a long-lived codebase/product |
| Duration | single-session / multi-session | Whether the work plausibly finishes in one sitting |

## Outcome-state rules

Check these in order, before selecting a method. Stop at the first one
that applies.

### 1. `bypass`

**Condition:** scale = trivial AND ambiguity = none AND risk = low.

**Output:**
```
bypass — trivial, low-risk, <one-line description of the change>.
Proceeding directly to Build → Verify.
```

### 2. `need_more_context`

**Condition:** ambiguity = high, AND the missing fact is answerable with
one or two specific questions.

**Output:**
```
need_more_context — before recommending an approach, I need to know:
1. <specific question>
2. <specific question, if needed>
```

Never guess past this point. A vague or generic clarifying question is a
sign the real question hasn't been found yet — keep it specific to what's
actually missing.

### 3. `ambiguous`

**Condition:** two or more methods score comparably in Section "Matrix"
below, AND no clarifying question would resolve it — it's a genuine
judgment call, not a missing fact.

**Output:**
```
ambiguous — this could reasonably go either way:
1. <method A> — <one-line trade-off>
2. <method B> — <one-line trade-off>
Which fits better, or is there a signal I'm missing?
```

### 4. Otherwise

Proceed to the method-selection matrix below.

## Method-selection matrix

Qualitative fit, drawn from each method's signal pattern in
[methods.md](methods.md):

| Method | Scale | Ambiguity | Risk | Spec-exists | Recurrence | Duration |
|---|---|---|---|---|---|---|
| Lightweight | trivial–small | none | low | n/a | either | single |
| TDD | small–medium | none–some | any | yes, or quickly captured | either | either |
| Spec-Driven Development | medium–large | some–high | any | no (spec is the output) | either | either |
| Compound Engineering | medium–large | some | any | either | recurring | either, often multi |
| BMAD | medium–large | high | any | no (PRD/architecture is the output) | either | often multi |
| GSD | any, esp. large | some | any | either | either | multi-session |
| Superpowers | any | some | high (discipline matters) | either | either | either |

## Tie-breaking heuristics

Worked examples for close calls:

- **SDD vs. BMAD** — choose BMAD when distinct personas/roles and heavier
  PRD-style ceremony are wanted (e.g. a genuinely new product direction).
  Choose SDD when a single spec artifact with lighter ceremony is enough
  (e.g. a well-understood feature added to an existing product). Worked
  example: "add a permissions system to an existing SaaS app" leans SDD or
  Compound Engineering, not BMAD — the product already exists, only the
  feature's shape needs specifying.
- **Compound Engineering vs. GSD** — choose GSD when the dominant risk is
  context/duration management on one specific task (it will span many
  sessions and needs to survive that). Choose Compound Engineering when
  the dominant goal is reusing knowledge across many future tasks in a
  codebase that will keep being worked on.
- **TDD vs. Lightweight** — choose TDD once there's a testable behavior
  worth locking down, even for a small task. Choose Lightweight only when
  even that ceremony isn't worth it (e.g. a copy change, a color tweak).

## Capability-scoring guidance

When two supporting capabilities could fill the same phase slot, this
repo does not compute a numeric score — there is no scraping pipeline or
stored preference data to feed one, so presenting a formula would be
decorative rather than actionable. Use this ordered, qualitative checklist
instead:

Prefer the capability that, roughly in this order:

1. Actually fits what this task needs.
2. Is complete enough to use as-is for the method in play.
3. Shows signs of active maintenance.
4. Is compatible with the agent/tool currently running.
5. Has a real adoption/usage track record.
6. Matches any explicit preference the user has stated in this
   conversation.

Downgrade or flag — don't silently exclude — anything that:

- Requires broad or unreviewed permissions, or runs commands
  automatically without confirmation.
- Adds process overhead clearly disproportionate to the task's scale.

## Full output template

```
Primary method: <name> — <one-line why>

Phases in play: Frame / Decide / Specify / Plan / Build / Verify / Compound
  (mark which are skipped and why)

Supporting capabilities:
  <phase>: <capability> — <why>
  <phase>: <capability> — <why>

Deliberately excluded:
  <method or capability> — <why not>

Recommend only — nothing installed or run. Say if any signal above looks wrong.
```
