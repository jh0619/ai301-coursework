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
**Pull Request:** pending

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

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

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

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

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

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
