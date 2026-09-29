# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

---

## Your identity upstream

**GitHub username**

jh0619

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5880998229

Hi! I'd like to investigate why `verify_password` raises `UnknownHashError` for a malformed stored hash instead of returning `False`. I'll reproduce the behavior in a documented environment and share the exact steps, command, and observed output here before making any code changes.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5881281829

I reproduced this issue without making any source-code changes.

### Environment

- Commit: `2f4e82f52efbcfcc57d65b3fa5348672163ca088`
- macOS 27.0 (`arm64`)
- Python 3.11.3
- passlib 1.7.4
- bcrypt 4.3.0
- pytest 9.1.1

No `.env`, Docker services, database, or frontend setup was required for this unit-level reproduction.

### Steps

From the repository root, I created a virtual environment and installed the project dependencies:

```bash
python3.11 -m venv .venv
.venv/bin/python -m pip install --upgrade pip setuptools wheel
.venv/bin/pip install -e ".[dev]"
```

I ran the existing test associated with issue #72:

```bash
.venv/bin/pytest \
  tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format \
  -vv -rxX
```

The test produced:

```text
XFAIL tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
- issue #72 (manifest H-05): password verify raises UnknownHashError instead of returning False

1 xfailed, 2 warnings in 0.75s
```

I then invoked `verify_password()` directly with the malformed hash used by the test:

```bash
.venv/bin/python -c 'from core.security import verify_password; print(verify_password("password", "not_a_valid_bcrypt_hash"))'
```

The call raised:

```text
File "core/security.py", line 37, in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
...
passlib.exc.UnknownHashError: hash could not be identified
```

As a control, I tested an incorrect password against a valid bcrypt hash:

```bash
.venv/bin/python -c 'from core.security import hash_password, verify_password; hashed = hash_password("correct_password"); print(verify_password("wrong_password", hashed))'
```

The final output was:

```text
False
```

### Expected

A malformed or unrecognized stored hash should fail closed and cause `verify_password()` to return `False`.

### Actual

The malformed hash causes `passlib.exc.UnknownHashError` to escape from `pwd_context.verify()`.

This matches the behavior described in issue #72. The control also shows that a valid hash with an incorrect password already returns `False`.

---

## Eval iterations

**Run history**

1. Smoke run: `agreement: 3/3 scored items`
2. First full run: `agreement: 17/20 scored items (bar: 18/20: below the bar)`
3. Targeted revision run: `agreement: 7/7 scored items`
4. Final full run: `agreement: 20/20 scored items (bar: 18/20: PASS)`

**Package analysis**

I analyzed `pkg-05`. In my first full run, the result was:

```text
pkg-05  accept  reject  NO  failed: Steps are followable
```

My rubric decided `reject`, while the gold label was `accept`. The package described creating a minimal `env.yml` with a valid `dependencies:` list and an unsupported `category:` section, but it did not include the complete file contents. My original interpretation of the steps check treated that omission as requiring the reader to guess an essential input.

The package nevertheless described the relevant characteristics precisely enough for another contributor to construct an equivalent file and trigger the same behavior. I revised the check so that equivalent inputs pass when their relevant properties are clear. After that revision, the targeted run produced:

```text
pkg-05  accept  accept  yes
```

**Check rationale**

My current `Steps are followable` check reads:

> **Check:** Steps are followable  
> **Evidence:** Read the repro report's setup instructions, commands, inputs, and referenced files in the order presented.  
> **Pass condition:** Pass if another contributor could recreate the same reproduction attempt without guessing an essential action or trigger condition. Exact file contents or byte-for-byte inputs are not required when the report describes the relevant characteristics clearly enough to construct an equivalent input. Fail if a missing command, input property, configuration, or setup action prevents the attempted behavior from being tested.  
> **Weight:** required

I added the sentence allowing equivalent inputs because the first version rejected `pkg-05` even though its description of the input was sufficient to reproduce the behavior. The current wording still requires every essential action and trigger condition, but it evaluates whether the experiment can be repeated instead of requiring a specific presentation format or byte-for-byte fixture.

**Trade-offs**

The revised check may accept a report that does not include an exact fixture, so long as it explains the fixture's relevant characteristics well enough to recreate an equivalent input. This gives up some byte-for-byte precision in exchange for accepting reproducible reports such as `pkg-05`.

To check that this change did not allow bad packages through, I reran the affected packages together with rejecting canaries:

```text
pkg-02  reject  reject  yes
pkg-05  accept  accept  yes
pkg-09  accept  accept  yes
pkg-10  accept  accept  yes
pkg-13  reject  reject  yes
pkg-14  reject  reject  yes
pkg-18  reject  reject  yes

agreement: 7/7 scored items
```

The canaries remained rejected, and the final full run reached `20/20`. The separate required `Evidence matches the issue` check also prevents a merely followable experiment from passing when its output does not demonstrate the issue's behavior.

---

Related paths: `eval-run.txt` in this directory; the skill files are in `tools/repro-check/`.
