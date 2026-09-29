# Evidence guide: where proof lives in a reproduction package

## Environment

**Where it lives**

In an eval package, inspect the repro report's environment record, the issue context, and the repo-facts block. In live mode, inspect the student's draft repro comment, the repository revision being tested, and the repository's setup or contribution documentation.

**What good looks like**

The package identifies the tested repository revision and the operating system, runtime, application, dependency, and configuration details that could affect the reported behavior. The versions should match the issue's target environment, or any relevant difference must be stated explicitly. Unrelated system details are not required.

## Steps

**Where it lives**

In an eval package, inspect the repro report's setup instructions, ordered actions, commands, inputs, configuration values, and referenced files. In live mode, inspect the draft repro comment together with the repository's documented setup instructions.

**What good looks like**

A stranger can recreate the same reproduction attempt from the stated environment, commands, actions, and input characteristics without guessing an essential action or trigger condition. Exact file contents or byte-for-byte inputs are not required when the report describes the relevant properties clearly enough to construct an equivalent input.

## Behavior shown

**Where it lives**

In an eval package, inspect the issue's expected and reported behavior, then compare it with the repro report's output excerpts, logs, stack traces, screenshots, exit codes, or failing tests. In live mode, compare the GitHub issue description with the artifacts included in the draft repro comment.

**What good looks like**

For a successful reproduction, the artifact directly demonstrates the same symptom, error, or incorrect behavior described in the issue; an unrelated setup, dependency, permission, or configuration failure does not prove the issue. For a cannot-reproduce result, the artifacts must show a relevant, good-faith attempt to exercise the reported scenario, and the report must identify any material environment or trigger differences that may explain why the behavior was not observed.

## Honesty

**Where it lives**

Compare the claim comment and the repro report's conclusion with the commands, outputs, and artifacts included in the package. Also compare any stated root cause with the evidence offered for it.

**What good looks like**

The contributor states only what the evidence establishes. A supported reproduction and a supported cannot-reproduce result are both acceptable. The report must not claim that the issue was reproduced, identify a root cause, or promise a fix when the included evidence does not support that conclusion.

## Comms

**Where it lives**

In an eval package, inspect the claim comment and repro report against the issue context, the repo-facts contribution-policy block, comment templates, and repository communication rules. In live mode, inspect the GitHub issue thread, the repository's `CONTRIBUTING` documentation, pull-request or issue templates, AI-use policy files, and the student's draft comments.

**What good looks like**

The claim names the issue-specific behavior, promises an investigation and a reproduction report, and does not prematurely assert success or promise a fix or completion date. The repro comment is specific, factual, and understandable without relying on another contributor's report. All repository requirements must be followed, including disclosure of AI assistance when the repository explicitly requires it. If the repository has no AI-disclosure rule, the absence of a disclosure is not a failure.
