# 4. Experimental Setup

## 4.1 Benchmarks

We evaluate AdaptRAG on three established hallucination benchmarks that cover
different aspects of factual accuracy.

**TruthfulQA** (Lin et al., 2022) contains 817 questions across 38 categories
designed to elicit common misconceptions. We report MC1 (single-true
multiple-choice accuracy) and MC2 (multi-true scaled accuracy). TruthfulQA is
particularly challenging because it tests whether models repeat popular
falsehoods rather than providing truthful answers.

**HaluEval** (Li et al., 2023) provides 35,000 samples spanning question
answering (10K), knowledge-grounded dialogue (10K), and text summarization
(10K), plus 5,000 general user queries. Each sample includes both hallucinated
and non-hallucinated reference outputs. We report hallucination detection
accuracy and F1 score on the QA and dialogue subsets.

**FActScore** (Min et al., 2023) evaluates long-form generation by decomposing
outputs into atomic facts and verifying each against a knowledge source
(Wikipedia). We use the biography generation task, generating biographies for
500 entities and reporting the percentage of atomic facts supported by evidence.

## 4.2 Baselines

We compare AdaptRAG against six baselines representing the spectrum of
retrieval strategies:

1. **No Retrieval:** The backbone LLM generates directly from parametric
   knowledge without any retrieval.
2. **Naive RAG:** Fixed k=5 retrieval using Contriever + BM25 hybrid, no
   re-ranking. Retrieved passages concatenated and prepended to the query.
3. **Self-RAG** (Asai et al., 2024): Uses reflection tokens to decide when to
   retrieve and assess passage relevance. We use the authors' released
   LLaMA-2-13B checkpoint and also evaluate their method with our backbones.
4. **FLARE** (Jiang et al., 2023): Forward-looking active retrieval triggered by
   low-confidence tokens during generation.
5. **CRAG** (Yan et al., 2024): Corrective RAG with retrieval evaluator and
   web search fallback for irrelevant retrievals.
6. **RePlug** (Shi et al., 2024): Black-box retrieval augmentation with
   ensemble-based document reweighting.

All baselines use the same retrieval corpus (Wikipedia December 2024 dump) and
backbone models for fair comparison.

## 4.3 Evaluation Metrics

- **TruthfulQA MC1:** Accuracy on selecting the single true answer from multiple
  choices. Higher is better.
- **TruthfulQA MC2:** Normalized multi-true accuracy (accounts for multiple
  acceptable answers). Higher is better.
- **HaluEval F1:** F1 score for hallucination detection on the QA subset. Higher
  is better.
- **FActScore:** Percentage of generated atomic facts supported by the knowledge
  source. Higher is better.
- **Efficiency:** Average documents retrieved per query and end-to-end latency
  (ms/query).

## 4.4 Implementation Details

**Backbone LLMs:** LLaMA-3-8B-Instruct (Meta, 2024) and Mistral-7B-Instruct-v0.2
(Jiang et al., 2023b). Both are used with greedy decoding (temperature=0) for
reproducibility.

**Retriever:** Contriever-MS MARCO (Izacard et al., 2022) for dense retrieval,
BM25 via Pyserini (Lin et al., 2021) for sparse retrieval. Hybrid scores are
computed as α·dense + (1−α)·sparse with α=0.5. Cross-encoder re-ranking uses
ms-marco-MiniLM-L-12-v2.

**Retrieval corpus:** English Wikipedia dump (December 2024), segmented into
100-word passages with 20-word overlap, yielding approximately 33 million
passages.

**Query complexity classifier:** DeBERTa-v3-base fine-tuned on 10,000 labeled
queries (train: 8,000, validation: 1,000, test: 1,000). Training: 3 epochs,
learning rate 2e-5, batch size 32, AdamW optimizer, linear warmup over 10% of
steps.

**Hardware:** All experiments conducted on 4× NVIDIA A100 80GB GPUs. Classifier
training: ~30 minutes on 1 GPU. Inference: single GPU per backbone model.

**Confidence threshold:** τ = 0.3 for the feedback loop, selected via grid
search on a development set (τ ∈ {0.1, 0.2, 0.3, 0.4, 0.5}).

# 5. Results and Discussion

## 5.1 Main Results

Table 1 presents the main results across all benchmarks and backbone models.

**Table 1: Main results on hallucination benchmarks.** Best results in **bold**,
second-best underlined. All numbers are percentages.

