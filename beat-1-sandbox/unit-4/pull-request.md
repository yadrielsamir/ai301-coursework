# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/276

**Branch**

fix/68-empty-index

## Eval iterations

**Run history**

1. Calibration run (calib-01 to calib-04, unscored): 3/4. calib-01 (gold accept) was rejected on evidence_is_observable.
2. Calibration run (unscored): 4/4.
3. Full run: `agreement: 17/20 scored items (bar: 18/20: below the bar)`, with `categories: clear-accept 4/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. Misses: pkg-05 and pkg-08 (failed repo_checks_run), pkg-11 (failed description_matches_diff), all gold accept.
4. Partial run (`--only`, 9 packages): 9/9. pkg-05, pkg-08, and pkg-11 flipped to accept; canaries pkg-03, pkg-04, pkg-07, pkg-09, pkg-10, and pkg-14 stayed reject.
5. Full run: `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`. This is the run in eval-run.txt.
**Package analysis**

pkg-11 (BurntSushi/ripgrep#3477). My rubric's verdict: accept. Gold label: accept.

In full run 3 my rubric rejected it on description_matches_diff. The description says "One function touched, tests added for the escaped, unescaped, and double-backslash cases." The diff edits `parse_gitignore_line` and also adds a new helper, `fn trim_unescaped_trailing_spaces(line: &str) -> &str {`, and its one test checks the unescaped case only indirectly. The grader read "one function" and the test list as claims the diff does not back. They are a loose summary of an accurate change: the fix, the behavior, and the scope the description states are all in the diff. I rewrote the check to fail only claims that are materially wrong, and the package now agrees with the gold label.

**Check rationale**

From `tools/pr-precheck/rubric.md`:

> | repo_checks_run | The test evidence and description read against the checks the repo facts, PR template, or contribution docs ask contributors to run (test suite, lint, format, type check, build, codegen) | Every check the repo asks for is reported with its specific result: the check named (by command or name) and its outcome ("passes (312 tests)", "clean", "no diff", or the failure and its reason). Pasted output is not required for these checks; the repro re-run's output is graded by evidence_is_observable. A requested check that is never mentioned, a blanket claim that names no check ("all checks pass"), or a check deferred to CI fails. A check that fails or could not run passes only if the evidence states the result and the reason honestly. If the repo names no checks, this passes with that fact as evidence. | required |

My first version failed any check "reported as passing with no output". That rejected pkg-05 and pkg-08, which name every requested check with a specific result ("`cargo test -p nu-protocol` passes (312 tests); fmt and clippy clean"). Real output matters most for the repro, which evidence_is_observable already grades, so this check now asks whether every requested check was run and reported honestly. In live mode it caught a real gap in my own PR: Path Review's CONTRIBUTING asks for a format check and the frontend tests, which my first draft never mentioned.

**Trade-offs**

Loosening repo_checks_run means a stated result like "clean" is trusted without output, so a PR that lies about a lint or format run could pass that check. I accepted that case because the repro, the evidence that proves the fix, still needs real output. To make sure the loosening did not let untested PRs through, I re-ran all four not-tested packages (pkg-04, pkg-07, pkg-10, pkg-14) as canaries with `--only`, and all four stayed reject.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
