# Unit 1 — Issue Selection

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68

**Verdict output**

Ranked read-out — all three accepted

1. #68 — Keyword search ZeroDivisionError on empty index
The only candidate with a failing test you can run today: tests/unit/test_keyword_search.py:134 is an @pytest.mark.xfail on test_empty_index, and it's a synchronous test — it sidesteps the async-testing gap in your profile entirely. Named files, named failing call (index([])), and a 2–4 hour estimate that fits "a few evenings." Its RAG/search-ranking location is superficial: the fix is an empty-corpus guard, not ranking math.

2. #61 — Health probe passes raw "SELECT 1" under SQLAlchemy 2.x
Strongest domain match in the set — SQLAlchemy textual SQL and text() wrapping is squarely the relational-DB/query-behavior ground you work in professionally, with the exact ArgumentError quoted. Ranked below #68 only because there is no tests/unit/test_health.py in the repo, so reproducing means standing up a live session or stack, and you'd be writing the covering test yourself.

3. #62 — Health check references nonexistent settings.redis_host
Smallest of the three and reproducible locally with no running services: pyproject.toml:147 carries a mypy suppression naming this issue, so make typecheck surfaces it. Ranked last on fit — Redis config plumbing is the furthest of the three from your SQL/database strength, and like #61 it has no covering test.

Per-check notes: unclaimed passes on #62 and #68 despite fresh claim comments (skonda29 2026-09-21; acordero4852 2026-09-19) — both are author_association: NONE classmates, and the Path Review house rule in scope.md says student claims don't block. Claim anyway. not-stalled passes trivially everywhere: the repo has zero pull requests, so there are no abandoned attempts.

