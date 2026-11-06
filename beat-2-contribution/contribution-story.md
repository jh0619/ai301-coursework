# Contribution: "Show full description" does not remove ellipsis after expanding opportunity description

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Jia Hou
**GitHub Username:** jh0619
**Issue:** https://github.com/HHS/simpler-grants-gov/issues/12056
**Pull Request:** https://github.com/HHS/simpler-grants-gov/pull/13116

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built and refined an issue scout for real GitHub searches**

I built an issue scout with Python, Java, and TypeScript search queries, free ranking filters, and my Unit 1 issue-selection rubric. The first scan graded five issues but four were already claimed, so I changed the queries from `--comments "1..10"` to `--comments "0..10"` and ranked zero-comment issues first. Direct grading later exposed a second weakness: the rubric accepted an issue already completed by a merged PR and another issue waiting on an unresolved product decision. I added a required `Issue is still valid` check and verified the revision by re-grading the same candidates; it correctly rejected HHS/simpler-grants-gov#5277 and #12056 while keeping ESCOMP/CTSM#2313 accepted.

**Unit 6: Added a voice guard and repository memory for upstream work**

I installed a global pre-command voice guard that checks GitHub comments against my voice guide, and its wiring test correctly stopped a body-less `gh issue comment --help` command before execution. Repro-check accepted my claim draft but identified wording that left the availability check to the maintainer and implied implementation too early, so I revised the draft to state what I had checked and promise investigation before a change. I also created an excluded `CLAUDE.local.md` for `HHS/simpler-grants-gov`; a scoped orientation captured the two relevant components, missing regression-test coverage, repository conventions, and exact frontend checks without mapping the whole repository. The orientation revealed that expansion state lives in a child component while the ellipsis is rendered by its parent, so I recorded that design boundary and the open scope question for future sessions.

**Unit 7: Added a ship-gate and opened a checked upstream pull request**

I created a repository-specific gate manifest that runs the frontend unit tests, lint, TypeScript check, and formatting check before a PR can open. On my real branch, all four commands passed, but pr-precheck stopped the first draft because `Fixes #12056` would close an issue whose threshold requirements remain unresolved, then stopped the second draft because the repository requires a designated maintainer tag. I changed the description to `Work for #12056`, explicitly disclosed the partial scope, and tagged `@serverless-mom`; the final gate passed every check and Claude Code opened PR #13116. Claude Code appended its generated-with-Claude attribution while posting the PR, but it did not change the implementation or test evidence.

**Unit 8: Added a PR monitor and an evidence-based response playbook**

I installed the PR monitor and wrote a four-part playbook using the repository's contribution rules and the response times of five recently merged external PRs. My first search page found only one qualifying external PR, so I expanded the search across older merged PRs and used the observed 0.8-to-32.2-day range to set a 21-quiet-day first-review threshold. On PR #13116, the monitor found no comments or reviews after 3 days and drafted a wait note rather than a nudge. I agreed with the draft and posted nothing because my threshold had not been met.
---
## Why I Chose This Issue

I found this issue through the AI301 Issue Selection site after building an issue scout that searched GitHub for open Python, Java, and TypeScript bugs and graded candidates with my issue-selection rubric. The first scan showed that recent issues with comments were often already claimed, so I changed my search strategy to include zero-comment issues and rank them first. I then directly graded the issues I was considering from the selection site. My first version of the rubric exposed another weakness: it could identify inactive repositories and competing claims, but it did not reliably detect work that had already been completed or was waiting on an unresolved decision. I added an `Issue is still valid` check to inspect current code and pull-request history.

In Unit 6, I replaced my strict newcomer-scope check with the staff-provided `scope-feasible` check and re-graded the same candidates. The scout accepted HHS/simpler-grants-gov#12056 with a **moderate** difficulty note because the ellipsis-after-expansion behavior is still present, the issue identifies reproducible examples, and no assignee or open pull request is working on it. The issue fits my React and TypeScript experience while giving me practice working in a larger public-sector Next.js codebase. The scout also surfaced an important limitation: the issue combines the confirmed ellipsis bug with an unclear truncation-threshold request and a closed PR that mentions an internal design plan. Instead of silently treating that uncertainty as settled, I included it as a focused question in my claim and proposed leaving threshold behavior unchanged unless the maintainer directs otherwise.

