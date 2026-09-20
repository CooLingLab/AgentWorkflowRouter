---
name: agent-workflow-router
description: Use before starting a coding task to decide which engineering methodology (TDD, Spec-Driven Development, Compound Engineering, BMAD, GSD, Superpowers, or lightweight/none) and which supporting skills fit, based on task scale, ambiguity, and risk. Triggers on requests like how should I approach, build, or plan this, or what workflow should I use. Skips itself for trivial one-line changes or when a method is already chosen.
license: MIT
---

# Agent Workflow Router

## Purpose

Before starting a coding task, pause and decide two things: whether a formal
engineering method is even warranted, and if so, which single method should
orchestrate the work plus which supporting capabilities (research, TDD,
code review, security review, ...) should layer on top of it. This skill
exists so that choice is made deliberately, not defaulted into.

## Core principles

These are constraints on how this skill behaves, not suggestions:

- **Recommend only.** Never install, run, or treat any capability as "in
  use" without the user's explicit go-ahead. This skill produces a
  recommendation, not an action.
- **Exactly one primary/orchestrating method at a time.** Never propose two
  methods both acting as the orchestrator for the same task. Supporting
  capabilities may stack freely underneath the one primary method.
- **Popularity is a minor input, never a promotion rule.** A trending
  GitHub repo does not become a new top-level method just because it's
  popular right now. New entries in `references/methods.md` are added only
  by a deliberate maintainer PR, never inferred on the fly.
- **Check "no formal method" first.** Do not manufacture process for a task
  that doesn't need it.
- **When signals conflict or are missing, say so.** Output `ambiguous` or
  `need_more_context` rather than guessing past a real gap in information.

## When this applies

Typical triggers: "how should I approach/build/plan this", "what workflow
should I use for X", "help me think through how to implement Y", starting
work on a new feature, or any moment before committing to an approach for a
non-trivial change.

Non-triggers that still route to an internal `bypass`, rather than being
skipped at the description level: the user has already stated which method
to use, or the task is a genuinely trivial one-line change. In both cases,
this skill should still activate briefly, recognize that, and get out of
the way quickly — see Step 2.

## Step 1 — Triage the signals

From the request itself, plus a light look at repo context (README,
package manifest, existing docs — not a deep crawl), establish:

- **Scale**: trivial / small / medium / large
- **Ambiguity**: is the "what" and "how" already decided, or still open?
- **Risk**: could this cause data loss, a security issue, production
  breakage, or something hard to reverse?
- **Spec-exists**: is there already a written spec/design/product shape?
- **Recurrence**: one-off task, or part of a long-lived codebase/product?
- **Duration**: fits in one session, or spans multiple sessions?

## Step 2 — Check the outcome state first

Before picking a method, check in this order:

1. **`bypass`** — scale is trivial, ambiguity is none, risk is low. Say so
   in one line and move directly to Build → Verify. Do not proceed further.
2. **`need_more_context`** — ambiguity is high, and the missing fact is
   answerable with one or two specific questions. Ask them. Do not guess
   past this point.
3. **`ambiguous`** — two or more methods fit comparably well, and no
   clarifying question would resolve it because it's a genuine judgment
   call. Present the top two with a one-line trade-off each and let the
   user decide.
4. Otherwise, proceed to Step 3.

The full rules and exact output wording for each state are in
[references/decision-guide.md](references/decision-guide.md).

## Step 3 — Read the decision guide

Read [references/decision-guide.md](references/decision-guide.md) for the
full signal-to-method matrix, tie-breaking heuristics for close calls, and
the capability-scoring checklist used when two supporting capabilities
could fill the same slot.

## Step 4 — Read the method registry

Read [references/methods.md](references/methods.md) for the candidate
method's actual definition, the signal pattern that suggests it fits, which
phases it emphasizes, and its canonical reference — before finalizing a
recommendation.

## Step 5 — Read the capability catalog

Read [references/capability-catalog.md](references/capability-catalog.md)
to fill in supporting capabilities per phase. This file is an explicit
starting point, not exhaustive — a capability outside it may still be
recommended, but flag it as unvetted when you do.

## Output format

For a real recommendation (not `bypass`/`ambiguous`/`need_more_context`),
use this shape:

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

The exact wording for `bypass`, `ambiguous`, and `need_more_context` is in
[references/decision-guide.md](references/decision-guide.md).

## What this skill does not do

- Does not install, run, or configure any third-party skill, tool, or
  package.
- Does not persist preferences or state across sessions.
- Does not scan GitHub, marketplaces, or the internet — the capability
  catalog is a static file, maintained by hand.
- Does not override a method the user has already explicitly chosen for
  this task.