[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "repo-active", "grade": "pass", "evidence": "archived=false; last main commit 2026-09-16T21:50:18Z by Aburke225, 5 days before capture date 2026-09-21"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; no linked PRs (repo has 0 PRs total); only claim is acordero4852 (author_association NONE) 2026-09-19, ignored per scope.md Path Review house rule"},
      {"name": "ai-policy-permits", "grade": "pass", "evidence": "docs/CONTRIBUTING.md, .github/PULL_REQUEST_TEMPLATE.md contain no AI clause; no AI_POLICY.md/AGENTS.md — policy is silent"},
      {"name": "not-stalled", "grade": "pass", "evidence": "no linked PRs and no cross-referenced PR events; repo-wide PR count is 0, so fewer than two closed linked PRs"},
      {"name": "contributor-can-start", "grade": "pass", "evidence": "opened by COLLABORATOR Aburke225 labeled 'bug'/'good first issue'; body names rag/retriever/keyword_search.py and the expected behavior, no approval gate in thread"},
      {"name": "actionable-without-followup", "grade": "pass", "evidence": "'KeywordSearcher.index() passes its tokenized corpus to BM25Okapi, so index([]) raises ZeroDivisionError' plus relevant files and 'Estimated effort: 2-4 hours'"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/61",
    "checks": [
      {"name": "repo-active", "grade": "pass", "evidence": "archived=false; last main commit 2026-09-16T21:50:18Z, 5 days before capture date 2026-09-21"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; 0 comments; no linked PRs (repo has 0 PRs total)"},
      {"name": "ai-policy-permits", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template contain no AI clause; policy is silent"},
      {"name": "not-stalled", "grade": "pass", "evidence": "no linked PRs at all, so no closed-unmerged attempt history"},
      {"name": "contributor-can-start", "grade": "pass", "evidence": "opened by COLLABORATOR Aburke225 with 'bug'/'good first issue' labels; body names api/routes/health.py and the required sqlalchemy.text() wrap"},
      {"name": "actionable-without-followup", "grade": "pass", "evidence": "'Steps to reproduce: call GET /health ... observe ArgumentError: Textual SQL expression 'SELECT 1' should be explicitly declared as text('SELECT 1')'"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/62",
    "checks": [
      {"name": "repo-active", "grade": "pass", "evidence": "archived=false; last main commit 2026-09-16T21:50:18Z, 5 days before capture date 2026-09-21"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; no linked PRs (repo has 0 PRs total); sole claim is skonda29 (author_association NONE, 'I am a student of codepath') 2026-09-21, ignored per scope.md Path Review house rule"},
      {"name": "ai-policy-permits", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PR template contain no AI clause; policy is silent"},
      {"name": "not-stalled", "grade": "pass", "evidence": "no linked PRs at all, so no closed-unmerged attempt history"},
      {"name": "contributor-can-start", "grade": "pass", "evidence": "opened by COLLABORATOR Aburke225 labeled 'good first issue'; docs/CONTRIBUTING.md names this issue's mypy suppression, confirming maintainers want the fix"},
      {"name": "actionable-without-followup", "grade": "pass", "evidence": "body names api/routes/health.py, the nonexistent settings.redis_host, the replacement redis_url, and a GET /health repro returning 503"}
    ],
    "verdict": "accept"
  }
]

---

## Eval iterations

**Run history**

2/3 (smoke, --limit 3); 0/1 then 1/1 on --only issue-01 while reworking
scope-bounded; 16/20 (full); 2/4 on --only issue-04,issue-11,issue-16,issue-19;
16/20 (full); 5/5 on --only issue-04,issue-05,issue-11,issue-15,issue-16;
17/20 (full, committed as eval-run.txt).

**Issue analysis**

issue-01. My rubric graded it reject; the gold label is accept. In my early runs
the check that sank it was scope-bounded, whose pass condition required the issue
to state "an observable current behavior and a desired behavior." That wording is
bug-report shaped, and issue-01 is a documentation proposal with no misbehaviour
to observe, so the check failed it on form rather than merit. I replaced that
check with contributor-can-start, which asks whether a newcomer could begin rather
than whether a behaviour was described. It still rejects issue-01, now because the
issue has zero comments and the check reads an empty thread as the maintainers not
having agreed the work should happen. The author is a repo contributor and nobody
had replied yet, so silence is not disagreement; my check cannot tell those apart
from the bundle text.

**Check rationale**

From `tools/issue-select/rubric.md`:

> | actionable-without-followup | The issue body | Enough to start without a
> clarifying question: repro steps for a bug, or a named target and its content
> for docs and feature work | preferred |

It has two alternative satisfiers because my earlier version asked only for
reproduction steps, which no documentation or feature issue can supply. Naming a
target and its content is the equivalent evidence for non-bug work, so the check
measures whether a newcomer can start rather than whether the issue is a bug.

**Trade-offs**

It gives up verdict power: as a preferred check it cannot reject anything. On
issue-05 (sympy type annotations) and issue-10 (tldr-pages) it was the only check
that failed, catching in both cases an umbrella issue with no single target, and
both were gold rejects my required checks accepted. Promoting it to required
would have caught both, at the cost of rejecting issue-01, which fails it because
a docs proposal has nothing to run. I left it preferred and accepted those two
false accepts.

---

## Selection rationale

**Selection rationale**

1. [Fit to your interests and the time you have — your words. The 2-4 hour
   estimate and the existing xfail test are the facts to draw on.]

2. The verdict identified correctly that the issue names its failing call
   (index([]) raising ZeroDivisionError in BM25Okapi), names the source file and
   its test file, carries a good first issue label from a collaborator, has no
   assignee or linked PRs, and sits in a repo committed to five days before I ran
   it. What the rubric could not weigh: [your words — the reproducibility point
   is the strongest one, since the rubric has no check for whether a failing test
   already exists, and that is what decided it over #61].

3. [Claiming difficulty — acordero4852 commented on 2026-09-19, two days before
   my run. My rubric passed unclaimed on the Path Review house rule that student
   claims do not block, and the read-out advised claiming anyway. Say what you
   plan to do.]reasoning that produced your rubric's result.]

**Check rationale**

[One check from the `rubric.md` uploaded to `tools/issue-select/`, quoted as it is
currently written, with the reasoning behind its current form.]

**Trade-offs**

[What the quoted check gives up. Any one of these is a complete answer: an issue whose
result it changes, a canary you re-ran with `--only`, a case you accept it will miss, or a
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
