# Contracts

The four artifacts that move between roles. Field names are suggestions;
the content is required.

## Brief (orchestrator → builder)

```markdown
## Unit <id> · <title>

**Done when:** <one observable sentence>

**Acceptance criteria**
- [ ] AC1: <check> — verify: `<command>` (or the exact manual check)
- [ ] AC2: …

**Scope:** <files/modules it may touch>
**Out of scope:** <what it must not do, even if tempting>
**Read first:** <docs to read: link them, don't paste them>
**Decisions of record:** <choices already made; don't revisit>
**Guidance:** <optional steps; follow them or deviate and say why in the report>

REPORT=<path>
```

## Report (builder → orchestrator, reviewer)

```markdown
| AC | Status | Evidence |
|----|--------|----------|
| AC1 | done | `<command>` → exit 0 |
| AC2 | not done | <why> |

Files touched: …
Tests added: <test> fails without the change: yes/no/not provable
Gate: `<gate command>` → exit <n>
Deviations from guidance: …
Open questions: <question + recommendation>  ← builders never ask interactively
```

The report is always written, also when the builder fails or stops early.

## Review (reviewer → orchestrator)

```markdown
## D1 [high|medium|low] <title>
- where: path:line
- defect: one sentence
- scenario: concrete input or state → wrong result
- fix: what a correct fix looks like
- decision needed: <only if outside the docs; options + recommendation>

VERDICT: CLEAN | VERDICT: FINDINGS <n>
```

The verdict is the last line, in a machine-parseable form. A review without a
verdict line is not finished.

## Record (orchestrator → project)

Round files (briefs, reports, reviews) can be temporary. What must survive goes
into the project:

- **Commit message**: what changed and why; ACs deferred, if any.
- **Plan / state file**: unit status, deferred scope, follow-ups.
- **Rules / `AGENTS.md`**: lessons that should change future briefs or reviews.
