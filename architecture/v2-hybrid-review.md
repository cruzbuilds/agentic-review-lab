# V2: hybrid review

**Status: hypothesis. Not built, not measured. This document describes what V2 is expected to be
and why, and lists what would have to be true for it to be worth keeping. Nothing here is a
result.**

## The hypothesis

V1 assumed that comprehensive review decomposes into independent specialist lanes run in parallel.
Experiment 001 suggests that assumption is incomplete rather than wrong: specialization produced
decomposition, repeatability, explicit ownership and an inspection record, and it also produced
cross-domain blind spots, unowned correctness findings, and weaker security coverage than a broad
reviewer on that subject.

So the V2 hypothesis is:

> Broad whole-system reasoning and specialist depth are complementary rather than mutually
> exclusive. A review architecture that has both, with an arbiter that keeps every serious finding
> visible regardless of lane, should recover the cross-cutting findings V1 lost while preserving
> what V1 did well.

The failure mode it is designed against is specific and was demonstrated: an agent noticed the most
consequential defect in the subject, and the architecture had no route from "noticed" to "finding."

## The shape

```
                 +----------------------+
                 |  Broad systems review |   one reviewer, whole repository, no lane
                 +----------+-----------+
                            |
   +-----------+  +---------+---------+  +-----------+  +-----------+  +-----------+
   | security  |  |      tests        |  |   infra   |  |   docs    |  |   scope   |   specialists, charters unchanged in principle
   +-----+-----+  +---------+---------+  +-----+-----+  +-----+-----+  +-----+-----+
         |                  |                  |              |              |
         +------------------+---------+--------+--------------+--------------+
                                      |
                              +-------+-------+
                              |    Arbiter    |   synthesis, not a new review
                              +-------+-------+
                                      |
                       +--------------+--------------+
                       |                             |
                +------+------+             +--------+---------+
                |  Findings   |             | Assurance record |
                | what to fix |             | what was checked |
                +-------------+             +------------------+
```

### The broad systems reviewer

One reviewer whose job is to reason about the application as a system. Its mandate: correctness,
business invariants, state transitions, cross-file interactions, the places where security, data,
state and business logic meet, performance that emerges from architecture, unexpected failures, and
anything that could materially affect production behavior but belongs to no specialist.

It is allowed to wander. It does not get the specialists' "stay in your lane" rule. It is discouraged
from formatting, naming and style opinions, lint-level issues, hygiene the specialists already cover,
praise, and speculative rewrites. It prioritizes meaningful failure over exhaustive coverage.

It is not a "general code reviewer" in the sense ADR 0004 rejected. That agent was imagined as one
that comments on everything and overlaps everyone. This one has a mandate that the specialists
structurally cannot fill, and Experiment 001 is the evidence that the mandate is real: eleven and
ten confirmed correctness findings from generalists against three from the swarm.

### The specialists

Security, tests, infrastructure, documentation, scope, preserved. Their charters are not rewritten to
make Experiment 001's results look better; V2 needs to remain comparable to V1. The security charter
gets a first-principles review, separately documented, because the specialist lost its lane to two
generalists and the reason is not established. Adding the findings it missed to its checklist would
be tuning to one experiment and is not the plan.

### The arbiter

V1's orchestrator merges independent reports. V2's arbiter synthesizes. It collects the broad review
and the specialist reports, deduplicates overlap, preserves specialist attribution, preserves broad
findings that fall between lanes, identifies disagreements and keeps them visible, ensures no
important unowned finding disappears, produces one final verdict, and produces the assurance record.
It does not perform a new review from scratch. Its intelligence goes to synthesis and conflict
resolution.

### The rule that changes

V1 permits "no owner in the roster" and reports it under handoffs, but such a finding has no route
into the BLOCK/WARN logic. V2 gives it one. An **unowned finding** remains visible, keeps its
original observer, receives a severity and a confidence, is eligible to affect the final verdict,
and does not require inventing a specialist to own it. No serious observation disappears because the
taxonomy was incomplete.

### Two outputs, not one

