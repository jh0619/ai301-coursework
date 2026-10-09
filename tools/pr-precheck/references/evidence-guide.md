# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

### Where it lives

In an eval bundle, read:

- the Plan context block for the diagnosis, scope, named files, exclusions, test plan, limitations, and deviation notes;
- the Candidate PR diff and commits for the implementation actually delivered;
- the Candidate PR title and description for claims about plan fidelity, implementation, and scope.

In live mode, read:

- `plan.md`, including its deviations section;
- the complete branch diff relative to the default branch, normally `git diff main...HEAD`;
- the branch commit list;
- the draft PR title and description.

### What good looks like

Every changed file and material behavior falls within the plan's stated boundary or an explicit, honest deviation note. Every material planned change is either implemented or explicitly disclosed as deferred. The description accurately matches the diff.

Silent drift includes unplanned files or behavior, omitted planned work with no note, or a description that claims more or less than the diff delivers.

## Test evidence (harness category: not-tested)

### Where it lives

In an eval bundle, read:

- the original Repro evidence and the Plan context's test plan;
- regression assertions visible in the Candidate PR diff;
- the Candidate PR Test evidence section;
- repository-required checks named in Repo facts.

In live mode, read:

- the Unit 2 reproduction and Unit 3 test plan;
- regression tests in the branch diff;
- the captured commands and outputs in `test_evidence.md`;
- repository-required test, lint, build, and type-check results.

### What good looks like

The evidence is traceable from a command, interaction, or targeted regression assertion to the issue's target behavior and its expected result after the change. Valid evidence may include:

- raw terminal output;
- a concrete test result or count associated with a named command;
- a command transcript whose comments record a specific observed result;
- a concrete interactive or UI observation;
- a targeted regression assertion in the diff paired with evidence that its test command or suite ran;
- a precisely identified absence of output or error when silence is the expected result.

The package should also show the relevant repository checks or honestly disclose a nonessential limitation. A broad suite pass supports target-specific evidence but does not replace it.

"Tested locally," "works now," or "tests pass" without a command-specific, behavior-specific result is not sufficient. Source code or an unexecuted test alone is also not proof.

## Diff quality (harness category: unreviewable)

### Where it lives

In an eval bundle, read the complete unified diff and commit list.

In live mode, read:

- `git diff main...HEAD`;
- the commit list between the default branch and `HEAD`;
- the working-tree status when checking for accidental uncommitted artifacts.

### What good looks like

The intended fix and regression protection are easy to identify. Changed files and hunks are related to the issue and plan. The diff contains no debug leftovers, temporary artifacts, generated files that should not be committed, dead code, commented-out experiments, broad formatting churn, or unrelated drive-by cleanup.

Multiple commits are acceptable when they remain focused and accurately described. A single commit is not automatically reviewable.

## Standards and comms (harness category: standards-wall)

### Where it lives

In an eval bundle, read:

- Repo facts for PR-template asks and contribution policy;
- Thread highlights for explicit maintainer direction;
- the Candidate PR title, description, commits, diff, and Test evidence.

In live mode, read:

- the repository PR template;
- `CONTRIBUTING.md` and any linked contribution or AI policy;
- applicable issue templates and maintainer comments;
- the draft title and description;
- commits, changed files, and captured test evidence.

### What good looks like

Every applicable repository requirement is satisfied with real, relevant content. Required tests and changelog entries are present, commit and title conventions are followed, required template sections are completed, and explicit maintainer guidance is addressed.

When the repository requires AI-use disclosure, the PR includes the required tool and extent information and otherwise follows the human-review requirements. When the repository states no such requirement, absence of a disclosure is not a failure.

Claims that the implementation matches the plan or diff belong under Plan fidelity, not this family.