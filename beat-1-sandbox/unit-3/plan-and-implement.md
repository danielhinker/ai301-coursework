# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

danielhinker

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54#issuecomment-6048688905

Plan for #54, built from my repro report above (`main` at `f89c06f`):

**Cause.** This is the cause the issue body describes, and my repro confirms it. All four header patterns in `_detect_sections()` require the section name at the very start of a line (`^education...`, `\neducation...`), so `    Education:` never matches. My dedent control (`[]` indented vs `['Education', 'Skills']` dedented) and `test_detect_sections` calling the function directly (`assert 0 > 0` / `where 0 = len([])`) both point at this function, not at the Markdown stripping that runs before it.

**Change.** Allow optional leading spaces or tabs (`[ \t]*`) after each anchor in those four patterns. `_strip_markdown()`'s header pattern `^#+\s+` has the same column-0 assumption, which is why `test_parse_markdown_resume` and `test_strip_markdown_syntax` (also tagged #54) fail, so I'll apply the same `[ \t]*` there. Then remove the five `xfail(strict=True, reason="issue #54: ...")` markers, since those tests should pass with the fix. Files: `ingestion/parsers/resume_parser.py` and `tests/unit/test_resume_parser.py`.

**Not in scope.** New section names, the PDF extraction path, the other Markdown-stripping rules, and any parser refactor.

**Test plan.** Re-run my repro: the issue's snippet should print `Education` and `Skills` (any order) instead of `[]`, and the dedent control should give the same result both ways. The five #54 tests should pass without their markers, and `pytest tests/unit` should show nothing else changing.

**Open question.** I'm reading the shared `issue #54` reason on the two Markdown tests as meaning they belong in this fix. If they were meant for a separate change, I'll leave `_strip_markdown()` alone and keep those two markers.

I used Claude Code (AI) to help draft this plan; I've checked it against my reproduction.

---

## Your branch

**Branch**

fix/54-indented-section-headers

**Evidence**

Base: `main` at `f89c06f`. After: branch `fix/54-indented-section-headers`, commit `6980a45`.
Commands run from the repo root of my clone, Python 3.11.14 on Ubuntu 24.04 (WSL2).

**Before (`main`)**

1. The issue's snippet (my unit 2 repro step 1):
```
$ cd pathreview-ai301-fa26-s1 && .venv/bin/python ../repro-54/snippet.py
[]
exit=0
```

2. The dedent control (my unit 2 repro step 2):
```
$ cd pathreview-ai301-fa26-s1 && .venv/bin/python ../repro-54/control.py
input (indented) repr: '\n    John Smith\n    john@example.com\n\n    Education:\n    - B.S. Computer Science\n\n    Skills: Python\n'
input (dedented) repr: '\nJohn Smith\njohn@example.com\n\nEducation:\n- B.S. Computer Science\n\nSkills: Python\n'
indented  detected_sections: [] | sorted: []
dedented  detected_sections: ['Education', 'Skills'] | sorted: ['Education', 'Skills']
exit=0
```

3. The five #54 tests, with `--runxfail` so the strict xfail markers don't hide the failures:
```
$ cd pathreview-ai301-fa26-s1 && .venv/bin/python -m pytest tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience tests/unit/test_resume_parser.py::TestResumeParser::test_parse_markdown_resume tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections tests/unit/test_resume_parser.py::TestResumeParser::test_strip_markdown_syntax --runxfail -v --tb=short --color=no
[... pytest session header trimmed ...]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text FAILED [ 20%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience FAILED [ 40%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_markdown_resume FAILED [ 60%]
tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections FAILED [ 80%]
tests/unit/test_resume_parser.py::TestResumeParser::test_strip_markdown_syntax FAILED [100%]
[... failure tracebacks trimmed; the assertion lines are in my unit 2 repro report ...]
============================== 5 failed in 0.32s ===============================
```

4. The whole resume parser test file:
```
$ cd pathreview-ai301-fa26-s1 && .venv/bin/python -m pytest tests/unit/test_resume_parser.py -v --color=no
[... pytest session header trimmed ...]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text XFAIL [ 10%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience XFAIL [ 20%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_multipage_pdf PASSED [ 30%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_markdown_resume XFAIL [ 40%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_invalid_content_type PASSED [ 50%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_invalid_list_content PASSED [ 60%]
tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections XFAIL [ 70%]
tests/unit/test_resume_parser.py::TestResumeParser::test_strip_markdown_syntax XFAIL [ 80%]
tests/unit/test_resume_parser.py::TestResumeParser::test_pdf_parsing_error_handling PASSED [ 90%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_preserves_text_content PASSED [100%]
========================= 5 passed, 5 xfailed in 0.36s =========================
```

**After (`fix/54-indented-section-headers`)**

1. The issue's snippet:
```
$ cd pathreview-ai301-fa26-s1 && .venv/bin/python ../repro-54/snippet.py
['Education', 'Skills']
exit=0
```

