# Rubric: is this a good first issue?

Dates: in eval mode, measure every "within N days" against the bundle's
capture date. In live mode, measure against today.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-in-use | Repo facts: the `archived:` value on the repo line, and the `last push to any branch` date. Live mode: the repo page's archived banner and the newest push on the Branches page. | Pass if archived is `no` AND the last push to any branch is within 180 days of the capture date. Releases are not required: a repo with no published releases passes on push activity alone. | required |
| maintainer-active | Repo facts: the `last 5 default-branch commits`, with their dates and authors. Live mode: the default branch's commit history. | Pass if at least one of those commits is dated within 90 days of the capture date AND was made by a person, or is a bot commit that merges a person's pull request ("Merge pull request #N from someone/..."). Commits by accounts ending in `[bot]` that don't merge a person's PR don't count. | required |
| unclaimed | Repo facts `this issue:` line (assignees; linked PRs and their states), plus the Comments section. Live mode: the issue's Assignees and Development boxes, plus the thread. | Pass only if all three hold: (1) assignees is `none`; (2) no linked PR has state `open`, and no comment links an open PR for this issue (closed or merged PRs don't block); (3) no non-maintainer comment claiming the issue ("I'd like to work on this", "can I take this", "working on this", "@bot claim") is dated within 30 days before the capture date, unless a later maintainer comment says the issue is free. A bot reminder asking whether someone is still working on it does not clear a claim. | required |
| single-bounded-task | The issue title and body, plus maintainer comments. | Pass if the issue asks for one change that one pull request can finish. A short body, or a short list of items one PR would cover, still passes, and so does a bug report that names several causes or suggested fixes. Fail if any of: the title or body calls it a megaissue, tracking, umbrella, or meta issue; the body lists separate tasks meant to be split across many PRs, or invites incremental "PRs big and small"; it is a usage or support question rather than a change. | required |
| settled-and-endorsed | The issue author's association, maintainer comments (OWNER, MEMBER, or COLLABORATOR), and the linked PR states. | Pass only if both hold: (1) it is a bug report or a documentation fix, OR it is a feature request that a maintainer opened or endorsed in the thread (agreed on it, settled the approach, or invited takers); (2) fewer than 2 linked PRs were closed without being merged. Fail a feature request with no maintainer involvement at all (for example, filed by a bot or an outside user with no maintainer reply): it hides a product decision nobody has made. | required |
| ai-policy-allows | Repo facts `contribution policy` line. Live mode: CONTRIBUTING.md, any AI policy file, and the PR template. | Fail only on an outright ban on AI-generated contributions (for example, "We do not accept AI-generated code"). Pass when the policy is silent, welcomes AI tools, ships an AGENTS.md, or sets conditions (disclose it, understand and test it, human review before submitting). | required |
| maintainer-signal | The issue author's association and labels. | Pass if a maintainer opened the issue, or it carries a `good first issue` or `easy` label. | preferred |
| has-repro-or-spec | The issue body. | Pass if the body gives reproduction steps, a failing command or output, or a concrete expected result. | preferred |

## Verdict rule

Accept only if every `required` check passes. Any `required` check that
fails or is `unclear` means reject: an issue I can't verify is not one I
should take. `preferred` checks never change the verdict; they only rank
accepted issues (more preferred passes first, then the fit profile in
`scope.md`).