**Claim comment:** https://github.com/HHS/simpler-grants-gov/issues/12056#issuecomment-6373805485

**Toolkit**

- Tool: `issue-scout`; I used its search strategy for an initial GitHub scan and direct grading mode for the issues selected from the course site.
- What it produced: It accepted issue #12056 after checking maintainer activity, repository use, current validity, scope feasibility, availability, and the repository's AI policy. It classified the issue as moderate difficulty because the core bug is bounded but the threshold requirement remains unclear.
- Difficulties: The initial search favored newly claimed issues, and the original rubric did not reliably distinguish active work from stale or already-completed issues. The first validity check also treated the issue's unresolved threshold request as blocking the entire issue even though the ellipsis behavior can be investigated separately.
- What changed: I allowed zero-comment search results and ranked them first, added an `Issue is still valid` check, and replaced `Newcomer-sized scope` with `scope-feasible` plus a non-binding difficulty note. I carried the remaining scope uncertainty into my claim as a question for the maintainer.

---

## Understanding the Issue

**Problem Description**

Opportunity descriptions are split into an initially visible portion and a hidden remainder. `ExpandableTextContent` permanently appends an ellipsis to the visible portion, so the ellipsis remains after the user expands the description and appears as an unnatural break between the two portions.

**Expected Behavior**

When the user selects `Show full description`, the hidden content should become visible and the truncation ellipsis should disappear. Collapsing the description again should restore the ellipsis. This contribution will not change the existing truncation thresholds or attempt to define new mobile and desktop behavior unless the maintainer provides different direction.

**Current Behavior**

Selecting `Show full description` reveals the remaining text and changes the control to `Hide full description`, but the original ellipsis remains visible on its own line between the initially displayed content and the revealed paragraph.

**Affected Components**

The behavior is primarily in `frontend/src/components/core/ExpandableTextContent.tsx`, which renders `preSplit + "..."`, and `frontend/src/components/core/ContentDisplayToggle.tsx`, which owns the expanded and collapsed state. Existing toggle behavior is covered in `ContentDisplayToggle.test.tsx`, while `ExpandableTextContent` currently has no dedicated component test, so the fix will require regression coverage for the ellipsis behavior.

**Toolkit**

- Tool: `repro-check` and the repository-local `CLAUDE.local.md`; I used them to validate the production reproduction and preserve a scoped map of the affected frontend components.
- What it produced: The tools connected the visible ellipsis behavior to `ExpandableTextContent` rendering the ellipsis separately from the expansion state owned by `ContentDisplayToggle`.
- Difficulties: The issue also describes inconsistent truncation thresholds and mobile/desktop behavior, but the public issue and closed PR #12389 do not settle those requirements.
- What changed: I narrowed the current contribution to the confirmed ellipsis-after-expansion behavior and recorded the threshold questions as deliberately out of scope unless the maintainer directs otherwise.
---

## Reproduction Process

**Environment Setup**

I reproduced the visible behavior on the deployed `https://simpler.grants.gov` production site using macOS 27.0.1 (build 26A434) and Google Chrome 155.0.8059.85. For code-path inspection, I forked and cloned `HHS/simpler-grants-gov` at default-branch commit `f0a1eeea0852c0fe1aa23da7a8878ceda2b0c934`. The frontend requires Node.js 24.19.0 or newer, while my machine initially had Node.js 20.12.1. I installed Homebrew's Node.js 24.22.1, used npm 11.19.1, and ran `npm ci` in `frontend/`, which added 1400 packages and audited 1401 packages. The visible reproduction used the production page rather than a locally running application.

**Steps to Reproduce**

1. Open https://simpler.grants.gov/opportunity/a9a78fab-33d8-4a34-a2de-c7571df8b19b in Chrome.
2. Scroll to the Description section and confirm that the collapsed description ends with a standalone ellipsis above the `Show full description` control.
3. Click `Show full description`.
4. Confirm that the remaining description becomes visible and the control changes to `Hide full description`.
5. Observe the join between the original bullet list and the newly revealed paragraph: the standalone ellipsis remains visible on its own line.

