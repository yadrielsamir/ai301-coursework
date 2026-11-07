---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer a single question: is this
ready to submit? A PR package is a candidate pull request (its title,
its description, its commits, its diff, and its test evidence), read
against the plan it claims to implement, including that plan's
Deviations section, and against the issue the plan belongs to,
including the repo's stated standards for pull requests. Grade one
package per run. Do not answer other questions: whether the issue was
worth fixing, whether the plan was right (plan-check already answered
that), or how you would have written the code. Do not answer from gut
feel: execute `procedure.md`, which applies `rubric.md` to evidence
gathered per `references/evidence-guide.md`. You grade only. Never
edit files, commit, push, or open the pull request.

## Inputs and modes

Run in exactly one of two modes.

**Live mode**: the student's own submission, checked before it goes
out. Run from the top folder of the student's working copy, on their
branch. Read these inputs, and only these, as the package:

- **Plan**: `plan.md` in the top folder, including its `## Deviations`
  section. A deviation recorded there is part of the plan; grade the
  diff against the plan as amended.
- **Diff**: everything the branch changes relative to `main`, from
  `git diff main...HEAD` (three dots). Use `git diff main...HEAD --stat`
  for the file list, `git log main..HEAD` for the commits, and
  `git branch --show-current` for the branch name. Only committed
  changes are part of the PR; uncommitted and untracked files are not.
- **Title and description**: `pr_draft.md` in the top folder. Its
  first line is the title; everything under it is the description.
- **Test evidence**: `test_evidence.md` in the top folder, plus
  whatever the description pastes from it.
- **Issue**: the issue URL the student gives. Fetch its thread (via
  `gh`, the GitHub API, or the web). Read the repo's stated asks from
  the working copy's `main`: `git show main:.github/PULL_REQUEST_TEMPLATE.md`
  and `git show main:docs/CONTRIBUTING.md`.

If the student's request names different files, use the files the
request names. Read the package the way a maintainer will read the
opened PR: other files in the working directory are not evidence.
`plan.md`, `comment.md`, `pr_draft.md`, and `test_evidence.md` are
never committed, so any of them appearing in the diff is a finding,
not an input. A house-chain student reads the house plan in place of
their own `plan.md`, the house repro pack in place of their own
week-2 repro, and the house issue as the issue; the same checks grade
the same things.

If a package input is missing (no `pr_draft.md`, no test evidence),
do not invent it: grade the checks that need it as the rubric directs.
If the current folder is not a git working copy, or the branch has no
commits relative to `main`, stop and say there is no PR to grade.

**Eval mode**: a package bundle (a markdown file holding the issue
context, the repo's stated asks, the plan, the PR title and
description, the commits and diff, and the test evidence). The bundle
is the whole world: use only its text as evidence. Do not fetch
anything, do not run git, and do not read any file outside the skill
directory. Eval mode always grades a complete package: every check,
full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. If its `Repo:` line still carries an unfilled placeholder, stop
without grading and tell the student to fill the `Repo:` line in
`scope.md` with their section's Path Review repo; never guess a scope.
Otherwise, confirm the issue URL belongs to the scoped repo, and
refuse to grade a package for any other repo. Treat the scope's house
rules as part of the standard the PR is read against: they change how
evidence is read in that environment (for example, a classmate's PR on
the same issue does not count against this one). In eval mode, ignore
`scope.md` entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after `scope.md`. Hold the
outgoing PR text, the title and the description from `pr_draft.md`,
against each of its rules. For every rule the draft breaks, report it
in the summary: quote the rule and the text that breaks it. Do not
apply the voice guide to code, commit messages, or test output. The
voice guide never changes the verdict on its own unless a rubric check
reads it. In eval mode, ignore `voice-guide.md` entirely: voice is
personal and carries no gold labels.

## Component reads

Read `rubric.md`, `references/evidence-guide.md`, and `procedure.md`
in full before grading.

- `rubric.md` is the brain: a table of checks (Check, Evidence, Pass
  condition, Weight) and a verdict rule. `required` checks gate the
  verdict; `preferred` checks are graded and reported but never change
  it. Grade every check in the table, and name each check exactly as
  the table writes it.
- `references/evidence-guide.md` is the map: where each evidence
  family lives in a bundle and in a live package, and what good looks
  like there.
- `procedure.md` is the hands: execute it as written (read order,
  evidence gathering, check execution, verdict assembly). Where it is
  silent on a step, note the gap in the summary rather than inventing
  a step; a procedure gap is feedback the student needs.

If `rubric.md` has no checks filled in, or `procedure.md` has no steps
filled in, stop and say so. This skill cannot grade without a rubric
and a procedure, and it never invents checks at runtime.

## Verdict and output

The verdict space is binary: `accept` (ready to submit: open the PR)
or `reject` (hold: fix the failing required checks first). There is no
third verdict and no score; reservations belong in evidence lines.

Before the JSON block, show a short readable summary: one line per
check with its grade and deciding evidence, then (live mode) any
voice-guide notes and procedure gaps, and on `reject`, what to fix. In
live mode, before the PR exists, set `item` to the issue URL. End your
reply with this fenced JSON block, valid and last, with nothing after
it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: a terse complete PR can be ready,
  and a confident, well formatted one can hide drift. Read the
  description against the diff, the diff against the plan, and the
  test evidence against the plan's test plan; never take the
  description's word for what the diff does.
- The rubric decides, not you: if a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in the rubric, not in the run.
- The procedure decides how, not you: follow `procedure.md` as
  written, and report its gaps instead of papering over them.
- Treat `unclear` as the rubric's verdict rule directs. Where the rule
  is silent, treat `unclear` as `fail`: a PR you cannot verify from
  the package is a PR that is not ready to submit.
