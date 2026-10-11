# Evidence guide: where evidence lives in a plan package

Plans do not use fixed headings. The candidate plan may use labels like "Cause:", "Change:", "In:", "Out:", "Test:", or full sections. Locate each piece by what it says, not by what it is titled.

## Diagnosis and grounding

Where it lives: Eval package: the cause statement in "## Candidate plan" (often "Cause:"), read against "## Repro evidence" (Steps, Expected, Actual). Live mode: the diagnosis in plan.md, read against the student's posted repro comment on the issue thread.

What good looks like: the stated cause explains every Actual result in the repro evidence, including what changes the behavior (for example, a re-entry that fixes it). It names nothing the repro contradicts. A cause that would predict a different result than the repro shows fails.

## Scope

Where it lives: Eval package: the change statement in "## Candidate plan", especially any "In:" and "Out:" lines and the files named. Live mode: the scope section and files to touch in plan.md.

What good looks like: one change aimed at the diagnosed cause, every named file serves it, and at least one explicit exclusion ("Out: any change to how push status is computed"). A plan that also refactors, renames, or adds features beyond the diagnosed cause is a drive-by rewrite.

## Executability

Where it lives: Eval package: the change statement in "## Candidate plan" (file paths, functions, callbacks). Live mode: the files to touch and approach sections of plan.md.

What good looks like: a specific file path plus the function or location inside it, and the concrete edit ("add the commits context to the post-push refresh scope in the push completion callback in pkg/gui/controllers/sync_controller.go"). A stranger could open that file and start. "Look into the refresh logic" is not executable.

## Test plan

Where it lives: Eval package: the test statement in "## Candidate plan" (often "Test:"), read against the numbered Steps and Expected in "## Repro evidence". Live mode: the test plan in plan.md, read against the student's repro steps.

What good looks like: it reruns the repro steps and names the step where the result must differ, plus the exact observable result expected after the fix ("at step 3 the color must flip without leaving the view"). Extra cases that share the changed code path are a strength. "Verify it works" or no expected result fails.

## Honesty

Where it lives: Eval package: any risks, unknowns, or hedged statements in "## Candidate plan" and "## Candidate plan comment", read against what "## Repro evidence" actually establishes. Live mode: the risks and unknowns section and the ## Deviations section of plan.md.

What good looks like: claims the repro did not test are labeled as assumptions or risks. If the repro fully supports the diagnosis, a plan with no risks section can still pass. A plan that states untested behavior as fact ("this also fixes Windows", "no other callers use this") fails. Mid-build changes are recorded under ## Deviations.

## Comms

Where it lives: Eval package: "## Candidate plan comment", read against "## Thread highlights" (maintainer requests, ruled out approaches, other posted plans, claim rules) and the contribution policy, templates, and AI disclosure lines in "## Repo facts". Live mode: comment.md, read against the live issue thread and the repo's CONTRIBUTING and templates.

What good looks like: the comment acknowledges and follows what the thread and policy ask (for example, keeping the change minimal because CONTRIBUTING says review time is scarce, or including an AI disclosure if the repo requires one). It does not propose an approach a maintainer ruled out, and it does not repeat an existing plan without its own reproduction. When the thread is empty, the comment still must not conflict with the repo facts. Boilerplate that would fit any issue fails.