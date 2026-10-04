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

hemasribavisetty

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68#issuecomment-5983455392

Following up on my reproduction above: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/68#issuecomment-5851214046

I reproduced Issue #68 and plan to address it by handling the empty-corpus case inside `KeywordSearcher.index()` before it reaches `BM25Okapi`.

The current failure occurs because `index([])` passes an empty tokenized corpus into `rank_bm25`, which divides by a corpus size of zero. The change will stay local to `rag/retriever/keyword_search.py`: when there are no chunks, the searcher will keep an empty chunk list, clear any previously built BM25 index, and allow the existing `search()` empty-index guard to return `[]`.

I’ll update `tests/unit/test_keyword_search.py` by removing the Issue #68 `xfail` marker and using the existing empty-index test as the regression test. I’ll also cover re-indexing from a populated corpus to an empty corpus to verify that an older BM25 index is cleared.

Validation will include the original `index([])` reproduction, the focused empty-index test changing from `XFAIL` to `PASSED`, the populated-then-empty regression case, the non-empty one-document control, the full keyword-search test file, and the repository checks with `make check` and `make test-unit`.

---

## Your branch

**Branch**

fix/68-handle-empty-keyword-index

**Evidence**

### Before the fix

Command:

```bash
python - <<'PY'
from rag.retriever.keyword_search import KeywordSearcher

searcher = KeywordSearcher()
searcher.index([])
PY
```

Output:

```text
Traceback (most recent call last):
  File "<stdin>", line 4, in <module>
  File "/Users/hemasribavisetty/Documents/Codepath_AI/pathreview-ai301-fa26-s1/rag/retriever/keyword_search.py", line 25, in index
    self.bm25 = BM25Okapi(tokenized_corpus)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/hemasribavisetty/Documents/Codepath_AI/pathreview-ai301-fa26-s1/.venv/lib/python3.11/site-packages/rank_bm25.py", line 83, in __init__
    super().__init__(corpus, tokenizer)
  File "/Users/hemasribavisetty/Documents/Codepath_AI/pathreview-ai301-fa26-s1/.venv/lib/python3.11/site-packages/rank_bm25.py", line 27, in __init__
    nd = self._initialize(corpus)
  File "/Users/hemasribavisetty/Documents/Codepath_AI/pathreview-ai301-fa26-s1/.venv/lib/python3.11/site-packages/rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
                 ~~~~~~~~^~~~~~~~~~~~~~~~~~
ZeroDivisionError: division by zero
```

The focused regression test before the fix reported:

```text
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index XFAIL
(issue #68 (manifest H-01): BM25 keyword search raises ZeroDivisionError on an empty index)

1 xfailed
```

### After the fix

Command:

```bash
python - <<'PY'
from rag.retriever.keyword_search import KeywordSearcher

searcher = KeywordSearcher()
searcher.index([])
results = searcher.search("python", top_k=10)
print(results)
PY
```

Output:

```text
2026-10-04 11:39:09 [info     ] keyword_index_built            chunk_count=0
2026-10-04 11:39:09 [warning  ] keyword_search_empty_index
[]
```

Non-empty control:

```bash
python - <<'PY'
from rag.retriever.keyword_search import KeywordSearcher

searcher = KeywordSearcher()
searcher.index([
    {"id": 1, "text": "python programming"}
])

results = searcher.search("python")
print(results)
PY
```

Output:

```text
2026-10-04 11:39:15 [info     ] keyword_index_built            chunk_count=1
2026-10-04 11:39:15 [info     ] keyword_search_complete        query_len=1 results_count=1
[{'id': 1, 'text': 'python programming', 'bm25_score': -0.2746530721670274}]
```

The full keyword-search test file after the fix reported:

```text
18 passed in 0.19s
```

The full unit suite reported:

```text
377 passed, 52 xfailed, 2 warnings in 5.80s
```
## Eval iterations

**Run history**

1. First successful full scored run: 19/20. The only disagreement was `pkg-14`, which my rubric rejected while the gold label accepted it.
2. Second full scored run: 19/20, again with `pkg-14` as the disagreement.
3. Final confirming run saved with `--save-run eval-run.txt`: 18/20. The disagreements were `pkg-09` and `pkg-14`, both gold `accept` packages that my rubric rejected. Every category still had at least one match, so the final run passed the 18/20 bar and category floor.

Before these scored runs, I also had harness attempts that errored because my Claude OAuth token had been revoked; those produced no scored agreement and did not change the rubric.

**Package analysis**

I analyzed `pkg-14`. My rubric verdict was `reject`, while the gold label was `accept`.

My rubric rejected the package because `diagnosis_grounded` and `uncertainty_honest` failed. The candidate plan states that on reattach, Zellij wires client input before OSC color-query responses are consumed and presents that mechanism as the diagnosis. However, the reproduction evidence directly proves the timing of the leak — fresh attach is clean, reattach leaks, 0.44.1 is clean, and clearing the cache gives one clean attach — but it does not directly prove the exact internal ordering of the client input wiring and OSC-response consumption.

Because my rubric requires an unproven root cause to be identified as a hypothesis or verification target, it interpreted the plan as more certain than the evidence supported. The gold label accepted the plan because the diagnosis is strongly grounded by the regression window, reattach-only behavior, cache observation, and the plan also says it will trace the exact functions using debug logs before implementation. This disagreement showed that my rubric is stricter than the gold label about how explicitly a plausible, evidence-backed diagnosis must be marked as uncertain.
**Check rationale**

Check: `diagnosis_grounded`

> Pass when the diagnosis explains the specific reproduced behavior without contradicting the evidence. If the evidence establishes only a failure location or correlation rather than a proven root cause, the plan must identify the proposed cause as a hypothesis or verification target instead of presenting it as certain.

I wrote this check this way because a plan can sound technically convincing while still overclaiming what the reproduction actually proves. I wanted the grader to distinguish between an observed failure and an inferred root cause. If the reproduction only establishes correlation, timing, or a failure location, the plan should clearly mark the proposed explanation as something to verify before building around it.

The trade-off became visible in `pkg-14`: the diagnosis was strongly supported and actionable, but because the internal ordering was not directly proven by the reproduction, my check treated the plan as too certain and rejected a package the gold label accepted.
**Trade-offs**

The `diagnosis_grounded` check favors avoiding confident but unsupported root-cause claims, which helps reject wrong-cause plans. The cost is that it can reject a strong, practical plan when several pieces of evidence point consistently toward one mechanism but do not directly prove the internal implementation detail.

`pkg-14` is the clearest example. The reproduction strongly supports a reattach-specific OSC handling problem, and the plan includes tracing with debug logs before pinning the exact functions, but my rubric still rejected it because the diagnosis was written too definitively. I accept that false-negative risk because I prefer a plan to label an unverified mechanism as a hypothesis rather than build from certainty the evidence has not yet established.
---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
