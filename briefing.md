# Briefing

A brief is good if a capable agent with no chat history can finish the unit
without asking anything, and a reviewer can tell if it's done.

## Checklist

- **Done when** is observable, not "implement X". A reader can say yes or no.
- **Every AC has a verification**: a command, ideally a test written to fail
  first; otherwise an exact manual check.
- **Out of scope is explicit.** Agents expand scope to things that look
  related; name those things.
- **Scope lists the files or modules it may touch.** Touching others is a
  finding.
- **Point, don't paste.** Link specs and docs. Pasted copies go stale and eat
  context.
- **Decisions of record are stated**, so the builder doesn't re-litigate them,
  and a contradiction gets reported instead of forced through.
- **One unit = one reviewable diff.** If the review would need more than one
  sitting, split the unit.

## How prescriptive

| Builder | Brief contains |
|---|---|
| Small / cheap model | Outcome, ACs, scope, and step-by-step guidance with file names |
| Strong model | Outcome, ACs, scope, constraints (interfaces, invariants); steps only for risky paths |

Steps are always guidance. The builder may deviate if it says why in the
report.

## Brief vs `AGENTS.md`

| Goes in the brief | Goes in `AGENTS.md` / project docs |
|---|---|
| This unit's outcome, ACs, scope | Conventions, commands, the gate |
| Decisions specific to this unit | Architecture, decisions of record |
| Pointers to relevant docs | Lessons that apply to every unit |

If you write the same line into a third brief, move it to `AGENTS.md`.

## From idea to units

1. **Discover**: talk the problem through until the goal and constraints are
   clear. No brief yet.
2. **Plan** (heavy tier): list units in dependency order. Each must be
   committable on its own and leave the gate green.
3. **Brief** each unit when it's next. Later briefs benefit from what earlier
   units revealed.
4. **Approve**: the human reads the brief and asks "would I trust a capable
   agent with this as it stands?"

## Example

Weak:

> Add rate limiting to the API.

Better:

> **Done when:** requests over the limit get `429` with `Retry-After`.
> - AC1: 101st request in 60 s from one key → 429 — verify: `pytest tests/test_ratelimit.py`
> - AC2: limits are configurable per route — verify: test with two routes, two limits
>
> **Scope:** `api/middleware/`, `config/limits.py`, tests.
> **Out of scope:** distributed limits (single process is fine), auth changes.
> **Decisions of record:** in-memory token bucket (see `docs/adr/007.md`).
