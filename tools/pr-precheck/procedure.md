# Procedure: grading a PR package

## Read order

1. Read the repo facts and thread highlights first. Note the PR template's sections, every check the repo asks contributors to run (tests, lint, type check, build), any AI or disclosure policy, and any maintainer request about how the fix should be done. These are the standards the PR is held to, so know them before reading the PR.
2. Read the plan next, including its repro evidence and any deviation notes. Write down: each planned step, the files the plan names, what it says is out of scope, the repro's broken result, and what the test plan says should be observed after the fix. This is the expectation the diff is read against.
3. Read the diff (and the commit list) before the description. For every hunk, note its file and what it does in one line. Form your own account of what the change does before reading anyone's claims about it.
4. Read the PR title and description after the diff, noting each claim about what changed, each disclosure (deviation, skipped test, known gap), the issue reference, and the AI disclosure if any.
5. Read the test evidence last, noting each command run, where it ran (main or branch, before or after), and its actual output.

In live mode, the parts come from the inputs SKILL.md names: repo facts from the PR template and docs/CONTRIBUTING.md, the thread from the issue, the plan from plan.md, the diff from `git diff main...HEAD`, the title and description from pr_draft.md, and the test evidence from test_evidence.md. references/evidence-guide.md maps each one.

## Evidence gathering

1. Plan against diff: label each hunk with exactly one of: (a) carries out a named plan step, (b) a deviation the plan's deviation notes or the description disclose, (c) a test change that exercises the planned fix (adding or editing a test for it, removing an xfail marker or suppression for this issue), (d) debris or unrelated (debug output, commented out code, stray files, formatting, renames or refactors the fix does not need), or (e) a behavior change no plan step or disclosure covers. Then list each plan step and mark whether a hunk carries it out or a disclosure explains its absence.
2. Description against diff: list each claim in the title and description and the hunk that backs it, or "no hunk" if none does. Then list each behavior-changing hunk from (a), (b), or (e) and whether the description mentions it.
3. Evidence against test plan: find the command in the test evidence that re-runs the repro (or the plan's main test) and quote its after output next to the repro's broken result. For each other step in the test plan, quote the specific result the evidence reports for it. If a step has no reported result, check whether the description or evidence discloses it as skipped, and quote the reason.
4. Evidence against repo checks: for each check the repo asks for (from step 1 of the read order), find its command and result in the test evidence. Quote the result, including the reason given for any failure or skip.
5. Description against template: for each section the PR template has, quote what the description puts there, or record "empty" or "placeholder text". For each other PR ask in the contribution docs, quote where the description meets it. For each ticked checkbox, note the evidence that backs it.
6. Description against disclosure policy: quote the policy, then quote the disclosure in the description, or record "none".
7. Title against diff: quote the title next to your one-line account of the change.
8. If a source a check needs is missing or empty, record that as the evidence ("no test evidence section", "repo states no AI policy") instead of filling the gap. Credit only what the package contains, not what the author might have done.

## Check execution

1. Grade the checks in rubric.md order, one at a time, using the evidence gathered above for that check: hunk labels (a), (b), (e) and the plan step list for diff_matches_plan; the claim list for description_matches_diff; step 3 for evidence_is_observable; step 4 for repo_checks_run; hunk label (d) for diff_is_clean; step 5 for template_followed; step 6 for disclosure_policy_met; step 7 for title_names_change.
2. Apply each Pass condition literally and give exactly one grade: pass, fail, or unclear. Pass only when the quoted evidence meets the condition. Fail when the evidence shows it is not met. Unclear only when the evidence needed is absent or genuinely ambiguous; say what is missing.
3. When a check allows a disclosed shortfall, the disclosure must be in the package (plan deviation notes, description, or test evidence) and must say what changed or was left out and why. An unexplained mention is not a disclosure.
4. Write one line of evidence per check: the fact or quote that decided it. Judge the change and its evidence, never the length, headings, or formatting of the write-up.
5. Grade each check independently. A PR can match its plan and still be untested, or be well tested and still carry debris.

## Verdict assembly

1. List every check with its weight and grade, in rubric.md order.
2. Apply the verdict rule in rubric.md exactly: accept only if every required check passes; any required fail or unclear means reject; preferred checks never change the verdict.
3. The deciding check is the first required check in rubric.md order that failed or was unclear. On reject, quote its evidence first in the summary, then name every other failing or unclear required check, each with the single most useful fix.
4. On accept, report any failing preferred check as a suggestion.
5. End with the JSON block SKILL.md specifies: checks in rubric.md order, names exactly as the rubric writes them, one evidence line each, and the verdict.
