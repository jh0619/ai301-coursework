# Procedure: how to grade an implementation plan

## Read order

1. In live mode, confirm that the issue belongs to the repository named in `scope.md`. In eval mode, treat the frozen package as the complete world and do not open the live GitHub issue.
2. Read the issue description from beginning to end. Record the reported behavior, expected behavior, affected component, and any explicit limits or acceptance criteria.
3. Read the thread highlights and repository facts. Record maintainer guidance, rejected approaches, ownership constraints, contribution requirements, and any disclosure rules that apply to the plan comment.
4. Read the Repro evidence before reading the plan. Record the environment, trigger, command or action, observed output, expected output, and whether the evidence shows successful reproduction or an honest cannot-reproduce result.
5. Read the complete plan. Identify its diagnosis, scope, files or components to change, implementation approach, test plan, risks, unknowns, and out-of-scope limits.
6. Read the draft plan comment last. Compare it with the full plan, issue, thread, Repro evidence, and repository conventions.
7. Do not change this order. Reading the plan before the Repro evidence can make an unsupported diagnosis appear more convincing than the evidence allows.

## Evidence gathering

1. Write down the issue's target behavior as a comparison between expected and actual behavior.
2. Extract the strongest observed artifact from the Repro evidence, such as an error message, stack trace, log, screenshot, failing test, or command output. Record what that artifact directly proves and what it does not prove.
3. Extract every causal statement from the plan's diagnosis. For each statement, identify the specific reproduction artifact that supports it. If no artifact supports it, determine whether the plan labels it as a hypothesis and includes a verification step.
4. Extract the proposed files, functions, components, configuration, or interfaces to change. Compare them with the diagnosed failure path and the boundaries of the issue.
5. Extract the plan's explicit exclusions. Check whether they prevent unrelated cleanup, dependency changes, broad refactoring, or behavior changes outside the issue.
6. Map each implementation step to the diagnosis: identify which part of the reproduced failure it addresses and what behavior it is intended to change.
7. Extract every planned test or verification command. For each one, record:
   - which real code path it exercises;
   - which original reproduction input or equivalent trigger it uses;
   - the expected post-fix output;
   - whether it would fail before the fix and pass after it;
   - whether a regression or control case is needed.
8. Compare the draft comment with the full plan. Record any omitted limitation, unsupported certainty, changed scope, promise, or conflict with maintainer guidance.
9. In live mode, inspect only the repository locations necessary to verify file names, code locations, tests, contribution rules, and thread constraints. Do not design or implement the fix while grading.
10. Keep facts, reasonable inferences, and unknowns separate. Do not convert an inference into a fact simply because the proposed fix appears plausible.
11. Extract every mandatory communication rule from the repository policy before grading the draft comment. If AI disclosure is required, list each required disclosure element, such as the tool used and the extent of assistance, and locate those elements in the exact draft comment. A reference to AI in the issue, thread, plan, or repository policy is not the contributor's disclosure.

## Check execution

1. Grade every rubric row independently and in table order.
2. Apply the row's stated Evidence and Pass condition exactly. Do not add an unstated requirement based on personal preference.
3. Assign `pass` only when the required condition is supported by evidence in the package or in repository evidence that this procedure permits.
4. Assign `fail` when the available evidence directly contradicts the pass condition or clearly shows that the condition is not met.
5. Assign `unclear` when the check is applicable but necessary evidence is missing, ambiguous, or insufficient to distinguish pass from fail.
6. For the diagnosis check, compare the cause with the Repro evidence rather than merely checking whether the cause sounds technically plausible.
7. For the scope and approach checks, do not require exact line numbers or a finished patch. Require enough specificity to identify the intended change and determine whether it addresses the diagnosed failure.
8. For the test-plan check, judge what the proposed test would demonstrate. A named test or command does not pass if it can succeed without exercising the reported failure path. A repeatable manual reproduction may pass when it exercises the real affected behavior and defines an observable post-fix result; do not require automation unless the issue or repository requires it.
9. For the comment check, grade the exact draft text that would be posted. Do not assume omitted limits, caveats, or disclosures will be added later.
10. For a required AI disclosure, assign `fail` when any required element is absent from the exact draft comment. Do not infer disclosure from the fact that an AI-related issue, suggestion, policy, or grading skill appears elsewhere in the package.
11. For each grade, record one concise evidence statement that names the relevant package section, artifact, plan statement, or repository instruction.
12. Do not repair the plan while grading it. Missing diagnosis, scope, verification, or communication evidence must affect the grade rather than being supplied by the grader.

## Verdict assembly

1. Collect the grades for all required checks.
2. Return `accept` (`ready`) only when every required check is `pass`.
3. Return `reject` (`hold`) when any required check is `fail` or `unclear`.
4. Never allow a preferred check to change an `accept` to `reject` or a `reject` to `accept`. Report preferred-check feedback separately.
5. Identify every required check responsible for a `reject`; do not report only the first failure.
6. Present a readable check-by-check result followed by the structured JSON block required by `SKILL.md`.
7. In the JSON block, use the exact check names from `rubric.md`, include each check's grade and supporting evidence, and use only `accept` or `reject` for the final verdict.
8. Do not post the plan comment, modify repository files, or begin implementation while grading unless the user separately requests those actions.