# Voice guide: how I talk upstream

## Who I am in threads

I am a graduate computer science student with experience in Python backend development, APIs, debugging, and automated testing. I am a new contributor in this repository, so I communicate what I tested, show the evidence, and avoid pretending that I already understand parts of the codebase I have not investigated. Maintainers can expect focused updates and reproducible details from me.

## Rules I write by

### Rule: Promise investigation, not a fix

Before reproducing an issue, I say what I plan to investigate and what evidence I will report. I do not promise a fix, a pull request, or a completion date before I understand the problem.

- Wrong: "I will fix this issue and open a PR by tomorrow."
- Right: "I'd like to investigate this issue, reproduce the reported behavior, and share the environment, steps, and observed output here."

### Rule: Name the specific behavior

I refer to the exact behavior described in the issue instead of posting a generic claim that could apply to any issue.

- Wrong: "I would like to work on this."
- Right: "I'd like to investigate why `verify_password` raises `UnknownHashError` for a malformed stored hash instead of returning `False`."

### Rule: Separate evidence from inference

I clearly distinguish what the commands and artifacts demonstrate from what I think may be causing the behavior. I do not present a possible explanation as a confirmed root cause.

- Wrong: "The missing exception handler is definitely the root cause."
- Right: "The observed stack trace shows that `UnknownHashError` escapes from password verification. I have not yet confirmed whether additional code paths are affected."

### Rule: Make the result independently verifiable

When reporting a reproduction, I include the relevant environment, exact command or actions, input, expected behavior, actual behavior, and supporting output. I do not rely on another contributor's report as my evidence.

- Wrong: "I reproduced the same problem as the previous comment."
- Right: "On Python 3.13 at commit `<SHA>`, I ran `<command>` with `<input>`. I expected `False`, but observed `UnknownHashError`; the relevant output is included below."

### Rule: State uncertainty honestly

If I cannot reproduce the issue or the evidence is incomplete, I say so directly and describe what I tested. I do not force the result into a confident reproduction.

- Wrong: "The bug is confirmed," when the command failed during setup.
- Right: "I could not reproduce the reported behavior because the application failed during dependency setup. The setup error and the commands I ran are included below."

## Things I never post

- A promise to fix the issue or submit a pull request before I understand the problem.
- A deadline or completion estimate that the maintainer did not request.
- A claim that I reproduced the issue without direct supporting evidence.
- A root-cause statement based only on an assumption.
- "Same as above" or "I can confirm" without my own environment, steps, and output.
- Logs, tokens, credentials, personal information, or other sensitive data.
- AI-generated wording that I have not reviewed, tested, and understood.
- A comment that omits an AI-use disclosure when the repository explicitly requires one.
