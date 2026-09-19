# Roadmap

Lightweight on purpose. This is engineering work that runs experiments when there is a reason to,
not a research program with a schedule.

## Now

- **Design V2 of agentic-review-swarm.** Broad systems reviewer, specialists preserved, arbiter,
  explicit semantics for unowned findings, findings and assurance record as separate outputs. A new
  ADR supersedes ADR 0004 and keeps it in place. Seeds for cross-cutting reasoning; tests for the
  arbiter. Existing V1 checks and seeds must still pass, and any V1 behavior that changes is named as
  intentional or as a regression.
- **Tag V1.** The swarm commit Experiment 001 tested gets a tag and a release so the study can cite
  a name instead of a hash.
- **Outside review of Experiment 001's verdicts.** Disputes become logged corrections.

## Next

- **Use V2 on real development work** and collect its failures the way V1's were collected, in a
  review log, before measuring it formally.
- **First-principles review of the security charter**, documented separately from V2. The specialist
  lost its lane to two generalists and the reason is not known. Adding the missed findings to its
  checklist is not the plan.

## Eventually

- **Experiment 002**, on a mature repository with existing tests, CI/CD, infrastructure and
  deployment configuration, documentation and real business logic. Four arms: naive generalist,
  strong generalist, V1, V2. Predictions sealed first. Cold naive prompts from people who have not
  read 001. Rubrics applied. Second judge. Cost recorded for every arm.
- **Compare V1 and V2 formally only when there is enough reason to spend the compute.** V2 in field
  use with a review log may answer most of the question before an experiment is needed.

## Not planned

- Adding a specialist per defect Experiment 001 found.
- Dynamic agent spawning. The domains are known.
- Claiming V2 is better before it is measured.