**Reproduction Evidence**

- **Commit showing reproduction:** none; the behavior was reproduced on the deployed production page without modifying repository code.
- **Screenshots/logs:** `issue-12056-before.png` shows the collapsed description with the standalone ellipsis, and `issue-12056-after.png` shows the expanded description with the same ellipsis still present between the bullet list and the revealed paragraph. The screenshots are retained locally and will be embedded in the pull request description.
- **My findings:** I reproduced the issue's core ellipsis-after-expansion behavior on the first URL supplied by the issue. Code inspection at commit `f0a1eeea0852c0fe1aa23da7a8878ceda2b0c934` showed that `ExpandableTextContent.tsx` renders `preSplit + "..."`, while `ContentDisplayToggle.tsx` separately owns the expanded-state value. This explains why the ellipsis does not currently react to expansion, but I have not yet selected a final fix approach. I did not treat the issue's truncation threshold or mobile/desktop parity statements as reproduced because the public issue and closed PR #12389 do not settle those requirements.

**Toolkit**

- Tool: `repro-check`; I used it first to grade the public claim draft and then to grade the claim together with the complete reproduction report, local screenshots, environment details, and code-path observation.
- What it produced: It verified that the steps reproduced the issue's exact visible behavior, checked the screenshots against the issue, confirmed the inspected commit and code path, and returned an `accept` verdict.
- Difficulties: The first full-package run rejected the evidence because the report named the two screenshot files without embedding them, so a reader could not verify the result from the package itself. My original environment paragraph also included dependency-install timing in a way that could imply the UI had been reproduced locally.
- What changed: I embedded both screenshots directly in `repro-draft.md`, removed the ambiguous install detail from that report, and explicitly stated that the visible reproduction occurred on the deployed production page while the local clone was used only for code inspection.
---

## Solution Approach

**Analysis**

`ExpandableTextContent.tsx` splits a long description into `preSplit` and `postSplit`. It renders the initial portion as `preSplit + "..."` and passes only `postSplit` into `ContentDisplayToggle`. The expanded-state value is internal to `ContentDisplayToggle.tsx`, so `ExpandableTextContent` cannot tell when the hidden content becomes visible and continues rendering the ellipsis in both states.

**Proposed Solution**

Add an optional state-change callback to `ContentDisplayToggle` so its existing internal toggle remains the source of truth while a parent can observe whether the content is expanded. In `ExpandableTextContent`, track the reported expanded state and append the ellipsis only while the remainder is collapsed. Preserve the existing toggle labels, accessibility attributes, split thresholds, short-description behavior, and mobile/desktop behavior. This change will not define a new truncation threshold or modify where the description is split.

The main implementation risk is that observing the toggle state requires `ExpandableTextContent` to become a client component. I will verify that this boundary change does not affect existing callers or server-rendered output. The maintainer has also not answered the scope question, and closed PR #12389 refers to non-public accessibility and machine-readability concerns, so the eventual PR will clearly describe the narrow scope and preserve the existing content, labels, accessibility attributes, and split behavior.

**Implementation Plan**

1. Update `frontend/src/components/core/ContentDisplayToggle.tsx` with an optional callback that reports the new visibility state whenever the toggle button is activated, without changing behavior for existing callers.
2. Update `frontend/src/components/core/ExpandableTextContent.tsx` as a client component, observe the reported visibility state, and render the ellipsis only when the additional content is collapsed.
3. Add regression coverage for a long expandable description, verifying that the ellipsis is present when collapsed, disappears after `Show full description` is selected, and returns after the description is collapsed again.
4. Run the targeted component tests followed by the frontend unit tests, lint, TypeScript check, and Prettier formatting check listed in `ship-manifest.md`.

**Toolkit**

- Tool: `plan-check`; I used it to grade `plan.md` together with a draft plan comment against the live issue, posted reproduction, repository rules, and current source.
- What it produced: The final run passed all six checks, confirming that the diagnosis follows from the reproduction, the scope is bounded, the callback approach addresses the state boundary, and the three-state regression test can prove the fix.
- Difficulties: The first run accepted the plan but failed the preferred risks-and-unknowns check because it did not record the unanswered scope question, the client-component boundary change, or the accessibility concerns mentioned in closed PR #12389.
- What changed: I identified `ExpandableTextContent` becoming a client component as an implementation risk, added four explicit risks and unknowns to `plan.md`, and recorded how the implementation will preserve existing callers, content, accessibility attributes, and split behavior.

