# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Plan fidelity | Compare the plan-context block's stated scope, files, exclusions, and deviation notes with every changed file and behavioral change in the candidate diff. Also compare the PR description's implementation claims with the diff. | Pass if the diff implements the planned change within its stated boundary, every changed file and meaningful behavior is covered by the plan or an explicit deviation note, and the description does not claim work the diff lacks or conceal material drift. An honestly disclosed, justified deviation may pass. Fail if the implementation silently adds, omits, or changes material work relative to the plan. | required |
| Description matches implementation | Compare each factual claim in the candidate title and description with the issue, plan, commits, diff, and test evidence. | Pass if the title and description accurately state the problem, implemented behavior, limits, files or tests when named, and verified results. Honest limitations may pass. Fail if a material claim is contradicted by or unverifiable from the package. | required |
| Test evidence is observable | Compare the original reproduction and the plan's expected-after behavior with the candidate diff's regression assertions and the Test evidence section. | Pass when the package gives concrete, issue-specific evidence that the target behavior was exercised after the change and produced the expected result. Decisive evidence may be direct output, a before/after transcript, an interactive observation, a command transcript with specific result comments, or a targeted regression assertion combined with a concrete result from the command or suite that ran it. A concrete result can be an exit code, matched or omitted value, visible behavior, named passing case, or test count. Once the core issue behavior is demonstrated, summarized results for supporting controls or broader suites are sufficient; do not require raw output for every plan item. Fail when the package gives only unsupported prose such as "tested locally," "works now," or "tests pass," when no command or interaction is tied to the claimed result, or when the evidence does not exercise the issue's target behavior. | required |
| Diff is focused and reviewable | Read the unified diff and commit list against the issue and plan. Inspect all changed files and hunks for unrelated edits, generated artifacts, debug output, dead or commented-out code, formatting churn, and other debris. | Pass if the intended fix and tests are visible, the commits and hunks stay focused on the issue, and no unrelated or accidental material obscures review. Fail if unrelated changes, artifacts, or cleanup substantially widen or bury the change. | required |
| Repository standards are satisfied | Compare the candidate title, description, commits, changed files, and test evidence with the repo-facts block's PR-template requests, contribution instructions, testing requirements, changelog rules, commit conventions, maintainer directions, and AI-use policy. | Pass if every applicable stated requirement is satisfied with real content. Required AI disclosure must identify the assistance to the extent the repository requests. If no disclosure rule exists, absence of a disclosure does not fail. Fail if an applicable template field, test or changelog requirement, contribution rule, disclosure rule, or explicit maintainer direction is ignored. | required |
| Reviewer context is useful | Read the title and description against the issue, plan, diff, test evidence, known limitations, and risks. | Pass if a reviewer can understand the problem, implemented change, main verification, and material limitations. A terse description may pass, and supporting test details may remain in the Test evidence section. Mark missing context as fail or unclear only when a material implementation limit or reviewer decision is hidden. This preferred check never changes the verdict. | preferred |

## Verdict rule

Return `accept` only when every required check passes.

Return `reject` when any required check is `fail` or `unclear`. An `unclear`
required check counts as failure because the package has not made readiness
verifiable.

Preferred checks provide feedback only and never change the verdict. A disclosed
limitation or incomplete optional follow-up does not by itself require rejection
when the implemented core claim is supported, the PR remains within its declared
boundary, and all required checks pass.