# Fixing an Empty-Corpus Crash in PathReview's Keyword Search

## Overview

During my CodePath AI301 coursework, I contributed to PathReview, an open-source project that includes a retrieval system for searching document content. My contribution focused on a bug in its keyword-search component. When the system attempted to build a search index with an empty list of documents, it crashed with a `ZeroDivisionError` instead of handling the empty input.

I investigated the issue, reproduced the failure, implemented a small fix, and added regression tests to verify that it worked without changing normal search behavior. I submitted the change through [Pull Request #163](https://github.com/codepath/pathreview-ai301-fa26-s1/pull/163). At my last recorded check on November 7, 2026, the PR was open, mergeable, and awaiting maintainer review, with six passing CI checks.

## What happened

### Understanding and reproducing the problem

I started by investigating [Issue #68](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68), which reported that `KeywordSearcher.index()` crashed when called with an empty corpus.

The keyword-search component uses the BM25 ranking algorithm through the Python `rank_bm25` library. BM25 calculates relevance scores between a query and indexed documents. In this case, the problem occurred before searching could begin, during index creation.

My first challenge was setting up the development environment. I initially used Python 3.14 alpha, but NumPy failed to import because of a native extension compatibility error. That failure was unrelated to the reported bug.

I switched to Python 3.11.14, recreated the virtual environment, and ran the reproduction again.

The following code triggered the issue:

```python
from rag.retriever.keyword_search import KeywordSearcher

searcher = KeywordSearcher()
searcher.index([])
```

The resulting traceback showed:

```text
File "rag/retriever/keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)

File "rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size

ZeroDivisionError: division by zero
```

This was my first important piece of evidence. It showed that `KeywordSearcher.index()` passed an empty tokenized corpus to `BM25Okapi`, which attempted to divide by the number of documents. Because there were zero documents, the calculation failed.

I also tested the same component with one document. That control case succeeded, helping me confirm that the failure was specific to empty input rather than a general problem with the dependency.

### Implementing the fix

After identifying the cause, I wanted to keep the solution small and focused.

The existing `search()` method already checked whether the BM25 index or document list was empty and returned `[]` in that situation. The missing protection was in `index()`, which attempted to construct the BM25 index even when no documents were available.

I updated `rag/retriever/keyword_search.py` to handle that case before calling `BM25Okapi`.

The relevant change was:

```python
self.chunks = chunks

if not chunks:
    self.bm25 = None
    logger.info("keyword_index_built", chunk_count=0)
    return
```

This prevents an empty corpus from reaching the BM25 constructor. Setting `self.bm25` to `None` also clears any previously created index, so an old index cannot remain active after re-indexing with an empty list.

I deliberately avoided modifying the external BM25 library, changing tokenization, or adjusting scoring behavior for non-empty documents.

### Strengthening the regression tests

The repository already had a test named `test_empty_index`, but it was marked with `@pytest.mark.xfail` because the bug was known.

After implementing the fix, I removed the expected-failure marker and verified that the test passed normally.

During the planning review, I also identified another important scenario: what happens when the same searcher first indexes documents and then receives an empty corpus?

I added `test_reindex_with_empty_corpus_clears_previous_index` to cover this state transition. This was an improvement to my original testing approach because it checked not only whether the exception disappeared, but also whether the searcher cleared its previous state correctly.

The focused test run produced:

```text
tests/unit/test_keyword_search.py
18 passed in 0.19s
```

I also reran the original reproduction after the fix:

```python
searcher = KeywordSearcher()
searcher.index([])
print(searcher.search("python", top_k=10))
```

The output was:

```text
[]
```

The original crash no longer occurred.

As a control, I indexed a single document and searched for `"python"`. The returned BM25 score remained `-0.2746530721670274`, matching the result before the fix.

### Testing and preparing the pull request

Beyond the focused regression tests, I ran the repository's broader checks.

| Check | Observed result |
|---|---|
| Keyword-search tests | 18 passed |
| Full unit suite | 377 passed, 52 expected failures, 3 warnings |
| `make check` | Ruff passed, Black left 110 files unchanged, mypy found no issues |
| Frontend tests | 18 passed |
| Integration tests | No tests collected; pytest exited with code 5 |

The integration result was an important limitation to document. I did not describe it as a successful integration test because no tests actually ran.

I also built and used a `pr-precheck` skill to compare my implementation with the plan, test evidence, and repository contribution requirements.

Its first live evaluation rejected my PR package because I had not recorded the formatter and frontend test results. Although I had implemented the fix, my submission evidence was incomplete.

I responded by running the missing checks and updating `test_evidence.md` and `pr_draft.md` with the actual results. When I ran the checker again, all six required checks passed and its final verdict was `accept`.

That experience showed me that a technically correct change is not automatically a review-ready contribution. The evidence explaining how it was tested matters too.

### The result

I opened [PR #163](https://github.com/codepath/pathreview-ai301-fa26-s1/pull/163), containing changes to two files: the keyword-search implementation and its unit tests.

On November 7, I used a PR-monitoring tool to inspect its status. The monitor reported six passing CI checks, a clean merge state, and no maintainer reviews after 25 days. It generated a polite follow-up draft, which I reviewed rather than automatically posting.

The concrete result was a focused fix submitted for review, supported by regression tests and passing CI checks. I had not received confirmation of a merge at the time of that report.

## Limitations and what's next

The fix addresses the empty-corpus case in `KeywordSearcher.index()`. It does not attempt to redesign keyword retrieval or handle every possible invalid input. Other edge cases, such as missing document fields or unexpected data types, remain outside the scope of this contribution.

The integration test command also collected no tests, so I cannot claim integration-level coverage from that run. Although the unit tests and CI checks passed, broader integration coverage would provide additional confidence.

My next step is to continue monitoring the pull request and respond to any maintainer feedback. If reviewers request changes, I would update the implementation, rerun the relevant checks, and document the results before asking for another review.

For future contributions, I would verify the supported Python environment earlier, preserve test evidence as I work, and review the repository's contribution checklist before preparing the PR.

This experience helped me understand that open-source contribution involves more than writing a fix. Reproducing a problem carefully, keeping changes focused, testing edge cases, documenting evidence, and communicating with maintainers are all essential parts of delivering reliable software.

