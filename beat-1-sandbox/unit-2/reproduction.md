# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

danielhinker

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54#issuecomment-6048090920

I am picking this one up. I will reproduce the empty detected_sections result from _detect_sections() in ingestion/parsers/resume_parser.py when the resume header lines are indented, using the snippet in the issue description and the tests named in tests/unit/test_resume_parser.py. I will post a reproduction report here with my environment, steps, and output.

I am using Claude Code (AI) to help run the reproduction and draft my comments, and I review everything before posting.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54#issuecomment-6048151778

**Reproduced** on current `main`: with indented header lines, `detected_sections` comes back `[]`. The same text with the indentation removed finds both sections.

**Environment**
- Ubuntu 24.04.3 LTS on WSL2 (Windows), kernel 6.6.114.1-microsoft-standard-WSL2
- Python 3.11.14 (CI pins 3.11), uv 0.12.18, pypdf 6.19.0, pytest 9.1.1
- Repo at `f89c06f` (`main`, 2026-09-16)
- Setup: `uv venv --python 3.11 .venv && uv pip install -e ".[dev]"`. I skipped `.env`, Docker and the database, because the parser and its unit tests don't use them.

**Steps**

1. The issue's snippet, exactly as written, saved as `snippet.py` and run with `.venv/bin/python snippet.py`:

   ```python
   from ingestion.parsers.resume_parser import ResumeParser
   r = ResumeParser()
   res = r.parse('\n    John Smith\n    john@example.com\n\n    Education:\n    - B.S. Computer Science\n\n    Skills: Python\n')
   print(res.metadata['detected_sections'])
   ```

   Output (exit 0, nothing on stderr):

   ```
   []
   ```

2. Control: the same string with the indentation removed through `textwrap.dedent()`, nothing else changed:

   ```python
   from textwrap import dedent
   from ingestion.parsers.resume_parser import ResumeParser
   s = '\n    John Smith\n    john@example.com\n\n    Education:\n    - B.S. Computer Science\n\n    Skills: Python\n'
   r = ResumeParser()
   print(sorted(r.parse(s).metadata['detected_sections']))
   print(sorted(r.parse(dedent(s)).metadata['detected_sections']))
   ```

   ```
   []
   ['Education', 'Skills']
   ```

   (`_detect_sections()` returns `list(set(...))`, so I sorted the output. The unsorted order changes between runs.)

