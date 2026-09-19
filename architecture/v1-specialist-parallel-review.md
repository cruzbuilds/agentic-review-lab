# V1: specialist parallel review

The architecture that was tested in Experiment 001. Implementation:
[agentic-review-swarm](https://github.com/cruzbuilds/agentic-review-swarm). Tested at commit
`a18fba7` (tools condition) and `7461982` (reading-only condition), both recorded per run in the
experiment's `reports/runs.csv`.

## What V1 assumed

1. Comprehensive review decomposes into independent lanes. Security, tests, infrastructure,
   documentation and scope can each be inspected on their own, in parallel, by a reviewer that
   knows nothing about the others' work.
2. A narrow charter with deeper instructions finds more inside its lane than a general instruction
   would. Direction is the variable that matters, and more of it, more specifically, is better.
3. A general reviewer is a liability. It has no lane, overlaps everyone, adds style opinions, and
   buries the one thing only it noticed under twenty things everyone noticed. So there is not one,
   by decision ([ADR 0004](https://github.com/cruzbuilds/agentic-review-swarm/blob/main/docs/decisions/0004-no-general-code-reviewer.md)).
4. Anything that falls between lanes is a signal to write a new specialist, not to loosen the lanes.
   Meanwhile it is reported as "no owner in the roster" so a human sees it.
5. Reviewers are critics, not builders. They never change code
   ([ADR 0003](https://github.com/cruzbuilds/agentic-review-swarm/blob/main/docs/decisions/0003-critics-not-builders.md)).

## Why narrow parallel specialists seemed attractive

- **A charter is readable.** Each reviewer is a page of English saying what it blocks on and what it
  leaves alone. A team can read it, argue with it, and change it. A tuned prompt nobody can read
  cannot be audited.
- **Seeds prove each one.** Every reviewer is run against planted defects and clean cases before it
  reviews anything real. That only works when the reviewer's job is narrow enough to seed.
- **Lanes keep the merged report short.** Five reviewers that each stay in their lane produce one
  report with little duplication. A general reviewer would double every lane.
- **The domains nobody scans for are exactly the ones a specialist can be told to scan for.**
  Published baselines exist for security. None exist for tests that cannot fail, README drift,
  resources with no teardown, or work outside scope. A charter is how you get a reviewer to look
  where no tool looks.
- **It worked in the field.** On a real project it reviewed every pull request, 37 findings, none
  overridden, and by the author's count 21 of the 33 actionable ones would have been missed reading
  the diff alone. It also blocked changes to itself twice in one afternoon and was right both times.

## Strengths observed in Experiment 001

- **Decomposition.** The test reviewer turned "there is no test suite" into thirteen module-level
  items with the assertions to write, all thirteen in every run. Each generalist said it once.
- **Repeatability.** 66% of the swarm's confirmed findings appeared in all three runs, against 44%
  for the strong generalist and 60% for the naive one.
- **The inspection record.** Every run produced a per-agent account of which reviewer checked which
  domain, which tools ran and which failed, which files were not read, where agents disagreed, and
  what was noticed but owned by nobody. Nothing else in the study produced that.
- **Precision held.** 79% strict, within two points of both generalists. Five reviewers hunting in
  lanes did not produce five times the noise.
- **Lane hygiene.** Documentation and infrastructure findings the generalists did not bother with:
  healthchecks, volume teardown, restart policy, README drift, undocumented API routes.

## Failure modes observed in Experiment 001

- **The unowned bucket is where the best finding went.** The subject lets a user rewrite an idea's
  original scores after recording the outcome, which silently falsifies the calibration view the
  product exists to show. The test reviewer noticed it in two of six runs. The merged report placed
  it under "Handoffs nobody picked up" with "no agent in the roster owns this." It never became a
  finding with a severity, never affected a verdict, and never appeared in any count the swarm
  produced. Both generalists found it as a finding, one of them every run.
- **The security specialist lost its own lane, twice.** 7 confirmed security findings against 11
  from a generalist told to check security and 11 from one that was not. Cause not established.
  Candidate explanations: a checklist-shaped charter that did not fit this application; security
  findings that need whole-system context a lane does not have; lane rules suppressing cross-domain
  reasoning (an enumeration leak in one run was caught by the test reviewer, not the security
  reviewer); or a subject that favors broad reasoning.
- **No correctness lane at all.** Eleven confirmed correctness findings from the strong generalist,
  ten from the naive one, three from the swarm, and those three only because they happened to touch
  a specialist's charter.
- **Out-of-lane observations are discarded by design.** Every charter ends with what the reviewer
  must not comment on. Performance and accessibility findings the generalists made were structurally
  unavailable to the swarm.
- **The assumption that more direction finds more did not hold at the high-value tier.** A
  twenty-four-word prompt found every merge-blocking defect in every run. Direction bought breadth in
  hygiene lanes and a work list. It did not buy detection of the things that matter most.

## What V1 got right that V2 must keep

Charters as readable English. Seeds before any real review. Critics, not builders. Reviewers get git
and only git ([ADR 0005](https://github.com/cruzbuilds/agentic-review-swarm/blob/main/docs/decisions/0005-reviewers-get-git-not-a-shell.md)). One report a
developer can act on. The per-agent record of what was and was not checked.

## Decision records in the implementation

- [ADR 0001, record architecture decisions](https://github.com/cruzbuilds/agentic-review-swarm/blob/main/docs/decisions/0001-record-architecture-decisions.md)
- [ADR 0002, one repo per agent install](https://github.com/cruzbuilds/agentic-review-swarm/blob/main/docs/decisions/0002-one-repo-per-agent-install.md)
- [ADR 0003, critics not builders](https://github.com/cruzbuilds/agentic-review-swarm/blob/main/docs/decisions/0003-critics-not-builders.md)
- [ADR 0004, no general-purpose code reviewer](https://github.com/cruzbuilds/agentic-review-swarm/blob/main/docs/decisions/0004-no-general-code-reviewer.md). In force during Experiment 001. To be superseded, not erased, by the V2 decision.
- [ADR 0005, reviewers get git, not a shell](https://github.com/cruzbuilds/agentic-review-swarm/blob/main/docs/decisions/0005-reviewers-get-git-not-a-shell.md)
- [Design notes](https://github.com/cruzbuilds/agentic-review-swarm/blob/main/docs/design.md) and [eval log](https://github.com/cruzbuilds/agentic-review-swarm/blob/main/docs/eval-log.md)
