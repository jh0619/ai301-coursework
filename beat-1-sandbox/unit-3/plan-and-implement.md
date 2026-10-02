# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

---

## Posted upstream

**GitHub username**

jh0619

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5961751088

I reproduced the issue on commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088`. Calling `verify_password()` with the malformed hash from the existing regression test reaches `pwd_context.verify()` and raises `passlib.exc.UnknownHashError` instead of returning `False`.

My plan is to make a narrow change in `core/security.py`: catch only `UnknownHashError` around the passlib verification call and return `False` for an unrecognized stored hash. I will keep the existing behavior for recognized hashes and will not change the hashing configuration, dependencies, JWT behavior, or catch unrelated exceptions.

In `tests/unit/test_security.py`, I will remove the issue #72 strict `xfail` marker while keeping its malformed-hash assertion as a normal regression test.

I will verify the change by:

- running the issue-specific test and confirming it passes normally rather than reporting `XFAIL` or `XPASS`;
- rerunning the original direct reproduction and confirming it prints `False` without an `UnknownHashError` traceback;
- checking that an incorrect password against a valid bcrypt hash still returns `False`;
- running the full security unit-test file and the focused Ruff, Black, and mypy checks.

The separate passlib/bcrypt backend-version warning is outside this issue's scope.

---

## Your branch

**Branch**

`fix/72-handle-malformed-password-hash`

**Evidence**

### Before the change

#### Issue-specific regression test

Command:

```bash
.venv/bin/pytest \
  tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format \
  -vv -rxX
```

Output:

```text
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format XFAIL [100%]

XFAIL tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
- issue #72 (manifest H-05): password verify raises UnknownHashError instead of returning False

1 xfailed, 2 warnings in 0.75s
```

#### Direct reproduction

Command:

```bash
.venv/bin/python -c 'from core.security import verify_password; print(verify_password("password", "not_a_valid_bcrypt_hash"))'
```

Output:

```text
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/Users/jiahou/Desktop/AI301/pathreview-ai301-fa26-s3/core/security.py", line 37, in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/jiahou/Desktop/AI301/pathreview-ai301-fa26-s3/.venv/lib/python3.11/site-packages/passlib/context.py", line 2343, in verify
    record = self._get_or_identify_record(hash, scheme, category)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/jiahou/Desktop/AI301/pathreview-ai301-fa26-s3/.venv/lib/python3.11/site-packages/passlib/context.py", line 2031, in _get_or_identify_record
    return self._identify_record(hash, category)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/jiahou/Desktop/AI301/pathreview-ai301-fa26-s3/.venv/lib/python3.11/site-packages/passlib/context.py", line 1132, in identify_record
    raise exc.UnknownHashError("hash could not be identified")
passlib.exc.UnknownHashError: hash could not be identified
```

#### Existing-behavior control

Command:

```bash
.venv/bin/python -c 'from core.security import hash_password, verify_password; hashed = hash_password("correct_password"); print(verify_password("wrong_password", hashed))'
```

Final output:

```text
False
```

The command also emitted the pre-existing passlib/bcrypt backend-version warning before returning `False`.

### After the change

#### Issue-specific regression test

Command:

```bash
.venv/bin/pytest \
  tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format \
  -vv
```

Output:

```text
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED [100%]

1 passed, 2 warnings in 0.18s
```

The test now passes normally, with no `XFAIL` or `XPASS`.

#### Direct reproduction

Command:

```bash
.venv/bin/python -c 'from core.security import verify_password; print(verify_password("password", "not_a_valid_bcrypt_hash"))'
```

Output:

```text
False
```

The malformed hash now returns `False` without an `UnknownHashError` traceback.

#### Existing-behavior control

Command:

```bash
.venv/bin/python -c 'from core.security import hash_password, verify_password; hashed = hash_password("correct_password"); print(verify_password("wrong_password", hashed))'
```

Final output:

```text
False
```

The pre-existing passlib/bcrypt backend-version warning still appeared, but the recognized hash with an incorrect password continued to return `False`.

#### Full security unit-test file

Command:

```bash
.venv/bin/pytest tests/unit/test_security.py -vv
```

Output:

```text
collected 25 items

tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format PASSED [88%]

25 passed, 2 warnings in 4.61s
```

#### Focused quality checks

Commands:

```bash
.venv/bin/ruff check core/security.py tests/unit/test_security.py
.venv/bin/black --check core/security.py tests/unit/test_security.py
.venv/bin/mypy core/security.py
```

Output:

```text
All checks passed!

All done! ✨ 🍰 ✨
2 files would be left unchanged.

Success: no issues found in 1 source file
```

---

## Eval iterations

**Run history**

1. Smoke run:

   ```text
   agreement: 3/3 scored items
   ```

2. First full run:

   ```text
   agreement: 18/20 scored items  (bar: 18/20: PASS)
   ```

3. Targeted revision and canary run:

   ```text
   agreement: 6/6 scored items
   ```

4. Final full run:

   ```text
   agreement: 20/20 scored items  (bar: 18/20: PASS)
   ```

**Package analysis**

I analyzed `pkg-20`. In my first full run, the result was:

```text
pkg-20  thread-convention  reject  accept  NO  graded accept
```

The gold label was `reject`, but my rubric initially decided `accept`. The plan was technically detailed and followed the maintainer's performance direction, so the diagnosis, scope, approach, and test checks passed. However, the repository facts stated that all AI usage must be disclosed in comments, including the tool used and the extent of assistance. The candidate plan comment contained no such disclosure.

My original communication check mentioned disclosure requirements, but it did not make sufficiently explicit that every required element had to appear in the exact draft comment. It also did not explicitly prevent references to AI elsewhere in the issue or thread from being treated as the contributor's disclosure.

I tightened the rubric, procedure, and evidence guide to require the exact draft comment to contain every policy-required disclosure element. After that revision, the targeted run produced:

```text
pkg-20  thread-convention  reject  reject  yes
```

The final full run also rejected `pkg-20`, matching the gold label.

**Check rationale**

My current check reads:

> **Check:** Plan comment is accurate and compliant  
> **Evidence:** Read the exact draft plan comment against the full plan, issue description, thread highlights, repo-facts contribution policy, and applicable repository instructions.  
> **Pass condition:** Pass if the comment accurately summarizes the diagnosis, bounded scope, approach, and verification plan; does not promise an unverified result or completion date; does not conflict with maintainer guidance; and follows every stated communication and disclosure requirement. If the repository requires AI-use disclosure, the exact draft comment must contain every required element, including the tool and extent of assistance when the policy asks for them. A mention of AI elsewhere in the issue, thread, plan, or package does not count as the contributor's disclosure. If no disclosure rule exists, absence of disclosure does not fail this check.  
> **Weight:** required

I revised the check into this form after the first full run incorrectly accepted `pkg-20`. The current wording identifies the exact evidence source—the draft comment—and prevents the grader from assuming that disclosure will be added later or treating a maintainer's reference to AI as the contributor's disclosure. It also avoids penalizing plans in repositories, such as the PathReview repository, that do not require AI disclosure.

**Trade-offs**

This stricter check can reject a technically sound and buildable plan solely because the draft comment omits a required communication or disclosure element. I accept that trade-off because the assignment evaluates whether a plan is ready to post, not only whether its implementation could work. A plan comment that violates the repository's explicit policy is not ready to post.

I reran the changed behavior with both target packages and canaries:

```text
pkg-02  clear-accept       accept  accept  yes
pkg-04  thread-convention  reject  reject  yes
pkg-10  unbuildable        reject  reject  yes
pkg-14  clear-accept       accept  accept  yes
pkg-17  unbuildable        reject  reject  yes
pkg-20  thread-convention  reject  reject  yes

agreement: 6/6 scored items
```

`pkg-20` changed to the correct rejection, while `pkg-04` remained rejected for its thread/convention problem. The clear-accept and unbuildable canaries also retained their correct verdicts. The final full run reached `20/20`.

---

Related paths: `plan.md` and `eval-run.txt` are in this directory; the skill files are in `tools/plan-check/`.
