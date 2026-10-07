# Rubric: is this plan ready to post and build from?

A plan package is the candidate plan plus the candidate plan comment, read against the
Issue, the Thread highlights, the Repro evidence, and the Repo facts block. In eval mode
the bundle is the whole world. In live mode the drafts are the candidate side, and the
issue thread, the student's posted repro comment (or the repro evidence the drafts quote),
and the repo's docs are the evidence side. `procedure.md` says the order to run these in.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The cause the Candidate plan states, read against every step and control in the Repro evidence block, and against maintainer findings in Thread highlights. | Pass if the stated cause explains the failure the repro shows AND is consistent with every control and observation in the repro evidence (a control that shows the suspected code working rules that code out). Fail if any repro step or control contradicts the stated cause, if the plan blames something the evidence never touches while ignoring what the evidence does point at, or if the plan states no cause at all. | required |
| one-bounded-change | The Candidate plan's scope statement (in / not in), its file list, and its approach, read against what the Issue asks for. | Pass if the plan makes the one change the issue's bug needs. Fixing the same mistake at sibling sites, adding a regression test, and explicitly deferring related work all still pass. Fail if the plan also takes on work the issue did not ask for: a refactor, a migration or dependency upgrade, a redesign, new options or UI, or a "while I'm in there" item, even when the core fix inside it is right. | required |
| stranger-can-start | The Candidate plan's files or areas, its chosen approach, and its order of work. | Pass if someone who has never talked to the author could start building from the plan alone: it names the files or functions to change and commits to one approach. Fail if a decision the build depends on is left open ("somewhere in the input layer", "upstream or vendored, whichever is easier", "gocui? tcell? not sure"), or if no file, function, or area is named. | required |
| decisive-test-plan | The Candidate plan's test plan, read against the Repro evidence's steps and artifacts. | Pass if the test plan names an observable result that would show this bug is fixed: re-running the repro (or a test built from it) with the expected-after output or exit code stated, or a new test asserting the specific behavior. Fail if the only test is general ("run the full test suite", "nothing else should break", "should feel fast"), or if no expected-after outcome for the bug itself is named. | required |
| thread-direction-respected | The Candidate plan comment and the Candidate plan, read against maintainer comments (OWNER, MEMBER, COLLABORATOR) in Thread highlights: a named culprit, a requested approach, a patch or test build to try, or open PRs on the issue. | Pass if the thread has no maintainer direction, or if the plan comment engages it: it follows the direction, or says why it departs from it. Fail if a maintainer gave explicit direction (named the culprit, asked for testing of a patch, chose an approach) and the comment and plan proceed as if that direction were not there. | required |
| ai-policy-disclosed | The `contribution policy` line in Repo facts (including any AI-use policy), read against the Candidate plan comment. Live mode: CONTRIBUTING.md and any AI policy file. | Pass if the comment meets what the stated policy requires of comments. When the policy requires disclosing AI assistance, the comment must say that AI tools were used and how; every package here is AI-assisted work. When the policy requires comments in the author's own words, the comment must not present itself as AI output. Pass when the policy says nothing about AI. Fail if a required disclosure is missing or a stated requirement for comments is broken. | required |
| unknowns-stated | The Candidate plan's risks or unknowns, read against what the repro evidence leaves open. | Pass if the plan names at least one real risk or open question, or the change is small enough that it has none worth naming. | preferred |

## Verdict rule

Accept (ready to post and build) only if every `required` check passes. A `required`
check that fails, or is `unclear`, means reject (hold): a plan I cannot verify is not one
to build from. `unclear` is for evidence that is genuinely missing from the package, not
for a hard call; a hard call gets `pass` or `fail` with the reason. `preferred` checks
never change the verdict.
