# Contracts

The artifacts that move between roles. Fill-in versions are in
[templates/](templates/).

| Artifact | From → to | Template |
|---|---|---|
| Brief | orchestrator → builder | [brief.md](templates/brief.md) |
| Report | builder → orchestrator, reviewer | [report.md](templates/report.md) |
| Review | reviewer → orchestrator | [review.md](templates/review.md) |
| Builder rules | read by every builder | [builder-rules.md](templates/builder-rules.md) |
| Review rules | read by every reviewer | [review-rules.md](templates/review-rules.md) |

Field names are suggestions; the content is required.

## Brief

Done-when, ACs each with a verification, scope, out of scope, docs to read,
decisions of record, optional guidance, report path. See
[briefing.md](briefing.md).

## Report

Per AC: status and evidence (command → exit code). Files touched, tests added
and whether they fail without the change, gate result, deviations, open
questions. Always written, also when the builder fails or stops early.

## Review

Per finding: severity, location, defect, concrete failing scenario, fix, and
the decision needed, if any. The last line is a machine-parseable verdict:
`VERDICT: CLEAN` or `VERDICT: FINDINGS <n>`. A review without a verdict line
is not finished.

## Rules files

Copied into each run's round folder and filled in with the gate command and
the project's own rules. They hold what applies to every unit; briefs hold
what applies to one.

## Record

Round files can be temporary. What must survive goes into the project:

- **Commit message**: what changed and why; ACs deferred, if any.
- **Plan / state file**: unit status, deferred scope, follow-ups.
- **Rules / `AGENTS.md`**: lessons that should change future briefs or reviews.
