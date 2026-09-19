# Contributing

Three kinds of contribution are useful right now, and each has a place.

## Disputing a verdict from Experiment 001

Every claim from every run was checked against the frozen subject and the verdict recorded with its
evidence. If you think one is wrong, that is the most valuable thing you can send.

1. Read the reviewer section of the findings README in the experiment repository:
   [findings/README.md, "For reviewers"](https://github.com/cruzbuilds/Five-Critics-or-One-Good-Prompt/blob/main/findings/README.md#for-reviewers-where-the-verdicts-actually-are).
2. Open the subject at the frozen commit: [idea-log](https://github.com/cruzbuilds/idea-log) at
   `b38c5b0`.
3. Open an issue on
   [Five-Critics-or-One-Good-Prompt](https://github.com/cruzbuilds/Five-Critics-or-One-Good-Prompt/issues)
   with one line per claim: claim ID, your verdict, one reason.

Every disagreement becomes a row in `findings/corrections.csv` there, with your handle as the person
who raised it, whether or not I agree. The count of corrections is reported.

## A cold review prompt

If you have not read Experiment 001's arm A prompt, write the one sentence you would give a reviewer
before shipping, and send it in the same issue. The naive arm in Experiment 001 is one prompt written
by someone who had seen the results. Prompts from people who had not are what would make it a
population.

## V2 design

Design discussion for the hybrid architecture belongs on
[agentic-review-swarm](https://github.com/cruzbuilds/agentic-review-swarm/issues), against the
proposed ADR that supersedes ADR 0004. The open questions are listed in
[architecture/v2-hybrid-review.md](architecture/v2-hybrid-review.md). Arguments that V2 will not work
are as welcome as arguments that it will; the point of the next experiment is to find out.

## What this repository does not take

Source changes to the review system go to `agentic-review-swarm`. Changes to Experiment 001's raw
data, reports, protocol or predictions are not accepted anywhere; that record is closed and
append-only, and corrections are the only write path. This repository takes edits to the narrative,
the architecture history, the summaries and the roadmap.

## Conventions

- Plain language. No claims of superiority without a measurement behind them and a limitation
  beside it.
- No employer or customer material.
- Commits by author name, no attribution trailers.