---

## Testing Strategy

The before-fix evidence is the production reproduction from 2026-10-23: after selecting `Show full description`, the additional text appeared but the original ellipsis remained between the two portions of the description. The branch-level after-fix evidence is a new `ExpandableTextContent` component test that exercises the observable collapsed, expanded, and collapsed-again sequence through the real toggle control. It asserts that the ellipsis is present when collapsed, absent when expanded, and restored after re-collapsing. A second test confirms that content below the split threshold still renders without an ellipsis or toggle.

I also ran the repository checks recorded in `ship-manifest.md`. The targeted regression test passed 2 tests; the complete frontend suite passed 376 suites and 2992 tests with 12 existing skips; lint completed with 0 errors and 35 existing warnings; TypeScript generated route types and completed without errors; Prettier reported that all files matched; and `git diff --check` produced no output.

**Toolkit**

- Tool: `ship-manifest.md`, Jest, and `test_evidence.md`; I used the manifest to run the repository's required checks and saved the issue-specific before/after evidence and exact results for the ship-gate and PR description.
- What it produced: The targeted test proved the collapsed, expanded, and collapsed-again ellipsis behavior, while the full unit, lint, type, formatting, and whitespace checks verified the broader branch.
- Difficulties: The first complete test run after synchronizing with the latest upstream branch produced four unrelated validation-message failures because the local dependencies still matched the older lockfile.
- What changed: I ran `npm ci` against the current `package-lock.json` without modifying the unrelated tests, then reran the full suite and obtained 376 passing suites and 2992 passing tests.
---

## Implementation Notes

**2026-10-23**

I claimed issue #12056 after confirming that it was open, unassigned, and had no competing claim or open pull request. I reproduced the ellipsis-after-expansion behavior on the deployed production site and traced the relevant rendering boundary between `ExpandableTextContent` and `ContentDisplayToggle`.

**2026-10-25**

I posted the promised reproduction update within the three-day reporting window: https://github.com/HHS/simpler-grants-gov/issues/12056#issuecomment-6403510277. The update records the production environment, reproduction steps, expected and actual behavior, and the relevant component boundary. I kept the full screenshots and detailed report for the pull request rather than posting the complete reproduction package on the issue.

**2026-11-02**

I synchronized my fork with the latest upstream `main` and created `jh0619/12056-remove-expanded-ellipsis`. Plan-check first accepted the callback approach but identified an unrecorded client-component risk, so I updated the plan before implementation. On the branch, I added an optional visibility callback to `ContentDisplayToggle`, used the reported state in `ExpandableTextContent` to conditionally render the ellipsis, and added a dedicated regression test covering collapsed, expanded, collapsed-again, and short-content behavior. The targeted test and all four manifest checks passed after reinstalling dependencies for the updated lockfile.

**2026-11-06**

I installed the PR monitor and wrote a response playbook based on the repository's contribution rules and five recently merged external PRs. Their first-maintainer-response times ranged from 0.8 to 32.2 days, so I set a 21-quiet-day threshold for an initial nudge. The monitor found PR #13116 mergeable but still awaiting review after 3 quiet days and drafted a wait note. I kept the note private and posted nothing because the threshold had not been met.

**Toolkit**

- Tool: Voice Guard, `repro-check`, `plan-check`, Claude Code with `CLAUDE.local.md`, and the ship-gate manifest; I used them across the claim, reproduction, planning, implementation, and testing stages.
- What it produced: The tools produced an accepted public update, an accepted implementation plan, a focused three-file diff, a new regression test, and observable passing results for the targeted and full repository checks.
- Difficulties: The first plan omitted the client-component risk, a generated public-update draft duplicated content, and the first full branch test used stale dependencies after the upstream sync.
- What changed: I added explicit plan risks, rejected the duplicated draft, kept the implementation inside the accepted scope, ran `npm ci` for the current lockfile, and reran the complete checks successfully.

---

## Pull Request

