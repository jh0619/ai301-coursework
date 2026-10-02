# Evidence guide: where proof lives in a plan package

This guide tells the grader where to find each kind of evidence and what good evidence looks like. In eval mode, the frozen package is the complete source of truth. In live mode, use the issue thread, repository instructions, reproduction draft or posted reproduction, `plan.md`, and `comment.md`.

## Issue target

### Where it lives

- Eval mode: the package's issue description, expected behavior, actual behavior, acceptance criteria, and thread highlights.
- Live mode: the GitHub issue body and its comment thread.

### What good looks like

The target should identify the specific behavior that is wrong, the behavior expected instead, and any limits stated by the reporter or maintainers. Labels and titles may help locate the issue, but they do not replace the issue body and thread as evidence.

## Reproduction evidence

### Where it lives

- Eval mode: the package's `Repro evidence` section, including environment details, steps, commands, output, logs, stack traces, screenshots, and test results.
- Live mode: the contributor's posted reproduction comment and any local reproduction report included with the plan package.

### What good looks like

The evidence should show what input or action reached the behavior, what the program actually produced, and what was expected. A successful reproduction should contain an observable artifact matching the issue. An honest cannot-reproduce result should show a relevant attempt and identify material differences that may explain the result.

Reproduction evidence proves observed behavior. It does not automatically prove the root cause. A stack frame proves that execution reached a location, but it may not prove why the program entered that state.

## Diagnosis

### Where it lives

- The diagnosis or root-cause section of `plan.md`.
- Supporting artifacts in the Repro evidence.
- When permitted in live mode, the relevant source locations or tests named by the plan.

### What good looks like

Each causal claim should be traceable to a reproduction artifact or clearly labeled as a hypothesis. A diagnosis may identify a confirmed failure boundary without claiming a deeper root cause that has not been established.

Good diagnosis language distinguishes among:

- observed fact: what the command, test, log, or traceback directly shows;
- inference: what the evidence reasonably suggests;
- unknown: what must still be inspected or tested.

A diagnosis is not supported merely because the proposed fix sounds plausible.

## Scope and files

### Where it lives

- The scope, files-to-change, out-of-scope, and approach sections of `plan.md`.
- The issue's requested behavior and maintainer guidance.
- In live mode, the repository tree and named source or test files.

### What good looks like

The plan should name the relevant behavior and the files, functions, components, configuration, or interfaces expected to change. It should also state meaningful exclusions, such as unrelated refactoring, dependency upgrades, API changes, or cleanup.

Exact line numbers are not required because they may change during implementation. The scope must nevertheless be specific enough for a reviewer to recognize unrelated work and determine whether the proposed files belong to the failure path.

## Implementation approach

### Where it lives

- The approach or implementation-steps section of `plan.md`.
- The diagnosis and Repro evidence it claims to address.
- Relevant code locations named in the plan.

### What good looks like

The approach should explain:

1. where the behavior will be changed;
2. what condition or failure will be handled;
3. what behavior will occur afterward;
4. what existing behavior will remain unchanged.

The approach should act on the diagnosed failure path. A workaround that suppresses evidence, skips the failing path, weakens an assertion, or catches unrelated failures does not demonstrate that the issue is addressed.

A plan does not need to contain finished code, exact syntax, or a full patch.

## Test plan

### Where it lives

- The test-plan section of `plan.md`.
- The original Repro evidence and issue acceptance criteria.
- Existing tests and test commands named in the plan.
- The draft plan comment when it summarizes verification.

### What good looks like

The test plan should exercise the real affected behavior using the original trigger or an equivalent input. It should state an observable expected result after the fix, not merely say that tests will be run.

A repeatable manual reproduction may provide sufficient evidence. An automated regression test is not required unless the issue or repository requires one. For example, a repeated interaction loop can pass when it exercises the real behavior, states how many times it will run, and defines the output or visible behavior that must disappear or remain unchanged.

When appropriate, the test plan should include:

- the issue-specific regression test;
- the original reproduction command or equivalent;
- a control for existing valid behavior;
- a broader relevant test file or suite;
- repository-required lint, formatting, or type checks.

A test does not prove the fix if it passes only because the failure path is skipped, the assertion is removed, the evidence is hidden, or the input no longer represents the issue.

## Risks and unknowns

### Where it lives

- The risks, unknowns, assumptions, and out-of-scope sections of `plan.md`.
- The diagnosis and implementation approach.
- Relevant thread guidance and repository constraints.

### What good looks like

The plan should identify uncertainty or regression risk that could materially affect the change, or explain why the change is narrow enough that no additional material risk was found. Important assumptions should include a verification step.

Good risk analysis is specific to the proposed change. Generic statements such as “tests may fail” provide little evidence.

## Plan comment and repository conventions

### Where it lives

- Eval mode: the exact candidate plan comment, repo-facts contribution-policy block, issue thread highlights, and frozen repository instructions.
- Live mode: the exact contents of `comment.md`, the GitHub issue thread, `CONTRIBUTING.md`, issue or pull-request templates, and any applicable AI-use or disclosure policy.

### What good looks like

The comment should accurately summarize the diagnosis, bounded scope, approach, and verification plan from `plan.md`. It should not introduce a broader change, omit a material limitation, claim that an untested fix works, or promise a completion date without a basis.

The comment must follow explicit maintainer instructions and repository communication or disclosure rules. When a policy requires AI-use disclosure, inspect the exact draft comment for every required element. If the policy asks for the tool used and the extent of assistance, both must appear in the comment.

A reference to AI in the issue, thread, plan, package, repository policy, or maintainer suggestion does not count as the contributor's own disclosure. Do not infer that disclosure will be added later.

If the repository has no AI-disclosure requirement, lack of a disclosure is not a failure.

A classmate's plan or claim in the Path Review repository does not block the contributor, but the submitted comment must describe the contributor's own evidence and plan rather than relying on “same as above.”