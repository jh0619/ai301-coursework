# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

# Rubric: is this a good first issue?

## Checks

| Check                  | Evidence                                                                                                                                                                                                          | Pass condition                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Weight   |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| Maintainer active      | Check the repo-facts block for the latest maintainer activity and read the issue comment thread for maintainer responses.                                                                                         | Pass if a maintainer has commented on an issue, reviewed or merged a pull request, or committed to the default branch within the last 90 days. Bot activity does not count.                                                                                                                                                                                                                                                                                                                               | required |
| Repository in use      | Check whether the repository is archived and inspect the last 5 default-branch commit dates listed in the repo-facts block or the repository activity locations described in `references/evidence-guide.md`.      | Pass if the repository is not archived and at least 2 of the last 5 default-branch commits were made within the last 90 days. Bot-only dependency updates do not count toward the 2 commits.                                                                                                                                                                                                                                                                                                              | required |
| Newcomer-sized scope   | Read the issue body and comment thread for the requested outcome, explicit blockers, prior implementation attempts, and linked pull-request history.                                                              | Pass if the issue identifies a concrete problem or requested outcome and provides at least one actionable implementation or investigation direction. Multiple possible causes, multiple files, technical complexity, performance work, or threading do not cause failure by themselves. Fail if there are at least 2 closed linked pull requests, or if a maintainer explicitly states that work is blocked pending an unresolved product decision, architecture decision, or requirements clarification. | required |
| Available to claim     | Check the assignee field in the repo-facts block and read the full comment thread for current claim statements, open linked pull requests, or evidence that another contributor is actively working on the issue. | Pass if there is no current assignee, no open linked pull request, and no claim or work-in-progress statement within the last 30 days that remains active. Old claims that were explicitly released or automatically unassigned do not cause failure.                                                                                                                                                                                                                                                     | required |
| AI contribution policy | Check the contribution-policy lines in the repo-facts block for rules about generative AI or AI-assisted contributions.                                                                                           | Pass if the policy allows AI-assisted contributions or contains no restriction on AI use. Fail if the policy explicitly prohibits AI-generated code or documentation. Requirements that contributors understand, review, explain, and test AI-assisted work still pass.                                                                                                                                                                                                                                   | required |

## Verdict rule

Accept an issue only when every required check passes. Reject the issue if any required check fails.

For Maintainer active, Repository in use, Newcomer-sized scope, and Available to claim, an `unclear` grade counts as failure. For AI contribution policy, the absence of an AI restriction counts as pass; only an explicit prohibition counts as failure.

If multiple issues are accepted, rank issues with clearer expected outcomes, narrower scope, fewer failed prior attempts, and more recent maintainer responses above the others.
