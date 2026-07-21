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

### Issue selection notes — “Is this right for me?”

This issue is appropriately scoped for a first contribution because it requires a focused documentation change 
rather than a broad code refactor. The relevant implementation is contained in the RAG retrieval files, and the 
issue identifies `docs/ARCHITECTURE.md` as the file to update. I verified the formula and default weights directly from `rag/hybrid.py`, so the documentation can accurately reflect the current behavior. The main risk is documenting assumptions instead of the implementation, which I addressed by reviewing the vector, BM25, normalization, weighting, filtering, and sorting logic before writing the explanation.
