# Verifying

Green tests are where review starts, not where it ends.

## 1. Gate

Deterministic and run by the builder before reporting. The reviewer runs it
once more. A red gate is the builder's problem, not a review finding.

## 2. Review

The reviewer reads the **brief**, the **diff** and the **report**, in that
order of trust.

- **Done-when, read literally.** Is the outcome there, or was it papered over?
- **Each AC against its evidence.** Re-run the verification commands; don't
  take the report's word for them.
- **Tests test something.** Would each new test fail without the change?
- **Scope.** Files outside scope, or out-of-scope behaviour, are findings.
- **New defects.** Try to break it: boundary inputs, error paths, concurrency,
  invariants the docs state.
- **Only the change.** Don't re-review untouched code. Paths that were dirty
  before the unit started don't belong to it.

A finding without a concrete failing scenario is an opinion, not a finding;
leave it out.

**Heavy tier:** add a second pass with a different frame (security, edge cases,
spec compliance), using a different agent and ideally a different model.
Different frames find different bugs.

## 3. Triage (orchestrator)

For each finding:

- **Wrong** → drop it, and note why for the reviewer.
- **Needs a decision beyond the docs** → decide if small and reversible,
  otherwise ask the human.
- **Valid** → batch it into the fix brief: "fix D1, D3; nothing else."

The fix goes to a builder. The same reviewer, which knows what it already
settled, reviews the fix only.

## 4. Close

- Close on `VERDICT: CLEAN`, or when the round cap is hit and the leftovers are
  explicitly deferred or escalated.
- Commit only the unit's paths. Never `add -A`.
- Record per [contracts.md](contracts.md#record).

## Human checkpoints

| When | The human checks |
|---|---|
| Before build | The brief: right problem, right scope, falsifiable ACs |
| Escalation | Decisions of record, scope changes, stuck loops |
| After commit | The outcome, against intent: accept, or write a follow-up brief |

Follow-ups are new units, not edits to a closed one.

## Learn from runs

The guideline improves only from evidence. Keep round files for a few real
runs and look for:

- briefs that caused extra rounds → what was missing from them?
- findings that recur → turn them into a builder or review rule
- defects that passed review → turn them into a review checklist item
- builder questions in reports → missing decisions of record
