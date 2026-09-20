# agent-workflow-router

AI coding agents like Claude Code and OpenAI Codex CLI are good at writing
code, but not at knowing *how* to approach a task before writing it —
whether to write a spec first, write a failing test first, prototype
first, or just dive in. This repo packages that decision as one portable
[Agent Skill](https://agentskills.io/specification) — a small capability
format that Claude Code, Codex, and about 40 other agent tools all read
natively, no conversion step needed.

Drop it into your agent's skills folder, and before it starts a
non-trivial coding task, it pauses, looks at the task's scale, ambiguity,
and risk, and tells you which engineering method — TDD, Spec-Driven
Development, Compound Engineering, BMAD, GSD, Superpowers, or none at all
— and which supporting practices (research, prototyping, code review,
security review, ...) actually fit. **It never installs or runs anything
by itself. It only recommends.**

## What it looks like in practice

A non-trivial request:

```
You: Add a permissions system to my SaaS app

Primary method: Compound Engineering — recurring, long-lived product;
getting the permission model right pays off repeatedly, and there's
time to document decisions for next time.

Phases in play:
  Frame:    map current roles/access patterns          [research]
  Decide:   choose a permission model (RBAC vs ABAC)    [prototype / spike]
  Specify:  write the access-control spec               [spec-writing]
  Plan:     break into milestones
  Build:    implement authorization logic                [tdd]
  Verify:   review before merge                           [code-review, security-review]
  Compound: write up the model for future features         [documentation]

Deliberately excluded:
  BMAD — overkill; this is a feature in an existing product, not a
  from-scratch product-definition exercise.
  Superpowers — not needed unless you specifically want that
  constraint style.

Recommend only — nothing installed or run. Say if any signal above looks wrong.
```

A trivial request:

```
You: Change the button color to blue

bypass — trivial, low-risk, single-file UI tweak.
Proceeding directly to Build → Verify.
```

## Install — Claude Code

```bash
# Personal (all your projects):
mkdir -p ~/.claude/skills
cp -r skills/agent-workflow-router ~/.claude/skills/agent-workflow-router
# — or, to track future updates via git instead of copying —
ln -s "$(pwd)/skills/agent-workflow-router" ~/.claude/skills/agent-workflow-router

# Project-only (just one repo):
mkdir -p /path/to/your-project/.claude/skills
cp -r skills/agent-workflow-router /path/to/your-project/.claude/skills/agent-workflow-router
```

Restart Claude Code, then try: "How should I approach adding a
permissions system to my app?"

## Install — Codex CLI

```bash
# Personal (all your projects):
mkdir -p ~/.agents/skills
cp -r skills/agent-workflow-router ~/.agents/skills/agent-workflow-router

# Project-only:
mkdir -p /path/to/your-project/.agents/skills
cp -r skills/agent-workflow-router /path/to/your-project/.agents/skills/agent-workflow-router
```

Some older Codex builds and docs reference `.codex/skills/` instead of
`.agents/skills/`. If the skill doesn't get picked up, try that path too —
`.agents/skills/` is the currently documented location as of this
writing.

## Extending or updating this router

This is meant to be maintained by more than one person over time:

- **To add a method:** edit
  [`skills/agent-workflow-router/references/methods.md`](skills/agent-workflow-router/references/methods.md),
  following the existing entry template (what it is / signal pattern /
  phases emphasized / canonical reference — use a real link, or write
  "link TBD" if you don't have one; never invent one). Add a row to the
  matrix in
  [`decision-guide.md`](skills/agent-workflow-router/references/decision-guide.md)
  too. Open a PR.
- **To add or update a supporting capability:** add a row to
  [`capability-catalog.md`](skills/agent-workflow-router/references/capability-catalog.md)
  — name, one-line description, phase(s). Open a PR.
- This is **intentionally hand-curated**, not auto-synced from GitHub or
  any marketplace. There's no bot and no scheduled job here. That's
  deliberate: it keeps one trending repo from silently redefining what
  "the" methodology is. See the skill's own "Core principles" section in
  `SKILL.md`.

## Repo hygiene

- **License:** MIT — see [`LICENSE`](LICENSE).
- **What this skill does not do:** it does not install or run any
  third-party skill, tool, or package automatically; it does not execute
  shell commands; it ships no scripts at all — it only reads its own three
  reference files and reasons; it does not persist preferences or state
  across sessions; it does not scan the internet or phone home — the
  capability catalog is a static file, edited by hand.
