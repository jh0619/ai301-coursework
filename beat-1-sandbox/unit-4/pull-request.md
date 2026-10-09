# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/116

**Branch**

`fix/72-handle-malformed-password-hash`

## Eval iterations

**Run history**

1. `agreement: 2/3 scored items`
2. `agreement: 3/3 scored items`
3. `agreement: 17/20 scored items (bar: 18/20: below the bar)`
4. `agreement: 3/6 scored items`
5. `agreement: 6/6 scored items`
6. `agreement: 20/20 scored items (bar: 18/20: PASS)`

**Package analysis**

I analyzed `pkg-04`, whose category was `not-tested`. My rubric decided `reject`, and the gold label was also `reject`.

The package's Test evidence said only:

> Tested locally on Windows 11 with less-Windows: colors work now in paging mode. `cargo test` passes.

Although the diff contained a targeted unit test, the evidence did not include a concrete command transcript, test count, named passing result, exit code, or other observable result showing that the issue-specific behavior was exercised. Under my `Test evidence is observable` check, unsupported statements such as `"tested locally"`, `"works now"`, and `"tests pass"` are insufficient. Therefore, the required check failed and the verdict was `reject`.

**Check rationale**

The final check in `rubric.md` reads:

> **Check:** Test evidence is observable
> **Evidence:** Compare the original reproduction and the plan's expected-after behavior with the candidate diff's regression assertions and the Test evidence section.
> **Pass condition:** Pass when the package gives concrete, issue-specific evidence that the target behavior was exercised after the change and produced the expected result. Decisive evidence may be direct output, a before/after transcript, an interactive observation, a command transcript with specific result comments, or a targeted regression assertion combined with a concrete result from the command or suite that ran it. A concrete result can be an exit code, matched or omitted value, visible behavior, named passing case, or test count. Once the core issue behavior is demonstrated, summarized results for supporting controls or broader suites are sufficient; do not require raw output for every plan item. Fail when the package gives only unsupported prose such as "tested locally," "works now," or "tests pass," when no command or interaction is tied to the claimed result, or when the evidence does not exercise the issue's target behavior.
> **Weight:** required

I revised this check after the first full run scored `17/20`. The earlier version treated raw terminal output too narrowly and rejected valid clear-accept packages such as `pkg-11`, `pkg-13`, and `pkg-19`. Those packages used concrete interactive observations, command-result comments, targeted regression assertions, or specific results tied to commands.

I changed the check to accept multiple forms of observable, issue-specific proof while retaining a boundary against unsupported statements. I rejected the simpler alternative of accepting any statement that a test suite passed, because a broad `"tests pass"` claim does not prove that the issue's target behavior was exercised.

**Trade-offs**

After loosening the test-evidence check, I reran this canary set:

```text
pkg-02,pkg-04,pkg-07,pkg-11,pkg-13,pkg-19