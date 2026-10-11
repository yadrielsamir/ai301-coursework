# Procedure: grading a plan package

## Read order

1. Read the repro evidence first (commands, output, observed vs expected behavior). Everything else is graded against it, so form your own view of what it proves before reading the plan.
2. Read the thread highlights next, noting every maintainer request, ruled out approach, assignment or claim rule, and any plan already posted.
3. Read the repo-facts block, noting the files and functions that exist and any stated conventions (contributing guide, test commands, branch or comment format).
4. Read plan.md last, in its own order: diagnosis, scope, files to touch, approach, test plan, risks and unknowns.
5. Read the plan comment after plan.md, so you can compare it with the plan it claims to summarize.

## Evidence gathering

1. For each check in rubric.md, find the exact sources its Evidence column names. Do not grade from memory of the package; return to the source.
2. Quote the specific line or passage you will rely on for each check, from both sides of the comparison (for example, the plan's stated cause and the repro output it relies on).
3. If a source the check needs is missing or empty, record that as the evidence ("no risks section", "test plan names no expected result") instead of filling the gap with assumptions.
4. Treat only what the drafts and package contain as evidence. Do not credit the author for things they might know but did not write.

## Check execution

1. Grade the checks in the order they appear in rubric.md, one at a time.
2. For each check, apply the Pass condition literally to the quoted evidence and give exactly one grade: pass, fail, or unclear.
3. Grade pass only when the quoted evidence meets the condition. Grade fail when the evidence shows the condition is not met. Grade unclear only when the evidence needed to decide is absent or genuinely ambiguous, and say what is missing.
4. Write one sentence of reasoning per check that names the evidence and states why it meets or misses the condition. Judge the substance of the plan, never its length, headings, or formatting.
5. Do not let one check's result change another's. A plan can have a correct diagnosis and still fail scope, or vice versa.

## Verdict assembly

1. List every check with its weight and grade.
2. Apply the verdict rule in rubric.md exactly: accept only if every required check passes; any required fail or unclear means reject; preferred checks never change the verdict.
3. If the verdict is reject, name each required check that failed or was unclear, and for each one state the single most useful change that would make it pass.
4. If the verdict is accept, still report any preferred check that failed, as a suggestion.
5. End with the JSON block the skill expects, with the verdict as accept or reject and each check's grade.