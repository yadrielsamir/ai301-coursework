# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| repo-active | The repo-facts block: last 5 default-branch commit dates, archived flag | Not archived, and at least one default-branch commit within 30 days of the capture date. Comment-response latency is not evidence for this check | required |
| unclaimed | The repo-facts assignee and linked-PR fields, plus the comment thread | No assignee, no open PR by a non-maintainer that would resolve this issue, and no unresolved claim comment newer than 21 days. Closed or merged linked PRs do not fail this | required |
| ai-policy-permits | The repo-facts contribution policy line | The policy permits AI-assisted contributions or is silent on them. Fail only on an outright bar | required |
| not-stalled | The repo-facts linked-PR field and the comment thread | Fail when prior attempts exist and none succeeded: two or more closed linked PRs with zero merged. If any linked PR has merged, this passes | required |
| contributor-can-start | The issue body and comment thread | Fail only when a newcomer could not begin: the maintainers have not agreed the work should happen, or every contributor in the thread had to get a target approved before starting, or the change needs access or expertise a student lacks. A vague target, an open-ended list, or no proposed fix does not fail this | required |
| actionable-without-followup | The issue body | Enough to start without a clarifying question: repro steps for a bug, or a named target and its content for docs and feature work | preferred |

## Verdict rule

Accept when every required check passes. A fail on any required check rejects the issue. Unclear on a required check counts as a fail, since a newcomer cannot act on evidence that is not there. Preferred checks never change the verdict. They rank accepted issues, most preferred passes first, with ties broken in favor of the one with more recent default-branch activity.