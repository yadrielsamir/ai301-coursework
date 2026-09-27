# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

yadrielsamir

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5860058343

I'd like to take this one. I'll set up the repo locally and try to reproduce the `ZeroDivisionError` from `KeywordSearcher` when `index()` is called with an empty corpus, using `test_empty_index` in `tests/unit/test_keyword_search.py`. I'll post a repro report here with my environment and the output I see.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5860062016

**Outcome:** Reproduced on main at 2f4e82f. Indexing an empty corpus raises `ZeroDivisionError` from rank-bm25. One difference from the issue text: the error is raised during `index([])`, when `BM25Okapi` computes the average document length (`avgdl`), before `search()` is ever called. It does not come from the IDF computation.

**Environment:** macOS 27.0, Python 3.12.14 (Homebrew; the repo requires 3.11+ and macOS ships 3.9), pytest 9.1.1, rank-bm25 0.2.2, fork of pathreview-ai301-fa26-s3 at commit 2f4e82f. Installed with `pip install -e ".[dev]"` in a venv. Docker services not started, since this test does not use them.

**Steps:**
1. `python -m venv .venv && .venv/bin/pip install -e ".[dev]"`
2. `.venv/bin/python -m pytest tests/unit/test_keyword_search.py -k "test_empty_index or test_index_not_called_returns_empty" --runxfail -v`

**Expected:** after `index([])`, `search("python", top_k=10)` returns `[]`.

**Observed:** `test_empty_index` fails at `searcher.index([])`. The control `test_index_not_called_returns_empty` passes, so searching without indexing returns `[]`; only indexing an empty corpus crashes.

```
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index FAILED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_index_not_called_returns_empty PASSED

>       searcher.index([])
tests/unit/test_keyword_search.py:140:
rag/retriever/keyword_search.py:25: in index
    self.bm25 = BM25Okapi(tokenized_corpus)
.venv/lib/python3.12/site-packages/rank_bm25.py:27: in __init__
    nd = self._initialize(corpus)
>       self.avgdl = num_doc / self.corpus_size
E       ZeroDivisionError: division by zero
.venv/lib/python3.12/site-packages/rank_bm25.py:52: ZeroDivisionError

1 failed, 1 passed, 15 deselected in 1.27s
```

I haven't decided on a fix yet; my next step is looking at how `index()` in `rag/retriever/keyword_search.py` should handle an empty corpus.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Run 1 (full): `agreement: 17/20 scored items (bar: 18/20: below the bar)`. All three misses (pkg-01, pkg-05, pkg-12) were clear-accept packages that failed only environment-recorded.
2. Run 2 (`--only pkg-01,pkg-05,pkg-12,pkg-16,pkg-20`): `agreement: 5/5 scored items`, after loosening environment-recorded. pkg-16 and pkg-20 were canaries.
3. Run 3 (full): `agreement: 18/20 scored items (bar: 18/20: below the bar; category floor unmet: no match in disclosure)`. pkg-07 and pkg-20 flipped on conventions-respected, a check I had not edited.
4. Run 4 (full, saved as eval-run.txt), after rewriting conventions-respected: `agreement: 19/20 scored items (bar: 18/20: PASS)`, with `clear-accept 7/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.

**Package analysis**

pkg-20. In run 3 my rubric decided accept; the gold label is reject. The repo's AI policy requires disclosing AI use in the contribution, and neither comment discloses. My grader's evidence for conventions-respected read: "No AI use is evident in the comments, so the disclosure requirement (triggered only by AI use) does not apply". My rule said only that comments must disclose when the policy requires it, so it left room to decide the policy did not apply. In run 1 the same rule had rejected pkg-20, which showed the rule was ambiguous rather than wrong in one direction. I added that a missing disclosure fails even when no AI use is evident, because a grader cannot observe whether AI was used. In run 5 my rubric rejected pkg-20, matching gold.

**Check rationale**

conventions-respected, Pass condition as it reads in my uploaded rubric.md:

> The comments follow the repo's stated conventions. Issue comments, including claims and repro reports, count as contributions: a policy that requires disclosing AI use in contributions applies to them. When such a policy exists, the package passes if at least one of its comments discloses AI assistance, since a claim and its report form one contribution, and fails if neither does. Do not excuse a missing disclosure by concluding the author used no AI; a grader cannot observe that, so under a disclosure policy the absence of a disclosure is a fail. When the policy is silent or only asks for human review or responsibility, no disclosure is required. The proof is in the author's own words; a piggyback on another commenter ("same as above", "can confirm") fails

It reads this way because my first version, "every comment in the package discloses it," produced opposite verdicts on unchanged packages between runs 1 and 3. Each added sentence closes one reading the grader actually took. For pkg-07 it wrote "Claim comment discloses AI assistance, but the repro report comment contains no disclosure at all", so one disclosure now covers a claim and its report. For pkg-20 it concluded no AI was used, so absence of a disclosure is now a fail. I rejected dropping the disclosure rule's strictness entirely, since pkg-20 is the only disclosure package and missing it fails the category floor.

**Trade-offs**

Accepting one disclosure per package is more lenient than requiring it in every comment, so a package where the claim discloses but a later report hides heavier AI use would still pass; I accept that miss because it matches how gold treated pkg-07. I skipped an `--only` canary run for this change and went straight to a full run, which let the full run serve as the check: pkg-03, whose policy asks for human authorship but no disclosure, stayed accept, and pkg-04, a piggyback, stayed reject. Earlier, loosening environment-recorded fixed pkg-01, pkg-05, and pkg-12, and I kept a clause failing a tested version that contradicts the issue's target or the template's ask so that pkg-16, whose only failing check was environment-recorded, stayed reject.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
