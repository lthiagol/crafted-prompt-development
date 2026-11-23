# Harness

The guideline doesn't depend on a tool. Subagents in one CLI session, agents
in separate terminals and mixed vendors all work if the harness provides the
following.

## Requirements

- **Fresh-context agents.** A new builder per brief. A reviewer that lives for
  one unit (so it doesn't re-raise what it settled) and is cleared or replaced
  between units.
- **File hand-off.** Briefs, reports and reviews in a round folder that every
  agent can read and write. Agent summaries drop detail.
- **Completion signal.** The orchestrator waits for the **file** (report,
  review), not for an agent to look idle.
- **No interactive prompts.** Agents run without anyone answering questions
  or permission dialogs, so permissions are configured up front. Builders write
  questions into the report.
- **Round limit and verdict parsing.** The loop stops on a parseable verdict
  or on the cap.
- **Orchestrator owns git.** Builders never commit, stage, revert or stash.

## Failure modes seen in practice

- **A permission prompt stalls the loop.** A sub-agent can't answer it.
  Smoke-test permissions with one trivial agent before the run.
- **A builder ends without a report.** Treat it as failed. Re-brief a new
  builder; never reuse its half-done claims.
- **HEAD moved during a unit.** Someone committed. Stop and check before
  reviewing.
- **Per-agent isolated worktrees from the default branch.** The reviewer
  doesn't see the builder's work. Share one tree per unit; for parallel
  units, create a worktree per unit yourself.
- **Parallel builds on one machine.** They contend for CPU, memory and
  locks. Run builds one at a time; research and reviews can run in parallel.
- **Destructive commands get refused for sub-agents.** The orchestrator runs
  reverts itself, after checking the diff contains only that round's work.
- **Disk fills up** from builds and test runs. Clean up between units.
