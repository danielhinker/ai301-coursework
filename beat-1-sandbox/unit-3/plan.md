# Plan: #54, resume section detection fails on leading whitespace

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/54
Built from: my reproduction on `main` at `f89c06f` (Ubuntu 24.04 on WSL2, Python 3.11.14).

## What the repro shows

- The issue's snippet, run unchanged, prints `[]` for `detected_sections`. The issue expects
  Education and Skills.
- Control: the same string through `textwrap.dedent()`, nothing else changed:
  ```
  []
  ['Education', 'Skills']
  ```
  So the only thing that flips the result is the leading indentation.
- `test_detect_sections` calls `_detect_sections()` directly on indented text and fails with
  `assert 0 > 0` / `where 0 = len([])`. The function misses indented headers on its own, so
  the bug is inside it, not in the Markdown stripping that runs before it.
- Two more tests tagged #54 fail on the same leading-whitespace problem in a different
  function: `test_parse_markdown_resume` (`assert ('#' not in '# Jane Doe\...`) and
  `test_strip_markdown_syntax` (`assert not True` on `startswith("#")`). Their input has
  indented `#` headers, and the `#` survives stripping.

## Diagnosis

`_detect_sections()` in `ingestion/parsers/resume_parser.py` builds four patterns per section
name: `^{section}\s*$`, `^{section}\s*[:|-]`, `\n{section}\s*$`, `\n{section}\s*[:|-]`. Each
requires the section name to be the first character of a line. An indented header
(`    Education:`) has spaces between the line start and the name, so no pattern matches and
the section is skipped.

`_strip_markdown()` has the same shape of bug in its header line:
`re.sub(r"^#+\s+", "", content, flags=re.MULTILINE)` only strips a `#` at column 0, so indented
Markdown headers keep their `#`.

## Scope

- **In:** allow optional leading spaces or tabs before the section name in the four
  `_detect_sections()` patterns, and before the `#` in `_strip_markdown()`'s header pattern.
  Remove the five `xfail(strict=True, reason="issue #54: …")` markers in
  `tests/unit/test_resume_parser.py`, since those tests should pass once this lands (and strict
  xfail turns an unexpected pass into a failure).
- **Not in:** new section names, changes to the PDF extraction path, the other
  `_strip_markdown()` rules (links, bold, code blocks), deduplication or ordering of the
  returned list, and any refactor of the parser.

## Files

- `ingestion/parsers/resume_parser.py`: the four patterns in `_detect_sections()`, and the
  header `re.sub` in `_strip_markdown()`.
- `tests/unit/test_resume_parser.py`: remove the five #54 `xfail` markers. No other test
  changes.

## Approach

1. Branch `fix/54-indented-section-headers` from `main` on my fork.
2. In `_detect_sections()`, insert `[ \t]*` after each anchor: `^[ \t]*{section}…` and
   `\n[ \t]*{section}…`. I'm using `[ \t]*` and not `\s*`, because `\s` also matches
   newlines and could let a header match across a blank line.
3. In `_strip_markdown()`, change the header pattern to `^[ \t]*#+\s+`.
4. Remove the five #54 `xfail` markers.
5. Run the test plan below. Keep this plan out of the commits.

## Test plan

My unit 2 repro steps, re-run against the change:

1. The issue's snippet. Expected after: `Education` and `Skills` (in any order; the function
   returns `list(set(...))`). Before: `[]`.
2. The dedent control. Expected after: both lines print the same two sections, Education
   and Skills (the control prints them sorted). Before: `[]` then `['Education', 'Skills']`.
3. The five #54 tests, run normally with the markers removed. Expected after: 5 passed.
   Before (with `--runxfail`): 5 failed.
4. `tests/unit/test_resume_parser.py`: expected after, 10 passed. Then the whole unit suite
   (`pytest tests/unit`) to check nothing else moved; expected after, the same result as on
   `main` plus these five.

## Risks and unknowns

- An indented line that starts with a section word in body text (for example
  `  Experience: 5 years with Python` inside a summary) would now count as a section header.
  The same line at column 0 already counts today, so this widens an existing behavior rather
  than adding a new kind. I haven't measured how often it happens on real resumes.
- I haven't checked whether other parsers (`readme_parser.py`) have the same anchored-pattern
  bug. If they do, that's a separate issue and not part of this change.
- I'm assuming the five xfail markers were added for exactly this fix, from their shared
  reason string. If a maintainer meant the two Markdown tests for a separate fix, I'll split
  step 3 out.

## Deviations

The code change matched the plan with no unexpected modifications. The commit touched the two planned files, updating the regex patterns in resume parser to allow leading spaces or tabs, and removing the five xfail markers in test resume parser. All tests passed as planned. The reproduction snippet detects both Education and Skills, and the full unit suite moved from 375 passed to 380 passed with zero regressions.

For workflow, the branch fix/54-indented-section-headers was prepared locally before pushing to my fork. The plan comment was posted after the reproduction comment so the issue thread maintained the proper sequence. The risk that an indented line starting with a section keyword inside body text might match as a header remains an inherent behavior of the existing parser logic and was kept out of scope.