3. The three tests named in the issue are marked `xfail(strict=True)`, so a normal run only shows `3 xfailed`. With `--runxfail` the actual assertions show (pytest's output from the FAILURES section on, unedited):

   ```
   .venv/bin/python -m pytest tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections -v --runxfail --tb=short
   ```

   ```
   =================================== FAILURES ===================================
   ____________ TestResumeParser.test_parse_single_column_resume_text _____________
   tests/unit/test_resume_parser.py:35: in test_parse_single_column_resume_text
       assert any("experience" in s for s in detected_lower) or any(
   E   assert (False or False)
   E    +  where False = any(<generator object TestResumeParser.test_parse_single_column_resume_text.<locals>.<genexpr> at 0x70aa91be9080>)
   E    +  and   False = any(<generator object TestResumeParser.test_parse_single_column_resume_text.<locals>.<genexpr> at 0x70aa91be9150>)
   ____________ TestResumeParser.test_parse_resume_no_work_experience _____________
   tests/unit/test_resume_parser.py:61: in test_parse_resume_no_work_experience
       assert any("education" in s for s in detected_lower)
   E   assert False
   E    +  where False = any(<generator object TestResumeParser.test_parse_resume_no_work_experience.<locals>.<genexpr> at 0x70aa92bc3d30>)
   ____________________ TestResumeParser.test_detect_sections _____________________
   tests/unit/test_resume_parser.py:152: in test_detect_sections
       assert len(sections) > 0
   E   assert 0 > 0
   E    +  where 0 = len([])
   =========================== short test summary info ============================
   FAILED tests/unit/test_resume_parser.py::TestResumeParser::test_parse_single_column_resume_text
   FAILED tests/unit/test_resume_parser.py::TestResumeParser::test_parse_resume_no_work_experience
   FAILED tests/unit/test_resume_parser.py::TestResumeParser::test_detect_sections
   ============================== 3 failed in 0.33s ===============================
   ```

   The whole file gives `5 passed, 5 xfailed` normally and `5 failed, 5 passed` with `--runxfail`.

**Expected vs actual**
- Expected (from the issue): `Education` and `Skills` detected.
- Actual: `[]` whenever the header lines are indented. `test_detect_sections` calls `_detect_sections()` directly and gets `len([]) == 0`.

**Two more tests tagged #54 fail for a different reason.** In the whole-file `--runxfail` run, `test_parse_markdown_resume` and `test_strip_markdown_syntax` fail on `#` markers left in the stripped text, not on `detected_sections` (pytest output, trimmed to the assertion lines):

```
tests/unit/test_resume_parser.py:114: in test_parse_markdown_resume
    assert "#" not in result.text or result.text.count("#") < markdown_resume.count("#")
E   AssertionError: assert ('#' not in '# Jane Doe\..., PostgreSQL'
[...]
tests/unit/test_resume_parser.py:175: in test_strip_markdown_syntax
    assert not stripped.strip().startswith("#")
E   AssertionError: assert not True
```

My guess, from reading the code and not tested on its own: `_strip_markdown`'s `^#+\s+` pattern has the same leading-whitespace problem, so a fix limited to `_detect_sections()` might leave these two failing.

Next I'll work out where the fix belongs and post a plan here before changing any code.

I used Claude Code (AI) to set up the environment, run these commands, and draft this report; I reviewed the output before posting.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run, `--only pkg-20,pkg-09,calib-03 --include-calibration` (partial, first rubric):
   `agreement: 2/2 scored items`.
2. Run 1, full: `agreement: 14/20 scored items  (bar: 18/20: below the bar)`, with
   `clear-accept 2/8`. Every reject agreed; six clear accepts were held.
3. Revise run, `--only pkg-03,pkg-05,pkg-07,pkg-09,pkg-10,pkg-12,pkg-18,pkg-15,pkg-17,pkg-20`
   (partial, after revising `steps-rerunnable`, `claims-backed` and `policy-respected`):
   `agreement: 9/10 scored items`.
4. Run 2, full, the committed `eval-run.txt` (rubric and evidence guide unchanged since run 3):
   `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 7/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.

**Package analysis**

`pkg-05` (conda/conda#16543). My rubric decided `reject`; the gold label is `accept`. Every
other required check passed, and the grader said the artifact is the issue's failure:
"Output shows 'EnvironmentSectionNotValid ... - category' on stdout before the JSON". It
failed only `steps-rerunnable`, on the input: the report says it "wrote a minimal `env.yml`
containing a valid `dependencies:` list plus a `category:` section" and never pastes the file.
The grader read that as a guess a stranger would have to make: "env.yml is never pasted, only
'a minimal env.yml ... valid dependencies: list plus a category: section'; dependencies and the
category value are unspecified". My pass condition lets an input be "described precisely
enough to recreate (which keys, which values)". The report names the keys but not the values,
so the grader failed it. The gold label treats the values as irrelevant, because any
`category:` section triggers the warning. I agree with the gold label on the facts, and I kept
the check as it is (see Trade-offs).

**Check rationale**

> | steps-rerunnable | The commands, input files, and configuration in the Candidate repro report, read together with the Issue (a report may reuse the issue's own snippet or input by reference). | Pass if a stranger with this report, the issue, and public software could get from a clean start to the trigger without guessing anything that matters to the failure. Commands are shown; an input may be pasted, or named as exactly the issue's input ("the issue's 12-line test.txt"), or described precisely enough to recreate (which keys, which values). Exact keypresses in an interactive tool count as shown. Fail if a step that matters to the trigger is too vague to recreate ("set up the project", "my usual config"), or if it depends on something private or unshared (a private repo, an internal config, an unshared file). | required |

It started as the activity rubric's version, which passed only if "every command and every
input (file contents, config, flags) is shown in full". Run 1 failed five clear accepts on
that line because good reports reuse the issue's own input instead of pasting it again:
`pkg-03` says "created `test.txt` with the exact 12 lines from the issue", and `pkg-12` runs
"`repro.mjs` containing the issue's two `prettier.format` calls". A stranger holding the issue
can recreate both exactly, so I changed the test from "is every input pasted" to "could a
stranger reach the trigger without guessing anything that matters", and I let the issue count
as part of what the stranger holds. I kept the fail for private or unshared inputs, because
that is the case the check exists for (`pkg-18`, a repro that lives in a private monorepo).
The worksheet swap pushed the same way: Carlos's "where unclear" note said his check "doesnt
specify whether commands must be identical or functionally equivalent", and my answer lives
in `behavior-matches-issue`: "An equivalent command is fine (a file instead of a pipe), but
the input that triggers the bug must be the issue's, not a changed one."

**Trade-offs**

Loosening `steps-rerunnable` and `claims-backed` could let a weak report through, so the
revise run added four canaries that already agreed: `pkg-18` (private monorepo), `pkg-15`
(root cause asserted with nothing shown), `pkg-17` (scope generalized beyond the artifact) and
`pkg-20` (the one disclosure package, since I also narrowed `policy-respected`). All four
still came back `reject` in that `--only` run and again in the committed full run. The case I
accept the check will miss is `pkg-05`: a report that describes a file by its keys without
the values still fails, even when the values don't matter. Loosening further, to "values may
be left out", would also pass a report that hides the one value that triggers the bug, and
I'd rather hold a borderline good report than post a bad one.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
