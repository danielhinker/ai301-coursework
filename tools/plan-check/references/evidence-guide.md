# Evidence guide: where evidence lives in a plan package

An eval bundle has six parts, in this order: **Repo facts** (repo line, bug-report template
asks, `contribution policy` with any AI-use rule), **Issue** (title, author, body with the
reported failure), **Thread highlights** (dated comments, each with the author's role:
OWNER, MEMBER, COLLABORATOR, or NONE), **Repro evidence** (the accepted reproduction the
plan builds on, with its steps, artifacts, and controls), **Candidate plan**, and
**Candidate plan comment**. In live mode: the repo's CONTRIBUTING.md and AI policy file
(repo facts), the issue page and its comments (issue and thread), the student's posted
repro comment on that issue or the repro evidence quoted in the drafts (repro evidence),
and the student's `plan.md` and draft comment file (candidate side).

## Diagnosis and grounding

- Where it lives: the cause sentence in the Candidate plan (often labelled "Diagnosis" or
  "the bug is"), read against the Repro evidence's steps, artifacts, and especially its
  controls (runs with one thing changed), and against maintainer findings in Thread
  highlights. Live: `plan.md`'s diagnosis against the posted repro comment and the thread.
- What good looks like: the stated cause explains the failing run AND every control. If a
  control shows the suspected component working (same input without the flag, the same
  module printing correctly in the same build, an earlier stage already wrong before the
  blamed step runs), a diagnosis that blames that component is contradicted, no matter how
  confident or detailed the plan is.

## Scope

- Where it lives: the Candidate plan's in-scope and not-in-scope statements, its file
  list, and the steps of its approach. Read them against the Issue body's ask.
- What good looks like: one change aimed at the issue's bug. Same-mistake sibling sites,
  a regression test, and "deferring X, here's why" are inside a bounded plan. A plan that
  also upgrades dependencies, restructures modules, adds options or UI, or rewrites a
  subsystem has crept, even when the real fix is in there too.

## Executability

- Where it lives: the Candidate plan's named files, functions, or areas, its chosen
  approach, and its order of work.
- What good looks like: a stranger could open the named file and start the first step
  without asking which file, which layer, or which of two approaches. Open questions are
  fine when they're about risk; they're not fine when they're about what to build.

## Test plan

- Where it lives: the Candidate plan's test plan, read against the Repro evidence's steps
  and observed artifacts.
- What good looks like: the plan says what will be run and what output, exit code, or
  assertion will show the bug is gone (usually the repro re-run with the expected-after
  stated, or a regression test pinned to the repro's input). "Run the full suite" or "make
  sure nothing breaks" proves nothing about this bug on its own.

## Honesty

- Where it lives: the Candidate plan's risks or unknowns, and any certainty words in the
  plan and comment ("definitely", "the fix is", "guaranteed"), read against what the
  repro evidence actually pins down. Deviations found during the build go in `plan.md`'s
  Deviations section.
- What good looks like: what the evidence shows is stated as fact; what it doesn't is
  stated as a risk or a question. A deferral says what is deferred and why.

## Comms

- Where it lives: the Candidate plan comment read against maintainer comments in Thread
  highlights (named culprits, patches to test, chosen approaches, open PRs), and against
  the `contribution policy` line in Repo facts. Live: the issue thread and the repo's
  CONTRIBUTING.md and AI policy file, against the draft comment.
- What good looks like: when a maintainer has already pointed somewhere, the comment says
  whether the plan follows that and why. When the policy requires disclosing AI use, the
  comment says which tool was used and for what (every package here is AI-assisted work).
  When the policy asks for comments in the author's own words, the comment reads as the
  author's. A policy silent on AI asks for nothing extra.
