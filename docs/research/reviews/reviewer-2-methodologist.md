# Reviewer 2: The Methodologist

## Overall Score: **Borderline** (5/10)

## Summary
The paper introduces AdaptRAG, which adapts RAG retrieval based on query complexity. While the idea is sound and results are positive, there are methodological concerns around the experimental setup, baseline fairness, and statistical rigor that need to be addressed before publication.

## Strengths

1. **Well-structured methodology.** The three-component architecture (classifier, adaptive retriever, context-aware generator) is clearly described and modular. The architecture diagram is helpful.

2. **Ablation study is informative.** Each component's contribution is isolated, and the ranking (adaptive depth > classifier > multi-source > feedback loop > re-ranking) provides useful design insights.

3. **Efficiency analysis included.** Many RAG papers ignore computational cost. The comparison of documents/query and latency across methods is valuable.

4. **Reproducibility potential.** Implementation details are thorough — specific model names, hyperparameters, hardware, and retrieval corpus are all specified.

## Weaknesses

1. **Baseline fairness concern.** Self-RAG uses a fine-tuned LLaMA-2-13B model (larger than LLaMA-3-8B). Comparing AdaptRAG on 8B against Self-RAG's released 13B checkpoint is not apples-to-apples. The paper mentions evaluating Self-RAG with the same backbones but doesn't clearly distinguish these results.
   - **Suggestion:** Clearly separate results using (a) Self-RAG's official checkpoint and (b) Self-RAG's method applied to LLaMA-3-8B and Mistral-7B. Report both.

2. **No statistical significance testing.** All improvements are reported as point differences without confidence intervals, standard deviations, or significance tests. How many runs were performed? Is the 6.3-point improvement statistically significant?
   - **Suggestion:** Report results over 3-5 runs with standard deviations. Apply paired bootstrap or McNemar's test for significance.

3. **Training data for the complexity classifier is synthetic.** Deriving complexity labels from dataset characteristics (single-entity → Simple, multi-hop → Complex) assumes the dataset structure matches real-world query complexity, which may not hold.
   - **Suggestion:** Validate with a human annotation study. Annotate 500 queries with 3 annotators and report inter-annotator agreement.

4. **Confidence threshold τ selection.** The threshold τ=0.3 is selected via grid search, but the search space and development set are not described. How sensitive is the system to this threshold?
   - **Suggestion:** Add a sensitivity analysis showing performance across the full range of τ values.

5. **Retrieval corpus recency.** Wikipedia December 2024 dump is used, but TruthfulQA questions may reference facts that changed between the benchmark creation (2022) and the corpus snapshot. This could introduce noise.
   - **Suggestion:** Acknowledge this temporal mismatch and discuss its potential impact.

## Questions for Authors

1. Were all baseline methods re-run with the same retrieval corpus, or were published numbers used?
2. How does the confidence feedback loop interact with the complexity tier? Does it trigger more often for Complex queries?
3. What is the computational cost of training the complexity classifier end-to-end (including data preparation)?

## Minor Issues

- Section 3.3: The confidence feedback loop description could use a formal algorithm box
- Table 4: Add FLARE to the efficiency comparison
- "Cohen's κ = 0.81" for label validation — report the full annotation protocol (# annotators, guidelines, resolution process)