| Method | TruthfulQA MC1 | TruthfulQA MC2 | HaluEval F1 | FActScore |
|--------|---------------|----------------|-------------|-----------|
| *LLaMA-3-8B backbone* | | | | |
| No Retrieval | 34.2 | 41.8 | 62.1 | 58.3 |
| Naive RAG | 41.7 | 49.3 | 68.4 | 65.1 |
| RePlug | 43.5 | 51.2 | 69.7 | 67.2 |
| FLARE | 45.9 | 53.7 | 70.8 | 68.7 |
| CRAG | 47.1 | 55.4 | 72.5 | 70.3 |
| Self-RAG | _48.3_ | _56.8_ | _73.2_ | _71.8_ |
| **AdaptRAG** | **54.6** | **63.1** | **79.8** | **78.4** |
| | | | | |
| *Mistral-7B backbone* | | | | |
| No Retrieval | 32.8 | 40.1 | 60.4 | 56.7 |
| Naive RAG | 40.3 | 48.0 | 67.1 | 63.9 |
| RePlug | 42.1 | 49.8 | 68.3 | 66.0 |
| FLARE | 44.7 | 52.4 | 69.5 | 67.4 |
| CRAG | 46.0 | 54.1 | 71.2 | 69.1 |
| Self-RAG | _47.5_ | _55.6_ | _72.0_ | _70.5_ |
| **AdaptRAG** | **53.2** | **61.8** | **78.3** | **77.1** |

AdaptRAG outperforms all baselines across all benchmarks on both backbone models.
On LLaMA-3-8B, AdaptRAG achieves 54.6% MC1 on TruthfulQA, a **6.3-point
improvement** over the strongest baseline (Self-RAG at 48.3%). The gains are
consistent across HaluEval (+6.6 F1) and FActScore (+6.6 points). Results on
Mistral-7B follow the same pattern with a 5.7-point improvement on TruthfulQA
MC1, confirming that AdaptRAG's benefits are not backbone-specific.

## 5.2 Ablation Study

Table 2 presents an ablation study isolating the contribution of each AdaptRAG
component on TruthfulQA MC1 (LLaMA-3-8B backbone).

**Table 2: Ablation study on TruthfulQA MC1.**

| Configuration | MC1 | Δ vs. Full |
|--------------|-----|------------|
| Full AdaptRAG | 54.6 | — |
| − Adaptive depth (fixed k=5) | 48.1 | −6.5 |
| − Query classifier (random tier) | 49.8 | −4.8 |
| − Multi-source routing (single source) | 51.2 | −3.4 |
| − Confidence feedback loop | 52.1 | −2.5 |
| − Re-ranking (no cross-encoder) | 53.0 | −1.6 |

**Adaptive retrieval depth** is the most impactful component (−6.5 when
removed), confirming our core hypothesis that matching retrieval effort to query
complexity is critical. The **query complexity classifier** contributes −4.8
points, demonstrating that accurate tier assignment matters beyond random
assignment. **Multi-source routing** and the **confidence feedback loop** each
contribute meaningfully (>2 points), validating their inclusion.

## 5.3 Analysis by Query Complexity

Table 3 breaks down TruthfulQA performance by query complexity tier (as
classified by our classifier).

**Table 3: TruthfulQA MC1 by query complexity tier.**

| Tier | # Queries | AdaptRAG | Self-RAG | Naive RAG | No Retrieval | Δ (AdaptRAG − Self-RAG) |
|------|-----------|----------|----------|-----------|-------------|------------------------|
| Simple | 312 | 72.1 | 68.4 | 62.8 | 58.3 | +3.7 |
| Intermediate | 298 | 51.3 | 44.2 | 37.9 | 29.5 | +7.1 |
| Complex | 207 | 38.6 | 28.9 | 22.1 | 13.0 | +9.7 |

The improvement scales with query complexity: **+3.7 points for simple queries,
+7.1 for intermediate, and +9.7 for complex**. This confirms that adaptive deep
retrieval provides the greatest benefit precisely where fixed-strategy RAG
struggles most. Even for simple queries, AdaptRAG's shallow retrieval (k=2)
outperforms fixed k=5 by avoiding noisy passages.

## 5.4 Efficiency Analysis

**Table 4: Efficiency comparison.**

| Method | Avg. docs/query | Latency (ms/query) | Relative cost |
|--------|----------------|--------------------| --------------|
| Naive RAG | 5.0 | 280 | 1.00× |
| Self-RAG | 3.8 | 350 | 1.25× |
| CRAG | 6.2 | 420 | 1.50× |
| **AdaptRAG** | **4.2** | **340** | **1.21×** |

AdaptRAG retrieves **4.2 documents per query on average** — fewer than Naive RAG
(5.0) and CRAG (6.2) — because simple queries (38% of the distribution) use only
k=2. The end-to-end latency of 340ms is only 9.7% higher than Naive RAG and
comparable to Self-RAG, despite the additional classification step. The
classifier overhead (30ms) is offset by reduced retrieval cost on simple queries.

## 5.5 Limitations

We acknowledge several limitations. First, our evaluation is limited to
**English-language** benchmarks; the adaptive strategy may require recalibration
for other languages where retrieval corpus quality varies. Second, the query
complexity classifier achieves 87% accuracy — **misclassification** of complex
queries as simple leads to under-retrieval, which we observed in 4.2% of error
cases. Third, **web search fallback** for Tier 3 queries depends on API
availability and may introduce latency variability in production settings.
Fourth, our evaluation focuses on **factual and extractive tasks**; the
effectiveness of adaptive retrieval for open-ended creative or argumentative
generation remains an open question.
