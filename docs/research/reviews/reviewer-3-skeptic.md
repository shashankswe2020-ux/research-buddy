# Reviewer 3: The Skeptic

## Overall Score: **Weak Accept** (6/10)

## Summary
AdaptRAG proposes adapting RAG retrieval depth based on query complexity. The results look good on paper, but I question whether the contribution is sufficiently novel beyond combining existing techniques (query classification + variable-k retrieval + multi-source) and whether the gains justify the added complexity.

## Strengths

1. **Simple and practical.** The core idea — match retrieval effort to query difficulty — is intuitive and easy to implement. The 30ms overhead for the classifier is negligible.

2. **Results are consistent.** Improvements hold across 3 benchmarks, 2 backbones, and the ablation decomposition is clean. This isn't a lucky result on one dataset.

3. **The per-complexity-tier analysis is the paper's strongest contribution.** Table 3 convincingly shows that the benefit scales with query difficulty, providing insight beyond aggregate numbers.

4. **Good efficiency story.** Retrieving fewer documents on average while getting better results is a compelling practical argument.

## Weaknesses

1. **Novelty concern: this is largely an engineering contribution.** The individual components — query classification, variable retrieval depth, multi-source routing, confidence thresholding — are all well-known techniques. The contribution is their combination, which some may view as incremental.
   - **Suggestion:** Strengthen the novelty claim by showing that naive combination (e.g., just adding more documents for harder queries) doesn't work. Show that the specific design choices matter.

2. **The "results" are simulated/illustrative.** The paper reads as if actual experiments were run, but given the context of this being a demo paper, it's unclear whether these are actual experimental results or projections. This needs to be transparently stated.
   - **Suggestion:** Clearly mark whether results are from actual experiments or are projected/estimated.

3. **Over-reliance on TruthfulQA.** TruthfulQA tests misconceptions, not general factual recall. A query about "the Great Wall of China being visible from space" (a common misconception) has little to do with retrieval depth. The improvement on TruthfulQA may reflect the classifier learning to detect misconception-prone queries rather than true complexity-based adaptation.
   - **Suggestion:** Add analysis on what types of TruthfulQA questions benefit most. Are they truly "complex" queries or something else?

4. **Missing real-world evaluation.** All three benchmarks are academic. How does AdaptRAG perform on real user queries (e.g., from search logs or chatbot interactions)?
   - **Suggestion:** Evaluate on a sample of real-world queries, even if small-scale.

5. **Ethical considerations missing.** The paper doesn't discuss potential misuse (e.g., selective retrieval could be manipulated to bias outputs) or the ethical implications of deploying systems that claim to reduce hallucination.
   - **Suggestion:** Add an ethics section discussing potential risks and mitigations.

## Questions for Authors

1. What happens when the classifier is wrong? Specifically, if a Complex query is classified as Simple and gets k=2 retrieval — how bad is the failure mode?
2. Could you achieve similar results by simply using k=10 for all queries and relying on re-ranking to filter? What's the actual performance of a "always deep retrieval" baseline?
3. Is the query complexity classifier domain-specific? Would it need retraining for a medical or legal QA application?

## Minor Issues

- The paper title includes "Large Language Models" which is becoming a cliché — consider a more specific title
- Abstract mentions "12-18% reduction" but the actual numbers in Table 1 are point improvements (6.3-6.6 points), not percentages of the baseline. Clarify the metric.
- Missing acknowledgments section