Experiment 001 suggests that "what needs fixing" and "what was checked, by whom, with what, and what
could not be" are two different products that V1 conflated in one report. V2 separates them:
**findings**, and the **assurance record**. Both keep V1's deterministic structure, file and line
evidence, BLOCK/WARN/PASS, tools attempted and unavailable, attribution, handoffs and limitations.

## Execution model

Not decided. The simplest architecture that addresses the demonstrated failure mode is preferred,
and "multi-agent means everything in parallel" is not assumed. Two candidates:

1. Broad reviewer and specialists inspect independently and in parallel; the arbiter runs after all
   of them. Simplest. Keeps V1's specialist runs comparable.
2. Broad review first; its output informs which specialists run or where they look. Possibly
   cheaper, possibly sharper, definitely harder to compare with V1 and harder to seed.

The domains are known and fixed, so fixed specialist workers remain appropriate. Dynamic agent
spawning is not planned; there is no demonstrated reason for it.

## Expected benefits, none of them shown

- Cross-cutting correctness findings become findings instead of handoffs.
- Security coverage at least matches a broad reviewer, because a broad reviewer is now in the room.
- Specialist decomposition, repeatability and lane hygiene are preserved because the specialists are
  unchanged.
- The assurance record becomes a first-class output rather than a section.
- Disagreements between the broad reviewer and a specialist become a signal rather than noise.

Each of these is a prediction to be sealed before Experiment 002, not a property of V2.

## Open questions

- Does the broad reviewer duplicate the specialists enough to make the arbiter's deduplication the
  hard part? V1's reason for ADR 0004 was exactly this, and it was a reasonable worry.
- How does the arbiter assign severity to an unowned finding without becoming a second full review?
- Does a broad reviewer's presence change what the specialists find, if they run in parallel and
  cannot see it? It should not. That is testable.
- Is the security specialist's underperformance a charter problem, a lane problem, or a subject
  problem? V2 does not answer this; it makes the question measurable.
- Cost. V1 was already five model calls plus a merge. V2 is at least seven. Whether the seventh and
  the synthesis pay for themselves is an output of Experiment 002.
- Whether the arbiter should ever downgrade a specialist BLOCK. The current position is no: a
  specialist BLOCK survives, a broad-only BLOCK survives, and the arbiter's job is to keep both
  visible, not to overrule either.

## What must eventually be tested

Experiment 002, not yet designed past a paragraph. A subject with existing automated tests, CI/CD,
infrastructure and deployment configuration, documentation and meaningful business logic, so that
the specialists review presence rather than absence and the tools have something to run. Four arms:
naive generalist, strong generalist, V1, V2. Predictions sealed first. Cold naive prompts from
people who have not read Experiment 001. The actionability and documentation-quality rubrics applied
to every finding. A second judge on a different model. Cost and report length recorded from the
harness for every arm.

Before that: build V2, use it on real development work, and collect its failures the same way V1's
were collected. Compare V1 and V2 formally only when there is enough reason to spend the compute.

## Evals the new parts need

The broad reviewer gets its own seeds, not copies of specialist seeds: state that can change after
an event that should freeze it; two individually correct modules that together violate a business
invariant; authorization correct locally but bypassed by a workflow; data written in one path
consumed incorrectly in another; a race or state transition that corrupts a core metric; and a clean
repository with none of these. The Experiment 001 calibration bug is one case, not the set.

The arbiter gets tests: a duplicate broad and specialist finding collapses to one with both
attributions; a specialist-only BLOCK survives; a broad-only BLOCK survives; an unowned severe
finding affects the verdict; a disagreement stays visible; one reviewer failing does not read as full
coverage; an all-clean input can still PASS.

## Decision history

ADR 0004 said there would be no general-purpose reviewer, and gave good reasons. V2 supersedes it
with a new decision record that keeps ADR 0004 in place and explains what V1 believed, what
Experiment 001 observed, why the assumption changed, why V2 adds broad whole-system review, and why
that does not remove the specialists. The proposed ADR lives in the implementation repository when
V2 work begins.
