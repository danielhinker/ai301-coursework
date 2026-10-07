# Procedure: how this skill grades a plan package

## Read order

Read the whole package once, in this order, before grading anything:

1. **Repo facts.** Note the `contribution policy` line word for word, especially any AI-use
   rule (disclosure required? own words required? silent?).
2. **Issue.** Note the reported failure (error, wrong output, exit code) and what the
   issue asks for.
3. **Thread highlights.** List every comment by an OWNER, MEMBER, or COLLABORATOR, and
   note any named culprit, requested approach, patch or test build to try, or open PR.
   Write "no maintainer direction" if there is none.
4. **Repro evidence.** Note the behavior it pins down, then each control and what that
   control rules in or out ("same items without -v parse fine: the tokenizer is not the
   cause").
5. **Candidate plan.** Note its stated cause, its in and not-in scope, its files, its
   approach, its test plan, and its risks.
6. **Candidate plan comment.** Note what it commits to, whether it mentions the
   maintainer direction from step 3, and whether it discloses AI use.

The order matters: the thread and repro evidence come before the plan so the plan is read
against what is already known, not the other way round. A confident, well-formatted plan
read first tends to set what the evidence "must" mean.

In live mode, steps 1-4 come from the live sources named in
`references/evidence-guide.md`: the repo's CONTRIBUTING.md and AI policy file, the issue
page, its comments, and the student's posted repro comment, or the repro evidence the
drafts quote when that comment isn't posted yet. Steps 5-6 are `plan.md` and the draft
comment file.

## Evidence gathering

For each check, pull exactly this, using the notes from the read:

- **diagnosis-grounded:** the plan's cause sentence (quote it), plus each repro control
  and maintainer finding from steps 3-4. For each control, write one line: does the
  stated cause predict what that control showed?
- **one-bounded-change:** the plan's in and not-in statements and every item in its
  approach. Mark each item "needed for this bug" or "extra".
- **stranger-can-start:** the named files or functions, and any sentence where the plan
  defers a build decision ("not sure", "either", "whichever", "somewhere").
- **decisive-test-plan:** the test plan's commands or tests, and the expected-after
  output it names for the bug itself.
- **thread-direction-respected:** the maintainer-direction list from step 3, and the
  sentence(s) in the comment or plan that respond to each item.
- **ai-policy-disclosed:** the policy line from step 1 and any AI-disclosure sentence in
  the comment.
- **unknowns-stated:** the plan's risks or open questions, if any.

If a check's evidence is not in the package at all, record "absent" for it. Don't infer
it from elsewhere.

## Check execution

Run the checks in table order: diagnosis-grounded, one-bounded-change,
stranger-can-start, decisive-test-plan, thread-direction-respected, ai-policy-disclosed,
then unknowns-stated. Grade every check, even after one fails, so the output shows every
problem at once.

For each check, apply its pass condition to the gathered evidence only, and grade
`pass` or `fail` with a one-line quote or fact that decided it. Use `unclear` only when
the gathered evidence was "absent". A hard call is not `unclear`: pick `pass` or `fail`
and say why in the evidence line.

Grade the plan's substance, never its polish. A short plan with a grounded cause, one
named file, and a stated expected-after can pass everything. A long, confident one can
fail diagnosis-grounded on a single control.

## Verdict assembly

Apply the rubric's verdict rule: `accept` only if every required check is `pass`; any
required `fail` or `unclear` gives `reject`. `unknowns-stated` never changes the verdict.
In the readable summary before the JSON, name the deciding check for a `reject` (the first
required check that failed, in table order) and quote its evidence line. Then emit the
JSON block in SKILL.md's format, with one entry per check in table order, last in the
output.
