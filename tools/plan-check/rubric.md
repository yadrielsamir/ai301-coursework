# Rubric: is this plan ready to post and build from?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis_matches_evidence | The plan's stated cause read against what the repro evidence shows (commands, output, observed vs expected behavior) | Every claim in the diagnosis is supported by something in the repro evidence, and nothing in the repro evidence contradicts it. A cause the evidence never touches, or one the evidence argues against, fails. | required |
| targets_cause_not_symptom | The plan's approach and files to touch read against the diagnosis | The proposed change acts on the code path the diagnosis names as the cause. A change that only hides, catches, or works around the visible symptom (suppressing the error, special casing the reported input) while leaving the named cause in place fails. | required |
| scope_is_bounded | The plan's scope statement (what changes, what does not) read against the files to touch and the approach | The change fixes one problem, every file listed serves that fix, and the plan names at least one thing it will not change. Refactors, renames, or features unrelated to the diagnosed cause fail. | required |
| executable_by_stranger | The plan's change and files to touch read against the issue and the repro evidence | A contributor who has never seen the issue could start the first edit without asking a question: the plan names a specific file path, function, or code location, and says what to change there. Vague steps ("investigate", "clean up", "improve handling") with no concrete location or action fail. | required |
| test_plan_is_observable | The plan's test plan read against the repro evidence's steps | The test plan reruns the repro steps (or a check that runs through the real code) and states a specific observable result expected after the fix that differs from the result in the repro evidence. "Make sure it works" or a result identical to the broken one fails. | required |
| unknowns_are_honest | The plan's risks and unknowns read against the diagnosis and the repro evidence | Anything the repro evidence does not establish is stated as an unknown or risk, not as fact. A plan that asserts certainty about behavior the evidence never showed fails. A plan that lists no risks passes only if the repro evidence fully covers the diagnosis. | required |
| fits_thread_and_conventions | The plan comment read against the thread highlights and the repo-facts block (contributing guide, maintainer requests, stated conventions) | The comment does not contradict or ignore anything a maintainer or the repo's stated conventions ask for (required format, approach a maintainer ruled out, claim or assignment rules, test or branch conventions), and it does not duplicate a plan already posted without adding its own reproduction. | required |
| comment_reflects_plan | The plan comment read against plan.md | The comment states the same diagnosis, scope, and test plan as plan.md, without adding claims the plan does not make. | preferred |

## Verdict rule

Accept (ready) only if every required check passes. Reject (hold) if any required check fails or is unclear: an unclear required check means the drafts did not show the evidence, and a plan that cannot show it is not ready to post. Preferred checks are reported but never change the verdict, whether they pass, fail, or are unclear.