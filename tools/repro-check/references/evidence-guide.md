# Evidence guide: where proof lives in a reproduction package

An eval bundle has five parts, always in this order: **Repo facts** (repo line,
latest release, bug-report template asks, `contribution policy`), **Issue** (title,
author, body with the reported error, versions, and steps), **Thread highlights**
(maintainer and commenter notes), **Candidate claim comment**, and **Candidate repro
report**. In live mode the same parts are: the repo's README, CONTRIBUTING.md, AI
policy file, and issue template (repo facts); the issue page and its comments
(issue and thread); and the student's draft files (claim and report).

## Environment

- Where it lives: eval, the environment lines at the top of the Candidate repro
  report (versions, OS, shell, install method), read against the versions and
  platform in the Issue body, the `latest release` line in Repo facts, and any
  maintainer note in Thread highlights that says a platform, driver, or build
  profile matters. Live, the draft report's environment block, read against the
  issue body, the repo's current release or `main` commit, and the thread.
- What good looks like: the report names the version or commit it tested and the
  OS, plus every setting the issue or a maintainer says changes the failure. Where
  any of these differs from what the issue reports or targets, the report says so
  in a sentence ("issue is on 1.4.2, I tested 1.5.0").

## Steps

- Where it lives: eval, the numbered steps, commands, and code blocks in the
  Candidate repro report, including any input file shown with `cat` or inline.
  Live, the draft report's steps and the files it references (which must be
  pasted into the draft, since a stranger cannot see the student's disk).
- What good looks like: a stranger with the report, the issue, and public
  software can go from a clean start to the trigger without guessing anything
  that matters to the failure. Commands are shown. An input can be pasted, named
  as exactly the issue's own input, or described precisely enough to recreate.
  Nothing the trigger depends on points at a private repo, an internal config,
  or "my setup".

## Behavior shown

- Where it lives: eval, the output excerpts, logs, exit codes, and described
  screenshots in the Candidate repro report, read against the error message,
  exit code, and symptom in the Issue body (and the trigger a maintainer names in
  Thread highlights). Also read the report's INPUT against the issue's input: the
  exact value, syntax, or file that triggers the bug. Live, the draft report's
  pasted output, read against the issue page.
- What good looks like: the artifact shows the issue's failure itself: the same
  exception or error text, the same exit code, the same kind of failure (a crash
  is not a clean error message; garbled output with a live process is not a
  crash). The input matches the issue's trigger character for character where it
  matters (`=` vs `:`, a range syntax, a bound vs an unbound variable). An
  equivalent way to feed the same input is fine. A control run with the trigger
  removed is a strong extra sign.

## Honesty

- Where it lives: the sentences in the Candidate claim comment and the Candidate
  repro report that state outcomes ("reproduced", "confirmed", "the cause is",
  "also happens on"), each read against the artifacts shown next to them.
- What good looks like: the central outcome (reproduced, or not) points at an
  artifact in the report, and any cause or wider scope is either backed by an
  artifact or labelled as a hypothesis ("I suspect", "my guess"). Supporting
  details, like how many times a run was repeated, can be stated. An honest
  cannot-reproduce shows the attempt's output and names what differed from the
  issue's setup; that is a complete, postable report. Certainty words with no
  artifact behind them ("guaranteed", "verified", "100%") are the warning sign.

## Comms

- Where it lives: the Candidate claim comment read against the Issue (does it name
  this issue's symptom, trigger, or component?), and both comments read against
  the `contribution policy` line in Repo facts. (The bug-report template asks are
  for opening issues, not for comments on one.) Live: the issue's thread, the
  repo's CONTRIBUTING.md and AI policy file, against the draft comments.
- What good looks like: the claim is specific to this issue and promises only the
  next step (investigate, reproduce, report back), never a fix or a date. If the
  repo's policy requires disclosing AI use, a comment says which tool was used
  and for what (every package here counts as AI-assisted work). If the policy asks
  for comments in the author's own words, the comments read as the author's, not
  as pasted AI output. A policy that says nothing about AI asks for nothing extra.
