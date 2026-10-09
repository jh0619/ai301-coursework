---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: is this candidate pull request ready to submit?

Treat a PR package as the candidate title and description, commits, unified diff, and test evidence, read against the issue, the implementation plan including its deviation notes, and the repository's stated contribution standards. Do not implement the change, rewrite the PR, review multiple packages, predict maintainer approval, or answer a different question during the grading run.

## Inputs and modes

Determine whether the request is live mode or eval mode before grading.

### Live mode

Read the following inputs:

1. The issue URL supplied by the user, including the issue body and thread.
2. `plan.md`, including its scope, test plan, risks, and `## Deviations` section.
3. The committed branch diff relative to the repository's default branch. From the working copy, use:

   ```bash
   git diff main...HEAD
   ```

   Use the three-dot diff so the graded change is everything the branch introduces since its merge base with `main`.
4. The branch's commit list and changed-file list when needed to locate unrelated changes or review debris.
5. The draft PR title and description from the file named by the user, normally `pr_draft.md`.
6. The captured test commands and printed output from the file named by the user, normally `test_evidence.md`.
7. The repository's PR template, normally `.github/PULL_REQUEST_TEMPLATE.md`.
8. The repository's stated contribution requirements, including `docs/CONTRIBUTING.md` and any applicable AI-use or disclosure policy.

Grade the files named in the command. Do not substitute a different plan, PR draft, or evidence file. Grade only committed branch changes in `git diff main...HEAD`; uncommitted source edits are not part of the candidate PR and must be reported as a gap when they materially affect the package.

For a house-chain student, read the supplied house issue, house plan, house reproduction pack, house branch diff, draft PR text, and test evidence in place of the student's own artifacts. Apply the same rubric and procedure.

If a required live input cannot be located or the branch diff cannot be determined, stop without inventing it. State which input is missing and do not emit a readiness verdict.

### Eval mode

Treat the supplied package bundle as the whole world. Read the issue context, repository facts, plan context, commits, diff, candidate title and description, and test evidence only from the bundle.

Do not access GitHub, the network, the local repository, `scope.md`, `voice-guide.md`, or any source outside the bundle. Do not correct the bundle with current live information. Run every rubric check and apply the full verdict rule to the frozen package.

## The scope seam (live mode only)

Before reading any other live-mode input, read `scope.md`.

If the `Repo:` line still contains a bracketed placeholder, stop without grading and tell the user to replace it with the section's Path Review repository. Never guess the repository.

Confirm that the issue and candidate PR belong to the repository named by `scope.md`. Apply the Path Review house rules recorded there. Refuse to grade work for any other repository and identify the scope mismatch.

Ignore `scope.md` entirely in eval mode.

## The voice seam (live mode only)

After confirming scope, read `voice-guide.md`. Hold the outgoing PR title and description against every applicable voice rule.

Report each broken voice rule in the readable summary, naming the rule and the exact draft language that conflicts with it. Voice-guide findings do not change the verdict by themselves unless a rubric check explicitly uses voice-guide evidence.

Do not rewrite or post the PR text during the grading run.

Ignore `voice-guide.md` entirely in eval mode.

## Component reads

Read `rubric.md`, `procedure.md`, and `references/evidence-guide.md` before assigning any grade.

Use the components as follows:

- `rubric.md` defines every check, its evidence, its pass condition, its weight, and the verdict rule.
- `procedure.md` defines the read order, evidence-gathering steps, check execution, and verdict assembly. Execute it exactly as written.
- `references/evidence-guide.md` identifies where each evidence family lives in eval and live packages and what observable evidence looks like.

If `rubric.md` contains no substantive check or verdict rule, refuse to grade and state that the rubric is empty.

If `procedure.md` contains no substantive operating steps, refuse to grade and state that the procedure is empty.

If the procedure is silent about how to perform a required step, report the procedure gap. Do not improvise a replacement method or silently borrow a step from another skill. Use `unclear` for an applicable rubric check whose required evidence cannot be resolved under the written procedure.

Do not create new checks during a run. Do not change a grade because the outcome feels wrong when the written pass condition says otherwise.

## Verdict and output

Use a binary verdict:

- `accept` means the PR package is ready to submit under the written rubric.
- `reject` means the PR package must be held.

There is no score, conditional acceptance, or third verdict. Put reservations and non-blocking feedback in the readable per-check summary rather than altering the verdict space.

Before the JSON block, provide a concise check-by-check summary. For every check, state its exact rubric name, grade, and the specific fact or quote that determined the grade. Identify any missing input, procedure gap, disclosed shortfall, or live-mode voice-guide finding separately.

End the reply with the fenced JSON block below. It must be valid JSON and must be the final content in the reply. Do not write anything after its closing fence.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

Follow these rules for every run:

1. Lead with evidence. Do not grade a check without identifying the concrete file, diff hunk, command output, repository rule, or quote that decided it.
2. Grade the artifact, not its polish. A short but complete PR may pass; a confident and well-formatted PR with unsupported claims, silent drift, or missing evidence must not.
3. Let the rubric decide what is checked. Apply every row exactly as written and do not add hidden requirements.
4. Let the procedure decide how evidence is gathered and compared. Follow its order and report gaps instead of improvising around them.
5. Compare negative claims against the complete diff. A claim such as “no dependency changes” passes only when no changed hunk introduces that change.
6. Treat command-plus-output as test evidence. A sentence saying that a test passed is not printed test evidence.
7. Distinguish an honestly disclosed shortfall from a hidden contradiction. Do not reject merely because a disclosed non-required check failed when the rubric's pass condition allows honest disclosure.
8. Assign `pass` only when the stated pass condition is supported by evidence.
9. Assign `fail` when the evidence contradicts the pass condition or directly shows it is unmet.
10. Assign `unclear` when an applicable check lacks enough verifiable evidence to decide. If the rubric's verdict rule is silent about `unclear`, treat it as a failing grade.
11. Use only `accept` or `reject` for the final verdict and ensure the JSON block is valid, complete, and last.