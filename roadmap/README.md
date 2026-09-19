# Roadmap

Lightweight on purpose. This is engineering work that runs experiments when there is a reason to,
not a research program with a schedule.

## Now

- **V2.0 is built and evaluated** (done 2026-09-19): systems reviewer, arbiter, unowned findings,
  disagreements, coverage caps, two outputs, ADR 0006. Component runs and an integrated run are
  preserved and graded in the swarm repository under `docs/evals/`. V1 is tagged
  `v1-experiment-final`.
- **Land the tool-execution fix on V2.** The fix branch Experiment 001's tools condition ran on was
  never merged; V2 was cut from `main` without it. A merge, not new work.
- **Repair the fixtures the reviewers found defects in** (five known) and make the seed runner's
  term check tolerant of paraphrase and of non-ASCII on macOS. Earlier gradings stay as graded.
- **Outside review of Experiment 001's verdicts.** Disputes become logged corrections.

## Next

- **Use V2 on real development work** and collect its failures the way V1's were collected, in a
  review log, before measuring it formally. V2 has reviewed zero real pull requests.
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

## The question after V2

Every reviewer still investigates once and returns a report; nothing inspects intermediate evidence
and decides whether to continue. The next research question, not yet designed: does review quality
improve when static reviewers become managed investigators, able to gather evidence iteratively,
choose tools, revise hypotheses, and be directed to continue when their evidence is incomplete?

## Not planned

- Adding a specialist per defect Experiment 001 found.
- Dynamic agent spawning. The domains are known.
- Claiming V2 is better before it is measured.
