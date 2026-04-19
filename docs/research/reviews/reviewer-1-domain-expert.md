# Reviewer 1: The Domain Expert

## Overall Score: **Weak Accept** (6/10)

## Summary
This paper presents AdaptRAG, a framework that adapts retrieval depth and source selection based on query complexity to reduce hallucinations in LLMs. The idea is well-motivated and the results are strong across three benchmarks. The work addresses a genuine gap in the RAG literature — the lack of query-adaptive retrieval strategies.

## Strengths

1. **Clear motivation and well-identified gap.** The paper convincingly argues that fixed-strategy RAG fails both simple and complex queries for different reasons. The under-retrieval/over-retrieval framing is intuitive and well-supported.

2. **Comprehensive evaluation.** Three hallucination benchmarks, two backbone models, ablation study, and efficiency analysis. The evaluation is thorough by conference standards.

3. **Strong empirical results.** 12-18% improvement over Self-RAG is substantial. The per-tier analysis (Table 3) effectively demonstrates *where* the improvements come from.

4. **Practical design.** The classifier adds only 30ms overhead, and AdaptRAG works as a plug-in without generator fine-tuning. This is deployment-friendly.

5. **Honest limitations section.** The authors acknowledge English-only evaluation, classifier accuracy ceiling, and API dependency for web search.

## Weaknesses

1. **Missing key baselines.** The paper does not compare with Adaptive-RAG (Jeong et al., 2024) which also uses query complexity for retrieval adaptation. This is a closely related concurrent work that must be discussed and compared against.
   - **Suggestion:** Add Adaptive-RAG as a baseline or provide a detailed qualitative comparison explaining the differences.

2. **Query complexity classifier evaluation is thin.** The 87% accuracy is reported but there is no error analysis. Which queries get misclassified? What happens to downstream performance for misclassified queries?
   - **Suggestion:** Add a confusion matrix and analyze error cases. Show downstream impact of misclassification per tier.

3. **Three-tier choice is not well justified.** Why three tiers and not two or four? The paper acknowledges this as a continuous spectrum in limitations but doesn't explore it experimentally.
   - **Suggestion:** Add an experiment varying the number of tiers (2, 3, 4, 5) and show the sweet spot.

4. **Multi-source routing evaluation is weak.** The ablation shows multi-source routing contributes -3.4 points, but there's no breakdown of which sources help for which query types.
   - **Suggestion:** Add a table showing retrieval source distribution by tier and its impact on accuracy.

## Questions for Authors

1. How does AdaptRAG perform on the full HaluEval benchmark (including summarization), not just QA and dialogue?
2. What is the classifier's performance on out-of-distribution queries (e.g., code-related or mathematical queries)?
3. Have you considered using the LLM itself for complexity classification (zero-shot) instead of a separate classifier?

## Minor Issues

- Table 1: Consider adding standard deviations across multiple runs
- The related work section could cite Trivedi et al. (2023) on interleaving retrieval with chain-of-thought
- "approximately 33 million passages" — please provide the exact count
