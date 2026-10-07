# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54

**Verdict output**

Live-mode output for my three candidates (#54, #68, #60), pasted verbatim. #54 is the issue I chose, and its verdict is `accept`.

````
I graded all three candidates against the rubric. #54 and #68 are accepted, with #54 ranked first. #60 is rejected because an open PR already targets it.

**Ranked read-out**

1. **#54, resume section detection fails on leading whitespace: accept.**
   - It passes all six required checks. Unassigned, no claim comments and no linked PR, and it is a bug report.
   - Both preferred checks pass, and it has the most concrete reproduction of the three: a runnable snippet plus three named failing tests in `tests/unit/test_resume_parser.py`.
   - Fit: it is a bounded Python bug that fails on plain pytest with no services, keys or Docker. A regression test comes with the fix.
   - Each of the three failing tests would confirm the fix.
2. **#68, `KeywordSearcher.index([])` raises `ZeroDivisionError`: accept.**
   - It passes all six required checks. Both preferred checks pass, and it names the files and an xfail test to un-mark.
   - One classmate (`yulijasso`) posted a claim on 2026-09-20. Under the Path Review house rule, claim comments don't block, so `unclaimed` passes on assignees (none) and open PRs (none).
   - It ranks second because it is contested, and the claimant has already posted a plan and a full reproduction.
   - It is still a good Python-only fit.
3. **#60, faithfulness checker crashes on `text: None`: reject.**
   - `unclaimed` fails because PR #74 is linked, state OPEN, and its body says "Closes #60". The house rule only waives claim comments, not open PRs.

**Evidence shared by all three**
- **repo-in-use:** passes. The repo isn't archived and its last push was 2026-09-16.
- **maintainer-active:** passes. The last five commits on `main` are by `Aburke225`, a person, with three dated 2026-09-16.
- **ai-policy-allows:** passes. `docs/CONTRIBUTING.md` and the PR template contain no AI-related language. A grep found only "agent tool" references. There is no root `CONTRIBUTING.md`, and I did not look for AI policy files elsewhere.
- **settled-and-endorsed:** passes for all three. Each is a bug with fewer than 2 unmerged closed PRs.
- **single-bounded-task:** passes for all three. Each asks for one fix.
- **maintainer-signal:** passes for all three. Each has the `good first issue` label. I confirmed the author association (COLLABORATOR) for #60 only. All three issues have the same author.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54",
    "checks": [
      {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived=false; pushedAt 2026-09-16 (7 days before 2026-09-23)"},
      {"name": "maintainer-active", "grade": "pass", "evidence": "main head f89c06f by Aburke225 (person) on 2026-09-16"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees []; no comments; timeline has only label events, no linked or cross-referenced PR"},
      {"name": "single-bounded-task", "grade": "pass", "evidence": "one bug: _detect_sections anchors headers at line start, fails on leading whitespace"},
      {"name": "settled-and-endorsed", "grade": "pass", "evidence": "bug report; 0 linked PRs"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template contain no AI ban or AI language"},
      {"name": "maintainer-signal", "grade": "pass", "evidence": "labels include good first issue and tier-1; same author as #60 (COLLABORATOR)"},
      {"name": "has-repro-or-spec", "grade": "pass", "evidence": "runnable snippet, observed [] vs expected Education/Skills, three named failing tests"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68",
    "checks": [
      {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived=false; pushedAt 2026-09-16"},
      {"name": "maintainer-active", "grade": "pass", "evidence": "main head f89c06f by Aburke225 (person) on 2026-09-16"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees []; no linked or open PR (only PR in repo is #74, for #60); yulijasso claim comment of 2026-09-20 ignored per Path Review house rule"},
      {"name": "single-bounded-task", "grade": "pass", "evidence": "one bug: index([]) should not raise; fix in rag/retriever/keyword_search.py plus xfail removal"},
      {"name": "settled-and-endorsed", "grade": "pass", "evidence": "bug report; 0 linked PRs"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template contain no AI ban or AI language"},
      {"name": "maintainer-signal", "grade": "pass", "evidence": "labels include good first issue and tier-1"},
      {"name": "has-repro-or-spec", "grade": "pass", "evidence": "states index([]) raises ZeroDivisionError and should return, as search() does; covering xfail test named"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/60",
    "checks": [
      {"name": "repo-in-use", "grade": "pass", "evidence": "isArchived=false; pushedAt 2026-09-16"},
      {"name": "maintainer-active", "grade": "pass", "evidence": "main head f89c06f by Aburke225 (person) on 2026-09-16"},
      {"name": "unclaimed", "grade": "fail", "evidence": "PR #74 (nianiiier, 'Closes #60') is linked, state OPEN, created 2026-09-21; house rule waives only claim comments, not open PRs"},
      {"name": "single-bounded-task", "grade": "pass", "evidence": "one bug: check() crashes when chunk text is None"},
      {"name": "settled-and-endorsed", "grade": "pass", "evidence": "bug report; 0 closed-unmerged linked PRs"},
      {"name": "ai-policy-allows", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template contain no AI ban or AI language"},
      {"name": "maintainer-signal", "grade": "pass", "evidence": "author_association COLLABORATOR; good first issue label"},
      {"name": "has-repro-or-spec", "grade": "pass", "evidence": "runnable snippet with observed TypeError; failing test test_none_context_chunk_text named"}
    ],
    "verdict": "reject"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. Run 1 (full run, before saving): `agreement: 20/20 scored items  (bar: 18/20: PASS)`,
   with `categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4`.
2. Run 2 (the committed `eval-run.txt`, same rubric, nothing changed in between):
   `agreement: 20/20 scored items  (bar: 18/20: PASS)`.

**Issue analysis**

`issue-12` (bookwyrm-social/bookwyrm#1133). My rubric decided `reject`; the gold label is
`reject`. Every liveness, scope, and claim check passed on the bundle's own facts:
"archived: no", "last push to any branch: 2026-08-12", "2026-08-12 by Mouse Reeve: Merge
pull request #4080 from PatrickChildersIT/cleaner-javascript", "this issue: assignees: none;
linked PRs: none", and a maintainer (mouse-reeve, MEMBER) settling the approach: "You could
definitely create one with html and css (we use the bulma css library)". The only failing
check was `ai-policy-allows`,
on the policy line: "We do not accept AI-generated code or documentation." That is an
outright ban rather than a condition, and my contribution workflow in this course is
AI-assisted, so the issue is a dead end before a maintainer reads any code. It is the one
issue in the `policy` category, so without this check the rubric would have accepted it.

**Check rationale**

> | ai-policy-allows | Repo facts `contribution policy` line. Live mode: CONTRIBUTING.md, any AI policy file, and the PR template. | Fail only on an outright ban on AI-generated contributions (for example, "We do not accept AI-generated code"). Pass when the policy is silent, welcomes AI tools, ships an AGENTS.md, or sets conditions (disclose it, understand and test it, human review before submitting). | required |

It fails only on an outright ban because conditions are terms I can follow, not reasons to
walk away. The eval set shows both kinds: zulip says "contributors must personally
understand, test, and be able to explain every change", and tldr says PRs "made wholly or
partly with generative AI or machine translation without human review are closed". Both
are conditions I can meet. bookwyrm's "We do not accept AI-generated code or documentation"
is the only ban. I made the check `required` because a ban makes the issue unworkable for my
workflow no matter how good it looks otherwise.

**Trade-offs**

It passes repos that discourage AI without banning it. tldr (`issue-10`) "strongly
discourages generative AI for new pages", and my check still passes that policy; the issue
is rejected on other checks instead (it fails `unclaimed` and `single-bounded-task`; the
"Documentation request megaissue" is not one bounded task). A case I accept it will miss: a
maintainer who rejects AI-assisted work in practice but has never written a ban. Nothing
changed in the rubric between my two runs, and the committed run gives the same verdict as
run 1 on all 20 issues, so no verdict moved because of this check. The only per-check
difference between the runs is on a preferred check: `issue-04` failed `has-repro-or-spec`
in run 2 and not in run 1, which cannot change a verdict.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. Fit and time:
Issue 54 fits my background in Python and pytest. The bug is contained in resume parser and needs no database, Docker, or external API keys. Because three failing tests are already marked in the repo, reproducing and testing the fix this week is straightforward. I want to learn C++ for future projects, but sticking to Python keeps this first issue focused.

2. What the verdict got right, and what I weighed that the rubric could not:
The rubric correctly rejected issue 60 because a classmate already opened a pull request for it, avoiding duplicate work. It also prioritized 54 over 68 because 68 was already being handled by another student. What the rubric cannot weigh is that regex parsing in Python is low risk and isolated to a single file without side effects across the rest of the project.

3. Anticipated difficulty in claiming it:
Claiming it should be simple. Other students left comments, but under CodePath rules comments do not reserve an issue and only open pull requests block a claim. Because no pull request exists, the issue is open. My claim comment will outline the bug and state that I am working on a reproduction report without promising a fix date.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
