# Plan for Issue #72: Handle malformed stored password hashes

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

## Reproduction evidence

I reproduced the issue at commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088` using Python 3.11.3, passlib 1.7.4, and bcrypt 4.3.0 on macOS 27.0 (`arm64`).

Running the existing issue-specific test produced the expected failure marker:

```text
XFAIL tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
- issue #72 (manifest H-05): password verify raises UnknownHashError instead of returning False
```

Calling the function directly with the malformed hash used by the test:

```bash
.venv/bin/python -c 'from core.security import verify_password; print(verify_password("password", "not_a_valid_bcrypt_hash"))'
```

produced:

```text
File "core/security.py", line 37, in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
...
passlib.exc.UnknownHashError: hash could not be identified
```

As a control, verifying an incorrect password against a valid bcrypt hash returned `False`.

This evidence establishes that `pwd_context.verify()` raises `UnknownHashError` when it cannot identify the stored hash format and that the exception currently escapes from `verify_password()`.

## Diagnosis

`verify_password()` directly returns the result of `pwd_context.verify()` without handling the documented failure raised for an unrecognized hash format.

The reproduction traceback reaches the return statement in `core/security.py` and ends with `passlib.exc.UnknownHashError`. Because the function does not catch that exception, a malformed stored hash interrupts authentication instead of following the function's boolean contract and failing closed with `False`.

The evidence supports the failure boundary at the call to `pwd_context.verify()`. I do not need to change how passlib identifies hashes or how bcrypt hashes are generated.

## Scope

The change will be limited to the malformed-hash behavior of `verify_password()`.

In scope:

- Catch the specific `passlib.exc.UnknownHashError` raised for an unrecognized stored hash.
- Return `False` for that case.
- Keep the existing behavior for valid hashes and incorrect passwords.
- Enable the existing issue #72 regression test by removing its strict `xfail` marker after the implementation makes it pass.

Out of scope:

- Changing the configured password-hashing scheme.
- Upgrading or replacing passlib or bcrypt.
- Catching unrelated exceptions from password verification.
- Refactoring authentication or JWT behavior.
- Addressing the separate passlib/bcrypt backend-version warning.

## Files to change

### `core/security.py`

- Import `UnknownHashError` from `passlib.exc`.
- Wrap the call to `pwd_context.verify()` in a narrow `try`/`except UnknownHashError`.
- Return `False` when passlib cannot identify the stored hash format.
- Preserve the existing boolean result for recognized hashes.

### `tests/unit/test_security.py`

- Remove the issue #72 `@pytest.mark.xfail(...)` marker from `test_verify_with_wrong_hash_format`.
- Keep the test input and assertion so the test becomes a normal regression test requiring malformed hashes to return `False`.

No other source, configuration, dependency, or test files are expected to change.

## Implementation approach

1. Add the specific `UnknownHashError` import from passlib.
2. Keep `pwd_context.verify(plain_password, hashed_password)` as the normal verification path.
3. Catch only `UnknownHashError` around that call.
4. Return `False` from the exception path so an unrecognized stored hash fails closed.
5. Remove the issue-specific strict `xfail` marker once the regression test passes normally.
6. Review the diff to verify that the change does not alter password hashing, token handling, or other authentication behavior.

I will not catch the base `Exception` class because that could hide unrelated operational or programming errors.

## Test plan

### Issue-specific regression test

Run:

```bash
.venv/bin/pytest \
  tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format \
  -vv
```

Expected after the fix:

```text
1 passed
```

The test must pass normally rather than appear as `XFAIL` or `XPASS(strict)`.

### Original direct reproduction

Run:

```bash
.venv/bin/python -c 'from core.security import verify_password; print(verify_password("password", "not_a_valid_bcrypt_hash"))'
```

Expected after the fix:

```text
False
```

No `UnknownHashError` traceback should be produced.

### Existing-behavior control

Run:

```bash
.venv/bin/python -c 'from core.security import hash_password, verify_password; hashed = hash_password("correct_password"); print(verify_password("wrong_password", hashed))'
```

Expected:

```text
False
```

This checks that a recognized bcrypt hash with an incorrect password continues to return `False`.

### Security unit-test regression check

Run:

```bash
.venv/bin/pytest tests/unit/test_security.py -vv
```

Expected: all security unit tests pass with no issue #72 `XFAIL` or strict `XPASS`.

### Focused quality checks

Run:

```bash
.venv/bin/ruff check core/security.py tests/unit/test_security.py
.venv/bin/black --check core/security.py tests/unit/test_security.py
.venv/bin/mypy core/security.py
```

Expected: all commands complete successfully without new findings.

## Risks and unknowns

The main risk is catching too broad an exception and hiding failures unrelated to malformed hash formats. I will avoid that by catching only `UnknownHashError`.

Returning `False` intentionally makes an unrecognized hash indistinguishable from an incorrect password to callers. That is the fail-closed behavior requested by the issue and avoids exposing hash-format details through the authentication result.

The passlib/bcrypt backend-version warning observed during the control run is separate from `UnknownHashError` and will remain out of scope.

## Deviations

The implementation followed the posted plan with no deviations. I limited the source change to catching `UnknownHashError` in `verify_password()` and returning `False`, and I removed only the issue #72 strict `xfail` marker while preserving the regression test's input and assertion.

I did not change the hashing configuration, dependencies, JWT behavior, or handling of unrelated exceptions. The original reproduction now returns `False` without an `UnknownHashError` traceback, the issue-specific test passes normally, all 25 tests in `tests/unit/test_security.py` pass, and the focused Ruff, Black, and mypy checks succeed. The pre-existing passlib/bcrypt backend-version warning remains outside the scope of this change.