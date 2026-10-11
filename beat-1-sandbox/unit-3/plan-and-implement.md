# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

yadrielsamir

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-6111724427

Plan for #68, building on my repro above (main at 2f4e82f).

Cause: the crash happens in `index([])`, not `search()`. Building `BM25Okapi([])` computes `avgdl = num_doc / self.corpus_size` and divides by zero (rank_bm25.py:52). `search()` already returns `[]` for an empty index, as `test_index_not_called_returns_empty` shows.

Change: in `KeywordSearcher.index()`, return early without building BM25 when `chunks` is empty, and remove the `xfail` marker on `test_empty_index` as the issue asks. Not changing `search()`, scoring, tokenization, or the rank-bm25 dependency.

Test: rerun my repro command without `--runxfail` and expect both tests to pass, then run the full `tests/unit/test_keyword_search.py` and expect everything to pass with `test_empty_index` no longer XFAIL.

Unknown: whether other callers read `bm25` right after `index()`. I'll check callers before editing.

---

## Your branch

**Branch**

fix/68-empty-index

**Evidence**

Before (Unit 2 repro, main at 2f4e82f):

```
.venv/bin/python -m pytest tests/unit/test_keyword_search.py -k "test_empty_index or test_index_not_called_returns_empty" --runxfail -v

tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index FAILED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_index_not_called_returns_empty PASSED

rag/retriever/keyword_search.py:25: in index
    self.bm25 = BM25Okapi(tokenized_corpus)
.venv/lib/python3.12/site-packages/rank_bm25.py:52:
    self.avgdl = num_doc / self.corpus_size
E   ZeroDivisionError: division by zero
1 failed, 1 passed, 15 deselected in 1.27s
```

After (branch fix/68-empty-index):

```
.venv/bin/python -m pytest tests/unit/test_keyword_search.py -k "test_empty_index or test_index_not_called_returns_empty" -v

tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index PASSED [ 50%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_index_not_called_returns_empty PASSED [100%]

======================= 2 passed, 15 deselected in 0.15s =======================
```

```
.venv/bin/python -m pytest tests/unit/test_keyword_search.py -v

tests/unit/test_keyword_search.py::TestKeywordSearcher::test_results_sorted_by_score_descending PASSED [  5%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_query_matching_no_documents PASSED [ 11%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_query_matching_multiple_documents PASSED [ 17%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_case_insensitive_matching PASSED [ 23%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_top_k_limit PASSED [ 29%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_top_k_larger_than_results PASSED [ 35%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_results_have_bm25_score PASSED [ 41%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_results_preserve_chunk_fields PASSED [ 47%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index PASSED [ 52%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_index_not_called_returns_empty PASSED [ 58%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_multi_word_query PASSED [ 64%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_tokenization PASSED [ 70%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_tokenization_case_handling PASSED [ 76%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_large_corpus PASSED [ 82%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_special_characters_in_query PASSED [ 88%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_exact_phrase_matching PASSED [ 94%]
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_single_word_chunks PASSED [100%]

============================== 17 passed in 0.10s ==============================
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

Run 1: `agreement: 19/20 scored items  (bar: 18/20: PASS)`. Categories: `clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.

That was my only full run. It passed the bar with every category matched, so I made no further changes, and this is the run saved in eval-run.txt.

**Package analysis**

pkg-14. My rubric decided reject; the gold label is accept. The eval output reads `pkg-14  clear-accept  accept  reject  NO  failed: executable_by_stranger`.

Every other required check passed, so the hold came from one check. Its pass condition requires that "the plan names a specific file path, function, or code location, and says what to change there." The grader judged that pkg-14's change statement did not meet that bar, and because my verdict rule says "Reject (hold) if any required check fails or is unclear," a single miss there was enough to hold a plan the staff considered ready. My rubric read it this way because it treats a concrete starting location as required, while the gold label shows a reviewer would accept a plan whose diagnosis, scope, and test plan are strong enough for a contributor to find the edit point themselves.

**Check rationale**

"A contributor who has never seen the issue could start the first edit without asking a question: the plan names a specific file path, function, or code location, and says what to change there. Vague steps ("investigate", "clean up", "improve handling") with no concrete location or action fail."

My first version of this check said the files and functions named must "exist in the repo facts." When I read calib-01 before running the eval, its repo facts block listed only repo metadata and contribution policy, not files, so that version would have graded every package unclear and held them all. I rejected it and rewrote the check to judge whether the plan itself gives a location and an action, which is what a stranger needs to begin. That version caught all 3 unbuildable packages.

**Trade-offs**

This check changes the result of pkg-14: it holds a plan the gold label accepts. Requiring a concrete location means a good plan that names the right area without a file path or function gets held. I accept that miss because loosening the check to "names an area" would let vague plans through, and the unbuildable category (3/3) depends on it. A false hold costs the author one revision; a false accept posts a plan nobody can build.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
