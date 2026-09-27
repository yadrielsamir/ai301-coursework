# Evidence guide: where proof lives in a reproduction package

## Environment

Used by: environment-recorded

Where it lives:
- Eval bundle: the repro report's environment record (the block listing OS, runtime, package manager, and commit or branch), read against the issue context for the version or platform the issue targets, and against the repo-facts block for any required versions.
- Live mode: the environment section of the student's draft report, read against the issue thread for the reported version or platform and against the repo's README or CONTRIBUTING for required versions.

What good looks like: a reader can tell which platform and which code the behavior was seen on. The record covers the OS or browser, the versions the bug-report template asks for, and the tested code, where a released version number is enough and a commit or branch is needed only when no release is named. The tested version matches the issue's target and any version the template asks reporters to test on, or the report calls out the difference. Extras the template does not ask for are not required. A record that says only "latest" or "my machine" is not sufficient.

## Steps

Used by: steps-followable

Where it lives:
- Eval bundle: the repro report's steps, read against the setup the repo-facts block or issue context says the repo documents.
- Live mode: the steps in the student's draft report, read against the repo's README or CONTRIBUTING setup instructions.

What good looks like: starting from a fresh clone, a stranger can go from setup to the trigger using only what is written. Every command, input, URL, account, or seed data the steps depend on is stated or points to the repo's docs. A step that says "set up the project" with no command or doc reference is a gap. Step count and formatting do not matter.

## Behavior shown

Used by: behavior-matches-issue

Where it lives:
- Eval bundle: the artifacts in the repro report (output excerpts, error text, logs, screenshot descriptions), read against the behavior described in the issue context.
- Live mode: the artifacts in the student's draft report, read against the issue body and any error or screenshot in the issue thread.

What good looks like: the artifact shows the same behavior the issue describes, on the same page, command, or component, with matching error text where the issue gives one. An artifact from a different page, a different error message, or a failure during install or setup does not show the issue's behavior, even if it looks like a bug. For a cannot-reproduce outcome, the artifact shows the issue's own trigger being run and the correct behavior appearing.

## Honesty

Used by: outcome-honest

Where it lives:
- Eval bundle: the repro report's stated outcome (reproduced, partially reproduced, or cannot reproduce) and any claims about cause, read against that report's artifacts.
- Live mode: the outcome line and any cause or fix claims in the student's draft report, read against the artifacts in the same draft.

What good looks like: every claim in the outcome is backed by an artifact in the same report. "Reproduced" has an artifact showing the issue's behavior. "Cannot reproduce" has an artifact showing the attempt and what happened instead. Claims about root cause or a fix appear only if an artifact demonstrates them, otherwise they are framed as guesses or left out. A confident outcome with no matching artifact is a fail even if it sounds plausible.

## Comms

Used by: conventions-respected, claim-specific

Where it lives:
- Eval bundle: the claim comment and repro report text, read against the contribution policy line in the repo-facts block and the house rules in scope.md. For claim-specific, the claim comment read against the issue context.
- Live mode: the student's draft comments, read against the repo's CONTRIBUTING file, any issue or PR templates, the AI-use policy, and the house rules in the skill's scope.md. For claim-specific, the draft claim read against the issue body.

What good looks like:
- Disclosure: issue comments count as contributions. When the policy requires disclosing AI assistance, at least one comment in the package must disclose it, since a claim and its report form one contribution. A missing disclosure fails even if no AI use is evident, because a reader cannot tell whether AI was used. If the policy is silent or asks only for human review or responsibility, no disclosure is required.
- Own words: the proof is written by the author and stands alone. A comment that defers to another commenter ("same as above", "can confirm", "+1") does not count, even on a shared issue. Another person's claim on the issue never counts against this package.
- Specific claim: the claim names something from this issue (the behavior, page, component, or error) and says what the author will do next. A claim that would fit any issue unchanged ("I'd like to work on this") is boilerplate.