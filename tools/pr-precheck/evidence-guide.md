# Evidence guide: where evidence lives in a PR package

Eval bundles use fixed sections: "## Repo facts", "## Issue", "## Thread highlights", "## Plan context", and "## Candidate PR" with "### Title", "### Description", "### Commits", "### Diff", and "### Test evidence". The plan inside "## Plan context" is an excerpt in prose, often with labels like "Plan:", "Not in scope:", "Files:", "Test plan:", and "Repro evidence"; locate each piece by what it says, not by its label.

## Plan fidelity (harness category: silent-drift)

Where it lives: Eval bundle: the planned steps, files, and "Not in scope" line in "## Plan context", read against the hunks in "### Diff", the commit list in "### Commits", and the claims in "### Title" and "### Description". Any deviation is disclosed either in the plan context or in the description. Live mode: the approach, files to touch, scope, and "## Deviations" section of plan.md, read against `git diff main...HEAD`, `git log main..HEAD`, and the title and description in pr_draft.md.

What good looks like: every behavior-changing hunk carries out a planned step or a disclosed deviation, and every planned step shows up in the diff or is disclosed as changed or left out, with the reason. The description names what the diff does and nothing it does not. Silent drift runs both ways: a diff that does more than the plan (an extra behavior change, a different approach) or less (a planned step missing) with no note anywhere, or a description that claims to follow the plan while the diff departs from it.

## Test evidence (harness category: not-tested)

Where it lives: Eval bundle: "### Test evidence" (and anything the description pastes), read against the "Test plan" and the repro evidence's broken result in "## Plan context", and against the checks "## Repo facts" says the repo asks for. Live mode: test_evidence.md and the Testing section of pr_draft.md, read against the test plan in plan.md and the four checks Path Review's PR template lists: `make test-unit`, `make test-integration`, `make lint`, and `make typecheck`.

What good looks like: the repro steps re-run on the branch, with the actual after output shown next to (or comparable with) the repro's broken result, differing the way the test plan predicted; and each check the repo asks for shown with its real output. A check that fails or cannot run is still evidence when the result and reason are stated (Path Review's `make test-integration` stopping with "no tests ran", reported as is, counts). "Tests pass", "verified locally", or a summary with no output is not evidence.

## Diff quality (harness category: unreviewable)

Where it lives: Eval bundle: "### Diff" (every file header and hunk) and "### Commits". Live mode: `git diff main...HEAD` and `git diff main...HEAD --stat`.

What good looks like: the changed file list contains only files the fix needs, and every hunk serves the fix: the planned change, plus test edits that exercise it (a new or updated test, or removing an xfail marker or a pyproject.toml suppression that names this issue, which Path Review's docs/CONTRIBUTING.md requires once a seeded bug is fixed). Debris tells: print or console.log lines, logging added only to investigate, commented out code, leftover TODOs from the work, whitespace or formatting churn in untouched lines, renames or refactors the fix does not need, and stray files (plan.md, pr_draft.md, test_evidence.md, comment.md, scratch scripts, local configs, unrelated lockfile changes).

## Standards and comms (harness category: standards-wall)

Where it lives: Eval bundle: the PR template, contribution docs, and AI policy lines in "## Repo facts", plus any maintainer direction in "## Thread highlights", read against "### Description". Live mode: `.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md` in the working copy, plus the issue thread, read against pr_draft.md. Path Review's template sections are Summary, Issue, Changes, Testing, Screenshots / Demo, and Notes for Reviewers; the issue goes after "Closes #", and the AI use disclosure goes in Notes for Reviewers.

What good looks like: every template section holds content specific to this change ("N/A" with a reason counts), ticked boxes only for things the evidence shows (an unticked CI box before the PR is opened is correct, not a gap), the issue referenced the way the docs ask, and the disclosure present wherever the repo's policy requires it. When the repo states no template, no PR asks, and no AI policy, there is nothing to violate. Failing looks like an empty or placeholder section, a template replaced by free text, a required disclosure missing, or a maintainer's stated direction in the thread ignored.
