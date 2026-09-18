# Unit 1 — Issue Selection

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

**Verdict output**

Graded live against the rubric in `/Users/jiahou/.claude/skills/issue-select/rubric.md`. The candidate is inside the scoped repo (`codepath/pathreview-ai301-fa26-s3`), so it's a valid candidate. Today is 2026-09-18, so the 90-day window opens 2026-06-20 and the 30-day claim window opens 2026-08-19.

Issue #72 — `verify_password` raises `UnknownHashError` on malformed stored hashes instead of returning `False` (labels: bug, good first issue, api, tier-1)

- Maintainer active — pass. Aburke225 (`author_association: COLLABORATOR`) opened the issue on 2026-09-10 and pushed three default-branch commits on 2026-09-16, both inside 90 days; neither is a bot.
- Repository in use — pass. `archived: false`, and 3 of the last 5 default-branch commits are dated 2026-09-16 (the other two 2026-08-24) — all human-authored, none dependency bumps.
- Newcomer-sized scope — pass. The body names a concrete outcome ("Verification against a malformed hash should fail closed (return False), not raise"), an actionable direction and the two files (`core/security.py`, `tests/unit/test_security.py`), plus the xfail marker for manifest id H-05 to remove. Zero linked PRs, so nowhere near the 2-closed-PR fail bar, and no maintainer statement of a blocking decision.
- Available to claim — pass. `assignees: []`, `comments: 0`, timeline holds only four labeled events from the opener, and a repo-wide PR listing plus a search for PRs referencing 72 both return nothing — no claim, no open linked PR. (The Path Review house rule would have made classmate claims harmless anyway; there are none.)
- AI contribution policy — pass. `docs/CONTRIBUTING.md` (189 lines) is the only contribution policy; there is no `AI_POLICY.md`/`AI_USAGE_POLICY.md`, and a case-insensitive grep for AI/LLM/Copilot/generative/ChatGPT/Claude across it returns nothing. Silence, not a prohibition. The PR template's only demands are green CI and removing the seeded bug's xfail marker — conditions, not a ban.

Every required check passes, so the verdict is `accept`. One caveat about the ranking half of the skill: the fit-profile section of `scope.md` is still the placeholder (`Write a few sentences here.`), so I have nothing to rank against. That doesn't affect this verdict — fit never changes one, and there's only one candidate — but fill it in before you bring several issues at once. Worth knowing for when you start: the fix is a `try/except UnknownHashError` returning `False` in `core/security.py`, and CONTRIBUTING's "Working on a seeded bug: remove its xfail marker" section tells you to also clear any matching suppression in `pyproject.toml`.

```json
{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
  "checks": [
    {
      "name": "Maintainer active",
      "grade": "pass",
      "evidence": "Collaborator Aburke225 opened the issue 2026-09-10 and pushed default-branch commits on 2026-09-16, both within 90 days; not a bot."
    },
    {
      "name": "Repository in use",
      "grade": "pass",
      "evidence": "archived: false; last 5 default-branch commits are 2026-09-16 (x3) and 2026-08-24 (x2), all human-authored, so >=2 fall inside 90 days."
    },
    {
      "name": "Newcomer-sized scope",
      "grade": "pass",
      "evidence": "Body states the outcome ('should fail closed (return False), not raise'), names core/security.py and tests/unit/test_security.py, and lists zero linked PRs."
    },
    {
      "name": "Available to claim",
      "grade": "pass",
      "evidence": "assignees: [], comments: 0, timeline holds only 4 'labeled' events, and repo PR listing plus search for PRs referencing 72 return none."
    },
    {
      "name": "AI contribution policy",
      "grade": "pass",
      "evidence": "docs/CONTRIBUTING.md is the only policy and a grep for AI/LLM/Copilot/generative/ChatGPT/Claude matches nothing; no AI_POLICY.md exists."
    }
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

**Run history**

1. `agreement: 2/3 scored items`
2. `agreement: 15/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in policy)`
3. `agreement: 1/5 scored items`
4. `agreement: 3/4 scored items`
5. `agreement: 1/1 scored items`
6. `agreement: 18/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

For `issue-12`, my rubric decided `reject`, and the gold label was also `reject`. The repository was active, the issue was unassigned, and its implementation scope appeared manageable. However, the repository's contribution policy explicitly stated that it does not accept AI-generated code or documentation. My `AI contribution policy` check therefore failed the issue. Because that check is required, the final verdict was `reject`.

**Check rationale**

The current wording of my `AI contribution policy` check is:

> Pass if the policy allows AI-assisted contributions or contains no restriction on AI use. Fail if the policy explicitly prohibits AI-generated code or documentation. Requirements that contributors understand, review, explain, and test AI-assisted work still pass.

I added this check after the first full eval run received no matches in the policy category. I made the check fail only on an explicit prohibition because a repository that says nothing about AI has not provided evidence that AI-assisted contributions are forbidden. Policies requiring contributors to understand and test their work still pass because those requirements do not prevent a responsible AI-assisted contribution.

**Trade-offs**

This check rejects an otherwise active, clearly scoped, and unclaimed issue when its repository explicitly prohibits AI-generated contributions, as happened with `issue-12`. It may also reject an issue that I could complete without AI assistance because the rubric evaluates whether the issue is suitable for this AI-assisted course workflow rather than whether any human contributor could work on it. I accepted that trade-off because respecting an explicit repository policy is more important than maximizing the number of accepted issues.

---

## Selection rationale

**Selection rationale**

1. This issue fits my interest in Python backend development and testing. Its expected behavior is concrete: password verification should return `False` for an unrecognized stored-hash format instead of allowing `UnknownHashError` to escape. The estimated effort is only one to two hours, and the change is limited to `core/security.py` and `tests/unit/test_security.py`, so it fits the time available for a first contribution.

2. The verdict correctly identified that the repository is active, the issue has no assignee or linked pull request, AI-assisted contributions are not prohibited, and the task has a narrow and testable outcome. Beyond the rubric, I also considered my familiarity with Python backend code, exception handling, and unit tests. I preferred this issue over the RAG parser candidates because it has less ambiguity about the expected output and should be easier to verify.

3. I anticipate that claiming it will be straightforward because it currently has no assignee, comments, active claim statements, branches, or linked pull requests. The main difficulty is that it is labeled `good first issue` and `tier-1`, so another student could select it before Unit 2. I will verify its status again before posting the claim comment required by the next unit.
