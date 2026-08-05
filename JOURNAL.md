# PathReview Contribution Journal

## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/36

**Issue title:** Architecture doc doesn't explain the hybrid retrieval scoring formula

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**  
The architecture documentation explains that PathReview combines vector similarity and BM25 keyword retrieval, but it does not describe how the two scores are normalized and blended. The implementation in `rag/hybrid.py` normalizes each score against the highest score returned by its retrieval method and then calculates a weighted sum using a default vector weight of `0.7` and keyword weight of `0.3`. Without this explanation, contributors cannot easily understand how chunks are ranked or why semantic similarity has more influence than keyword matching. A successful fix will document the normalization process, scoring formula, default weights, filtering behavior, and a numerical example in `docs/ARCHITECTURE.md`.

**Branch name:** `docs/36-hybrid-retrieval-scoring`

**Setup confirmation:** [x ] App runs locally at localhost:5173

**Cohort ledger:** [ x] Issue added to cohort ledger

### Issue selection notes — "Is this right for me?"

This issue is appropriately scoped for a first contribution because it requires a focused documentation change 
rather than a broad code refactor. The relevant implementation is contained in the RAG retrieval files, and the 
issue identifies `docs/ARCHITECTURE.md` as the file to update. I verified the formula and default weights directly from `rag/hybrid.py`, so the documentation can accurately reflect the current behavior. The main risk is documenting assumptions instead of the implementation, which I addressed by reviewing the vector, BM25, normalization, weighting, filtering, and sorting logic before writing the explanation.

---

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** [https://github.com/aditichhawacharia/pathreview/commit/8c152852d8913717e5f5b9c0a9e945447cfc5b8d]

**Reproduction summary:**  
Opened `docs/ARCHITECTURE.md` locally and confirmed that the RAG System section describes hybrid retrieval at a high level but contains no formula, no weight values, and no worked example. Cross-referenced against `rag/hybrid.py` and verified the gap is real: the normalization step, the `0.7 / 0.3` weight constants, the minimum-score filter, and the sort-and-truncate step are all implemented in code but absent from the doc.

**PLAN.md link:** [https://github.com/aditichhawacharia/pathreview/blob/docs/36-hybrid-retrieval-scoring/plan.md]

**Blockers or open questions:**  
Need to re-read `rag/hybrid.py` in full before writing the doc to confirm whether `vector_weight` and the minimum-score threshold are hardcoded constants or caller-configurable parameters — this affects how the doc describes the filter step and whether it should mention overridable defaults.

---
## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
Re-read `rag/hybrid.py` in full to verify the normalization logic, weight
constants, threshold, and sort behavior. Confirmed that `vector_weight=0.7`
and `keyword_weight=0.3` are constructor-level defaults on `HybridRetriever`,
that per-method normalization divides each raw score by the highest score that
method returned, and that `min_score=0.3` and `max_chunks=10` are defaults on
`retrieve()`. Drafted the Hybrid Retrieval Scoring subsection in
`docs/ARCHITECTURE.md` covering normalization, the weighted-sum formula,
default weights, threshold filtering, and a worked numerical example.

**Next steps:**
Make the minor wording fix clarifying that weights are set at retriever
initialization (not per-query), run `make check` to confirm no linting or
formatting errors, open a draft PR, and request peer feedback.

**Blockers:**
None.

---



### Check-in 2 (end of week)

**PR link:** https://github.com/ascherj/pathreview/pull/393

**Branch:** `docs/36-hybrid-retrieval-scoring`

**What you built:**
Added a “Hybrid Retrieval Scoring” subsection to `docs/ARCHITECTURE.md` explaining how PathReview blends vector similarity and BM25 keyword scores. The section documents per-method score normalization, the weighted-sum formula (`hybrid_score = 0.7 × vector_norm + 0.3 × bm25_norm`), the default weights and where they are configured, the minimum-score filter, and a concrete numerical example showing how two chunks are scored and ranked.

**Tests added or updated:**
No new automated test files were added because the implementation in `rag/hybrid.py` was not modified. This contribution only documents the existing retrieval behavior.

To validate the change, I ran the existing unit test suite with `make test-unit`, including the hybrid retrieval tests that cover score normalization, vector and keyword weighting, minimum-score filtering, result ordering, and maximum-chunk limits. I also ran `make check` to verify that the documentation change introduced no formatting, linting, or repository validation errors.

**Test files reviewed:**

* `tests/unit/test_hybrid.py` — verifies the hybrid retriever’s scoring, filtering, ranking, and result-limit behavior.
* `rag/hybrid.py` — reviewed directly to confirm that the documented formula, default weights, threshold, and sorting behavior match the implementation.

**Coverage provided:**
The existing unit tests exercise the implementation described by the new documentation. Since this PR does not change executable code, no new code paths were introduced and the project’s test coverage percentage is unchanged.

**Self-review confirmation:** [x] `make check` passes  [x] `make test-unit` passes

**Draft PR feedback received from:** None
