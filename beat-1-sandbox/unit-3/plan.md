# Plan for Issue #68 — Handle an empty keyword-search corpus

## Diagnosis

The reproduced failure occurs when `KeywordSearcher.index([])` builds an empty `tokenized_corpus` and passes it directly to `BM25Okapi`.

My reproduction showed:

```text
File "rag/retriever/keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)
File "rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
ZeroDivisionError: division by zero
```

This shows that the empty list reaches `rank_bm25`, where the library calculates average document length using a corpus size of zero.

The existing `search()` method already handles an unavailable or empty index by returning `[]`, so the fix should prevent creation of `BM25Okapi` when no chunks are provided and preserve that existing empty-search behavior.

## Scope

### In scope

- Update `KeywordSearcher.index()` so an empty `chunks` list does not construct `BM25Okapi`.
- Preserve `self.chunks` consistently when indexing an empty corpus.
- Make `search()` return `[]` after `index([])` without raising an exception.
- Update or unmark the existing Issue #68 unit test so the corrected behavior is tested normally.

### Out of scope

- Changing BM25 scoring behavior for non-empty corpora.
- Modifying the external `rank_bm25` dependency.
- Refactoring unrelated retriever code.
- Changing query tokenization or result ranking.

## Files to change

- `rag/retriever/keyword_search.py`
  - Add explicit handling for an empty list in `KeywordSearcher.index()` before constructing `BM25Okapi`.

- `tests/unit/test_keyword_search.py`
  - Convert the existing Issue #68 `xfail` test into a normal regression test.
  - Keep the expected result `[]` after indexing an empty corpus and searching.

## Approach

1. Add an early empty-corpus path in `KeywordSearcher.index()`.
2. Keep `self.chunks` set to the provided empty list.
3. Leave `self.bm25` unset or reset to `None` rather than calling `BM25Okapi([])`.
4. Allow the existing guard in `search()` to return `[]` when the index is empty.
5. Remove the Issue #68 `xfail` marker from `test_empty_index` so the behavior becomes a required passing test.
6. Run the focused test and existing keyword-search unit tests to confirm the fix does not change non-empty behavior.

## Test plan

### Reproduction check

Before the fix, this command raises `ZeroDivisionError`:

```python
from rag.retriever.keyword_search import KeywordSearcher

searcher = KeywordSearcher()
searcher.index([])
```

After the fix, it should complete without raising.

Then:

```python
results = searcher.search("python", top_k=10)
assert results == []
```

Expected result after the fix:

```text
[]
```

### Existing regression test

Run:

```bash
python -m pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -v
```

Before the fix, the test reports `XFAIL`.

After removing the `xfail` marker and implementing the fix, it should report `PASSED`.

### Control case

Re-run the non-empty control:

```python
searcher = KeywordSearcher()
searcher.index([
    {"id": 1, "text": "python programming"}
])
results = searcher.search("python")
```

Expected: one result is returned and normal BM25 indexing/search behavior remains intact.

### Broader unit coverage

Run:

```bash
python -m pytest tests/unit/test_keyword_search.py -v
```

Expected: all keyword-search tests pass, including the formerly expected-failure empty-index test.

## Risks and unknowns

- I need to confirm whether resetting `self.bm25` to `None` on every empty `index([])` call is the intended behavior if the same `KeywordSearcher` previously held a non-empty index.
- I will verify that `self.chunks` and `self.bm25` remain consistent when re-indexing from a populated corpus to an empty corpus.
- The fix should remain local to `KeywordSearcher` rather than relying on behavior inside the external `rank_bm25` package.

## Deviations

## Deviations

The implementation followed the planned approach. I explicitly reset `self.bm25` to `None` when `index([])` is called so that a previously populated BM25 index cannot remain active.

I also added a regression test that first indexes a non-empty corpus, then calls `index([])`, and verifies that a subsequent search returns `[]`. This directly covers the re-indexing risk identified in the plan.

No other implementation changes were needed.