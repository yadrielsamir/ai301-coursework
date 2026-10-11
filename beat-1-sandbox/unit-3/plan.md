# Plan: issue #68, keyword search crashes on an empty index

## Diagnosis

`KeywordSearcher.index([])` constructs `BM25Okapi([])`. rank-bm25 computes the average document length as `num_doc / self.corpus_size`, and with an empty corpus that divides by zero. The crash happens inside `index()`, before `search()` runs. `search()` already returns `[]` when `self.chunks` is empty, which the control test confirms. The issue text attributes the error to `search()` and the IDF computation; my repro shows it comes from `index()` and avgdl instead.

Repro evidence (from my unit 2 repro comment, main at 2f4e82f, Python 3.12.14, rank-bm25 0.2.2):

```
.venv/bin/python -m pytest tests/unit/test_keyword_search.py -k "test_empty_index or test_index_not_called_returns_empty" --runxfail -v

tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index FAILED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_index_not_called_returns_empty PASSED

rag/retriever/keyword_search.py:25: in index
    self.bm25 = BM25Okapi(tokenized_corpus)
.venv/lib/python3.12/site-packages/rank_bm25.py:52:
    self.avgdl = num_doc / self.corpus_size
E   ZeroDivisionError: division by zero
1 failed, 1 passed, 15 deselected
```

## Scope

In: make `index()` skip building BM25 when `chunks` is empty, and remove the `xfail` marker on `test_empty_index` as the issue asks.

Out: any change to `search()`, tokenization, BM25 scoring, the rank-bm25 dependency, or other retrievers.

## Files to touch

- `rag/retriever/keyword_search.py`: at the top of `index()`, if `chunks` is empty, set `self.chunks = []` and `self.bm25 = None`, log it, and return before `BM25Okapi` is called. The existing guard in `search()` then returns `[]`.
- `tests/unit/test_keyword_search.py`: delete the `@pytest.mark.xfail(...)` decorator on `test_empty_index`.

## Approach

1. Search the repo for other callers of `KeywordSearcher.index()` and confirm none depend on `self.bm25` being set after indexing.
2. Add the empty check to `index()`.
3. Remove the xfail marker. It is `strict=True`, so leaving it would fail the suite once the fix works.
4. Run the test plan.

## Test plan

1. Rerun my repro command above, without `--runxfail` since the marker is gone:
   `.venv/bin/python -m pytest tests/unit/test_keyword_search.py -k "test_empty_index or test_index_not_called_returns_empty" -v`
   Before: `test_empty_index FAILED` with `ZeroDivisionError` at `rank_bm25.py:52`. Expected after: both tests PASSED, no traceback.
2. Run the whole file: `.venv/bin/python -m pytest tests/unit/test_keyword_search.py -v`. Expected after: all tests pass, with `test_empty_index` PASSED instead of XFAIL.

## Risks and unknowns

- Not yet checked: whether any caller of `index()` (such as a hybrid retriever) reads `self.bm25` directly after indexing. Step 1 of the approach checks this before any edit.
- My repro covers an empty list only. A corpus where every chunk's text is empty or whitespace produces documents of length zero, which may fail differently; I have not tested it and it stays out of scope.

## Deviations

None. I built the change as planned. Before editing I searched for other callers of `index()` and readers of `.bm25`, and found none outside `keyword_search.py`, so the early return in `index()` needed no extra handling. The commit touches only `rag/retriever/keyword_search.py` and `tests/unit/test_keyword_search.py`. All 17 tests in `tests/unit/test_keyword_search.py` pass after the fix. The case where every chunk's text is empty or whitespace is still untested and out of scope.
