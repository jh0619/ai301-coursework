# Procedure: how this tool grades a PR package

## Read order

1. Determine the mode before grading.
   - In eval mode, treat the package bundle as the entire world. Do not fetch or infer anything outside it.
   - In live mode, use the inputs and locations defined by `SKILL.md`, `scope.md`, and `references/evidence-guide.md`.

2. Read the issue and thread evidence first. Record:
   - the target behavior;
   - the expected behavior;
   - important controls or platform conditions;
   - explicit maintainer directions.

3. Read the repository facts and contribution requirements. Record:
   - PR-template asks;
   - required tests or checks;
   - changelog and commit requirements;
   - AI-use or disclosure requirements.

4. Read the complete plan context before reading the candidate PR. Record:
   - diagnosis and intended behavioral change;
   - named files or directories;
   - scope exclusions;
   - test plan;
   - risks, limitations, and deviation notes.

5. Read the candidate title and description. List every factual claim about:
   - the cause;
   - the implementation;
   - changed files or scope;
   - tests and results;
   - limitations and compliance.

6. Read the commits and complete diff. Record every changed file and the purpose of each meaningful hunk. Note regression assertions and any unrelated, generated, debugging, dead-code, or formatting-only changes.

7. Read the Test evidence section last. For each command or interaction, record:
   - the command or action;
   - the trigger input or environment when relevant;
   - the concrete observed result;
   - whether the result is before or after the change;
   - which plan test or repository check it satisfies.

This order prevents the candidate description from defining its own standard. The plan defines the expected boundary before the diff is judged, and the test plan defines what the evidence must prove before test claims are graded.

## Evidence gathering

### Plan fidelity

1. Make a scope table with one row for every changed file.
2. For each file, identify the plan statement or deviation note that authorizes it.
3. Compare each planned material change with the diff and mark it implemented, explicitly deferred, or silently missing.
4. Compare every implementation claim in the description with the actual diff.
5. Record any material change that is larger or smaller than the declared plan.

### Description accuracy

1. Split the title and description into separately verifiable claims.
2. Match each claim to the issue, plan, diff, commits, or test evidence.
3. Record contradictions, overstatements, unsupported results, and accurately disclosed limitations.
4. Do not require a particular length or heading structure unless the repository explicitly requires it.

### Test evidence

1. Start with the original reproduction and the plan's test plan.
2. Identify the target behavior that must change and any control behavior that must remain unchanged.
3. Find the exact command, interaction, test, or assertion that exercises each behavior.
4. Record the concrete result associated with it.

Count any of the following as potentially observable evidence:

- raw command output;
- a concrete result or test count clearly tied to a named command;
- a command transcript whose comments state specific observed behavior, such as an exit code, visible UI result, matched file, ignored file, or absence of the original error;
- a targeted regression assertion visible in the diff, paired with evidence that the relevant test command or suite ran successfully;
- for a command that succeeds by producing no output, a precise report of that absence when supported by the target assertion, comparison, or control.

Do not count these by themselves:

- "tested locally";
- "tests pass";
- a broad suite result with no evidence that the issue's behavior was exercised;
- expected output presented as though it were observed output;
- a source-code change with no execution evidence.

A broad test suite may support target-specific evidence, but it does not replace it. An honestly disclosed unavailable or deferred optional check may pass when the core behavior is still demonstrated and repository requirements are met.

### Diff quality

1. Review every changed file and hunk, not only the main fix.
2. Look for debug statements, temporary files, generated artifacts, dead code, commented-out blocks, formatting churn, and unrelated cleanup.
3. Compare the commit list with the diff and flag unrelated or misleading commits.
4. Record whether the fix and its regression protection remain easy to isolate and review.

### Standards and communications

1. Translate every applicable repository instruction into a concrete requirement.
2. Locate its satisfaction in the title, description, commits, diff, or test evidence.
3. Verify required PR-template sections contain real content.
4. Verify required changelog entries, tests, commit conventions, and AI-use disclosures.
5. Treat the absence of an AI disclosure as a failure only when the repository requires one.
6. Compare explicit thread guidance with the candidate PR and record whether it was followed or directly addressed.

## Check execution

1. Grade checks in the exact order they appear in `rubric.md`.
2. Use only the gathered evidence named by that check.
3. Assign:
   - `pass` when the stated pass condition is demonstrated;
   - `fail` when the evidence contradicts the pass condition;
   - `unclear` when required evidence is absent or cannot decide the condition.
4. Do not turn formatting preferences or personal taste into failures.
5. Do not repair a rubric gap during the run. If the rubric does not decide a concern, report the gap separately and apply the written rubric.
6. Do not repair a procedure gap by silently inventing a search or comparison. Report the gap and grade only from evidence the procedure directs you to gather.
7. Write one evidence line per check naming the concrete fact, quote, changed file, assertion, command result, or missing artifact that decided the grade.

## Verdict assembly

1. Apply the verdict rule in `rubric.md` exactly.
2. Treat every required `unclear` as a failure.
3. Return `accept` only if all required checks pass.
4. Return `reject` if any required check fails or is unclear.
5. Preferred checks never change the verdict.
6. In the readable summary, identify all failed or unclear checks. Lead with the first failed or unclear required check in rubric order as the deciding check.
7. Include every rubric check in the final JSON in rubric order.
8. End with the exact fenced JSON structure required by `SKILL.md`. The JSON block must be valid and must be the final content in the response.