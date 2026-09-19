# Experiment 001: findings summary

The readable version. Every number below is from
[ANALYSIS.md](https://github.com/cruzbuilds/Five-Critics-or-One-Good-Prompt/blob/main/ANALYSIS.md)
in the experiment repository and can be regenerated from its `findings/` directory. This page
separates what was observed from what I make of it.

## Observed

Fifteen reports, 79 distinct claims after de-duplication, each checked against the frozen code. A
confirmed defect ("strict true positive") means the code is as described and nothing elsewhere
prevents the failure. 64 of 79 qualified.

| | C, naive generalist | A, strong generalist | B, V1 swarm |
| --- | --- | --- | --- |
| Prompt | 24 words | 170 words | five charters, contract, orchestrator |
| Distinct claims | 37 | 47 | 48 |
| Precision (strict) | 81% | 81% | 79% |
| Confirmed defects, all domains | 30 | 38 | 38 |
| Confirmed defects, the swarm's five domains | 18 | 24 | 35 |
| Same, without tests | 17 | 23 | 22 |
| Merge-blocking defects found (of 4) | 4 | 4 | 3 |
| Merge-blocking found in every run | 4 | 3 | 2 |
| Calibration defect found as a finding | 3 of 3 runs | 5 of 6 | 0 of 6 |
| Missing test suite noticed | 3 of 3, as one item | 6 of 6, as one item | 6 of 6, as thirteen items |
| Security defects | 11 | 11 | 7 |
| Correctness defects | 10 | 11 | 3 |
| Findings seen in all three runs (tools condition) | 60% | 44% | 66% |
| Report length, words | 877 to 1,199 | 2,176 to 2,901 | 1,708 to 2,136 |
| Per-agent record of what was and was not checked | no | no | every run |

Per domain, confirmed defects:

| Domain | C | A | B |
| --- | --- | --- | --- |
| Tests | 1 | 1 | 13 |
| Security | 11 | 11 | 7 |
| Infrastructure | 4 | 7 | 8 |
| Documentation | 1 | 2 | 5 |
| Dependencies | 1 | 3 | 2 |
| Correctness | 10 | 11 | 3 |
| Performance | 2 | 2 | 0 |
| Accessibility | 0 | 1 | 0 |

Tools versus no tools, arms A and B: a handful of findings swapped in each direction, one
tool-dependent confirmed defect, which was already in the baseline scan.

Predictions, sealed before any run: (1) swarm wins every category, failed; (2) generalist misses a
critical the swarm catches, failed and the reverse happened; (3) biggest gap is documentation then
security, half, documentation was the swarm's second lead and security inverted; (4) swarm precision
higher, failed; (5) generalist does not notice the missing tests, failed six of six, and then three
of three for the naive arm.

## Interpretation

**Defect discovery did not favor the swarm.** A and B tied. C was eight behind, and the eight were
moderate and hygiene items. The swarm is not broadly better at finding bugs on this subject.

**Most of the capability was in the model, not the prompt.** The naive arm found every
merge-blocking defect every run, investigated security unasked, ran the build and linters, and wrote
a probe to prove a bcrypt truncation. What the engineered prompt added was breadth in hygiene lanes.
It did not add high-value detection, and it cost consistency.

**The swarm's real advantage is decomposition and the record.** The test reviewer turned "no tests"
into thirteen module-level items with assertions, every run. That is specialist depth and
actionability. It is not thirteen root defects; at root-cause level all three arms saw the same
absence. And every swarm run produced an inspection record nothing else produced: which reviewer
checked what, which tools ran or failed, which files were not read, where agents disagreed, what was
noticed but unowned. That is assurance evidence. It is not the same claim as a better review.

**Lane boundaries lost the most important finding.** The subject lets a user rewrite predictions
after the outcome is recorded, which falsifies the product's core metric. Both generalists found it.
A swarm agent noticed it in two runs. The merged report filed it under "Handoffs nobody picked up:
no agent in the roster owns this," and it never became a finding. The model saw it; the
architecture, by its own decision record (ADR 0004, no general reviewer), had nowhere to put it.
This is an architecture-induced coverage gap, not a model capability failure.

**The security specialist lost its lane, twice.** Eleven and eleven to seven, once with a prompt
that named security and once without. Cause not established. Candidates: a checklist-shaped charter,
security findings that need whole-system context, lane rules suppressing cross-domain reasoning, or
a subject that rewards broad reasoning. All testable, none tested.

**Tools were not really tested.** The subject had nothing to run them on.

**The subject was the kind V1 was least built for.** V1 is a pull-request gate for repositories with
engagement documents and decision records. Experiment 001 pointed it at a whole freshly generated
repository with neither. Not an excuse; every arm reviewed the same thing. An external-validity
limit.

## Architectural implications

- V1's assumption that review decomposes into independent lanes is incomplete. Something has to own
  what crosses lanes and what falls between them.
- "Hand it to nobody" cannot be a terminal state for a serious finding.
- Specialist decomposition and the inspection record are worth keeping and are the argument for
  keeping specialists at all.
- Findings and the assurance record are two products and should be produced as two.
- The V2 hypothesis: broad whole-system review plus specialists plus an arbiter. Not built, not
  tested. See [architecture/v2-hybrid-review.md](../architecture/v2-hybrid-review.md).

## Unresolved

- Why the security specialist underperformed two generalists.
- Whether a broad reviewer alongside specialists recovers cross-cutting findings without
  reintroducing the duplication ADR 0004 was written to avoid.
- Whether the naive arm's result holds across a population of naive prompts rather than one.
- What the test and infrastructure specialists do on a subject that has tests and infrastructure.
- The second judge. Registered, not run. All verdicts are one judge's and open for dispute.
- Whether the actionability and documentation-quality rubrics, registered and never applied, change
  the picture.

## What failed, kept as failed

Four of five predictions, including the one the author called the sharpest. The reading-only
condition existed because of a harness bug. The first baseline captured the wrong exit code. The
harness leaked a settings file into the subject and one arm reported it as a defect. An early
headline said "nearly twice" when the number was 40%. All recorded in the experiment repository, none
rewritten.