**PR Link:** https://github.com/HHS/simpler-grants-gov/pull/13116

**PR Description:** The PR explains that the persistent ellipsis came from `ExpandableTextContent` rendering it unconditionally while `ContentDisplayToggle` separately owned the visibility state. It proposes an optional visibility callback, conditionally renders the ellipsis only while collapsed, and adds regression tests for collapsed, expanded, collapsed-again, and short-content behavior. The description uses `Work for #12056` and identifies the PR as a partial fix because it deliberately leaves the unresolved character thresholds and mobile/desktop requirements unchanged. It also records the targeted and complete frontend check results and discloses how AI assistance was used.

**Maintainer Feedback:**

- 2026-11-06: None yet. The PR monitor confirmed that PR #13116 has been quiet for 3 days with no comments or reviews, so I am continuing to wait.

**Status:** Awaiting review

**Toolkit**

- Tool: `ship-gate`, its repository-specific `ship-manifest.md`, and `pr-precheck`; I used them to run the project's checks and review the branch, evidence, and PR description before opening the PR through Claude Code.
- What it produced: The manifest ran four successful repository checks, and the final pr-precheck passed plan fidelity, description accuracy, observable evidence, focused diff, repository standards, and reviewer context before PR #13116 opened.
- Difficulties: The first gate run rejected `Fixes #12056` because this PR does not resolve every requirement in the issue, and the second rejected the draft because it did not tag one of the repository's designated maintainers.
- What changed: I replaced `Fixes` with `Work for`, stated clearly that the change is a partial fix, explained which requirements remain unresolved, and added `@serverless-mom`. Claude Code later appended its attribution line while posting, without changing the implementation.

---

## Learnings and Reflections

**Technical Skills Gained**

I learned how to trace a UI defect across a React parent-child state boundary instead of treating the visible symptom as an isolated string change. I added an optional callback without changing existing callers, converted `ExpandableTextContent` into a client component so it could observe the toggle state, and wrote a Jest/React Testing Library regression test that exercises collapsed, expanded, and collapsed-again behavior through the real control. I also practiced the complete external-contribution workflow: forking and synchronizing an upstream repository, following its branch and PR conventions, running repository-specific checks, and opening a focused PR from a fork.

**Challenges Overcome**

The issue mixed a confirmed ellipsis defect with unresolved truncation-threshold and mobile/desktop requirements. I handled that uncertainty by reproducing only what I could verify, asking the maintainer a focused scope question, keeping the threshold work out of the implementation, and changing the PR from `Fixes #12056` to `Work for #12056` when pr-precheck showed that the original wording would close a partially unresolved issue. I also encountered four unrelated unit-test failures after updating from upstream; rather than editing unrelated tests, I recognized that `node_modules` was stale, ran `npm ci` against the current lockfile, and confirmed that all 376 suites passed. Finally, ship-gate caught the missing designated-maintainer tag before the PR opened, so I corrected the description and reran the complete gate successfully.

**What I'd Do Differently Next Time**

I would compare every expected-result bullet with my proposed scope before drafting the PR, which would have led me to use `Work for` from the beginning. I would also extract the repository's reviewer-assignment rule into my local memory file earlier, reinstall dependencies immediately after synchronizing a changed lockfile, and record the client-component boundary as a risk during the first planning pass. These changes would reduce repeated gate runs without weakening the checks that caught real problems.

---

## Resources Used

- [Issue #12056 and its reproduction examples](https://github.com/HHS/simpler-grants-gov/issues/12056) — defined the visible behavior and supplied the production opportunity URLs I used.
- [Simpler Grants CONTRIBUTING.md](https://github.com/HHS/simpler-grants-gov/blob/main/CONTRIBUTING.md) — provided the external-contributor workflow, PR title convention, AI policy, and designated-reviewer requirement.
- [Frontend development guide](https://github.com/HHS/simpler-grants-gov/blob/main/documentation/frontend/development.md) — provided the required Node version and the unit-test, lint, TypeScript, and formatting commands.
- [Closed PR #12389](https://github.com/HHS/simpler-grants-gov/pull/12389) — exposed the unresolved internal design, accessibility, and machine-readability concerns that led me to keep the contribution narrowly scoped.