2. The dedent control:
```
$ cd pathreview-ai301-fa26-s1 && .venv/bin/python ../repro-54/control.py
input (indented) repr: '\n    John Smith\n    john@example.com\n\n    Education:\n    - B.S. Computer Science\n\n    Skills: Python\n'
input (dedented) repr: '\nJohn Smith\njohn@example.com\n\nEducation:\n- B.S. Computer Science\n\nSkills: Python\n'
indented  detected_sections: ['Education', 'Skills'] | sorted: ['Education', 'Skills']
dedented  detected_sections: ['Education', 'Skills'] | sorted: ['Education', 'Skills']
exit=0
```

3. The five #54 tests, markers removed, no flags:
```
$ cd pathreview-ai301-fa26-s1 && .venv/bin/python -m pytest tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience tests/unit/test_resume_parser.py::TestResumeParser::test_parse_markdown_resume tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections tests/unit/test_resume_parser.py::TestResumeParser::test_strip_markdown_syntax -v --tb=short --color=no
[... pytest session header trimmed ...]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text PASSED [ 20%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience PASSED [ 40%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_markdown_resume PASSED [ 60%]
tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections PASSED [ 80%]
tests/unit/test_resume_parser.py::TestResumeParser::test_strip_markdown_syntax PASSED [100%]
============================== 5 passed in 0.38s ===============================
```

4. The whole resume parser test file:
```
$ cd pathreview-ai301-fa26-s1 && .venv/bin/python -m pytest tests/unit/test_resume_parser.py -v --color=no
[... pytest session header trimmed ...]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text PASSED [ 10%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience PASSED [ 20%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_multipage_pdf PASSED [ 30%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_markdown_resume PASSED [ 40%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_invalid_content_type PASSED [ 50%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_invalid_list_content PASSED [ 60%]
tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections PASSED [ 70%]
tests/unit/test_resume_parser.py::TestResumeParser::test_strip_markdown_syntax PASSED [ 80%]
tests/unit/test_resume_parser.py::TestResumeParser::test_pdf_parsing_error_handling PASSED [ 90%]
tests/unit/test_resume_parser.py::TestResumeParser::test_parse_preserves_text_content PASSED [100%]
============================== 10 passed in 0.38s ==============================
```

Whole unit suite (`pytest tests/unit -q`): before `375 passed, 53 xfailed`, after `380 passed,
48 xfailed`. A per-test comparison shows exactly these five tests moving from XFAIL to PASSED,
and nothing else changing.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Run 1, full, first version of the rubric, evidence guide and procedure:
   `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.
   It was not saved with `--save-run`.
2. Run 2, full, the committed `eval-run.txt`, with nothing changed since run 1 (same file
   fingerprints): `agreement: 20/20 scored items  (bar: 18/20: PASS)`.

**Package analysis**

`pkg-15` (the WebDAV sync timeout). My rubric decided `reject`; the gold label is `reject`.
It's the arguable one, because the core fix is right. Every check about the fix passed: the
diagnosis matches every control ("curl works, Node 26 at 500 ms works 10/10, and forcing
250 ms fails 8/10"), and the test plan names an expected-after ("sync succeeds 10 of 10 runs
after the change"). It failed only `one-bounded-change`. Item 1 is the fix the issue needs:
"Set the default `autoSelectFamilyAttemptTimeout` to 500 ms at app startup to match current
Node (the direct fix)". Items 2-5 are not: "replace `node-fetch` with `undici` across the
desktop sync code", "Add a Settings > Synchronisation panel field exposing the timeout",
"Fix the silent-success UI while in the area", and "Add a retry-with-backoff wrapper". My
pass condition fails a plan that takes on "a migration or dependency upgrade" or a "while
I'm in there" item "even when the core fix inside it is right", which is exactly this plan.

**Check rationale**

> | diagnosis-grounded | The cause the Candidate plan states, read against every step and control in the Repro evidence block, and against maintainer findings in Thread highlights. | Pass if the stated cause explains the failure the repro shows AND is consistent with every control and observation in the repro evidence (a control that shows the suspected code working rules that code out). Fail if any repro step or control contradicts the stated cause, if the plan blames something the evidence never touches while ignoring what the evidence does point at, or if the plan states no cause at all. | required |

The parenthetical, "a control that shows the suspected code working rules that code out", is
the part I chose deliberately. The wrong-cause plans in the set are long and confident, and
their diagnosis sounds right until you hold it against one control in the repro evidence.
`pkg-16` blames the post-read cast, but the repro's step 4 shows the zeros are already gone
before any cast runs. A first version that only asked "does the diagnosis explain the
failure" would pass those plans, because a wrong cause can explain the failing run
perfectly. It has to fail on a control it can't explain. I rejected grading the
diagnosis's detail or confidence: that rewards exactly the polished wrong-cause plans.

**Trade-offs**

Nothing changed between my two runs. Run 2 graded the same rubric, evidence guide and
procedure as run 1 (the fingerprints in `eval-run.txt` match the files I uploaded), and
every package got the same verdict both times. What `diagnosis-grounded` gives up: a plan
whose cause is right but which the repro's controls never test either way still passes,
because nothing contradicts it. I accept that miss. The check can only catch a diagnosis
the evidence rules out, not one the evidence is silent about.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
