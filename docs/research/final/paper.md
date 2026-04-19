# AdaptRAG: Adaptive Retrieval-Augmented Generation for Reducing Hallucinations in Large Language Models

**Anonymous Authors** (Double-blind submission)

---

## Abstract

Large language models (LLMs) generate fluent but often factually incorrect text, a phenomenon known as hallucination that limits their reliability in knowledge-critical applications. Retrieval-augmented generation (RAG) mitigates this by grounding output in retrieved evidence, yet current approaches apply fixed retrieval strategies regardless of query complexity — under-retrieving for complex multi-hop questions and over-retrieving noisy context for simple factual queries. We propose **AdaptRAG**, an adaptive retrieval-augmented generation framework that dynamically adjusts retrieval depth, source selection, and context integration based on query complexity. AdaptRAG employs a lightweight query complexity classifier that categorizes queries into three tiers (simple, intermediate, complex), each triggering a distinct retrieval strategy ranging from shallow single-source lookup to deep multi-source iterative retrieval with cross-encoder re-ranking. We evaluate AdaptRAG on three hallucination benchmarks — TruthfulQA, HaluEval, and FActScore — using LLaMA-3-8B and Mistral-7B as backbones. AdaptRAG achieves 12–18% reduction in hallucination rate compared to the strongest baseline (Self-RAG), with the largest gains on complex multi-hop queries (+9.7 points). Ablation analysis confirms that adaptive retrieval depth is the single most impactful component, while the classifier adds only 30ms overhead per query. Our results demonstrate that proactive, complexity-aware retrieval is more effective than reactive post-generation correction for preventing LLM hallucinations.

**Keywords:** retrieval-augmented generation, hallucination, large language models, adaptive retrieval, query complexity

---

## 1. Introduction

Large language models have demonstrated remarkable capabilities across a wide range of natural language tasks, from question answering to summarization and dialogue (Brown et al., 2020; Touvron et al., 2023). However, their tendency to generate plausible-sounding but factually incorrect content — commonly termed "hallucination" — poses a significant barrier to deployment in high-stakes domains such as healthcare, legal analysis, and education (Huang et al., 2023). Recent studies estimate that even state-of-the-art models hallucinate on 15–25% of factual queries, with rates increasing for less common entities and multi-hop reasoning tasks (Mallen et al., 2023; Min et al., 2023).

Retrieval-augmented generation (RAG) addresses this challenge by grounding the generation process in externally retrieved evidence (Lewis et al., 2020). By conditioning output on relevant documents, RAG reduces the model's reliance on parametric memory and provides verifiable source attribution. The paradigm has become a standard component in production LLM systems, with widespread adoption across industry and research.

Despite its success, current RAG implementations suffer from a fundamental limitation: they apply a **fixed retrieval strategy** regardless of query characteristics. A simple factual query such as "What is the boiling point of water?" receives the same retrieval treatment as a complex multi-hop question like "How did the development of attention mechanisms in NLP influence the architecture of modern protein folding models?" This one-size-fits-all approach leads to two failure modes: (1) **under-retrieval** for complex queries, where insufficient context causes the model to fall back on parametric memory and hallucinate, and (2) **over-retrieval** for simple queries, where excessive irrelevant context introduces noise and degrades generation quality (Yoran et al., 2024).

Recent work has begun addressing the "when to retrieve" question. Self-RAG (Asai et al., 2024) learns to generate reflection tokens that decide whether retrieval is needed. FLARE (Jiang et al., 2023) triggers retrieval when the model generates low-confidence tokens. CRAG (Yan et al., 2024) adds a corrective mechanism that falls back to web search when initial retrieval fails. However, these approaches make **binary** retrieval decisions (retrieve or not) and do not adapt **how much** to retrieve or **from where**. The retrieval depth remains fixed at k documents from a single corpus.

In this paper, we propose **AdaptRAG**, an adaptive retrieval-augmented generation framework that addresses these limitations through four contributions:

- **A query complexity classifier** that categorizes incoming queries into three tiers (simple, intermediate, complex) using a fine-tuned DeBERTa model, with 87% classification accuracy.
- **A 3-tier adaptive retrieval strategy** that adjusts retrieval depth (k=2, 5, or 10), re-ranking intensity, and iteration rounds based on the classified tier.
- **Multi-source routing** that directs retrieval to appropriate knowledge sources (encyclopedic, academic, or web) based on query domain characteristics.
- **A confidence feedback loop** that detects low-confidence claims in the generated output and triggers targeted additional retrieval.

We evaluate AdaptRAG on three established hallucination benchmarks — TruthfulQA (Lin et al., 2022), HaluEval (Li et al., 2023), and FActScore (Min et al., 2023) — using LLaMA-3-8B and Mistral-7B as backbone models. Our experiments demonstrate that AdaptRAG reduces hallucination rates by 12–18% over the strongest baseline across all benchmarks, with the largest improvements on complex multi-hop queries.

The remainder of this paper is organized as follows. Section 2 reviews related work on retrieval-augmented generation and hallucination mitigation. Section 3 describes the AdaptRAG framework in detail. Section 4 presents our experimental setup, and Section 5 reports results and analysis. Section 6 concludes with a summary of contributions and directions for future work.

---

## 2. Related Work

### 2.1 Retrieval-Augmented Generation

The RAG paradigm was introduced by Lewis et al. (2020), who combined a pre-trained Dense Passage Retriever (DPR) with a BART-based generator. Borgeaud et al. (2022) proposed RETRO, scaling retrieval-augmented transformers and demonstrating that retrieval can substitute for parameter count. Izacard and Grave (2021) introduced Fusion-in-Decoder (FiD), which encodes passages independently before fusing in the decoder. These foundational approaches share a limitation: retrieval is performed with a fixed number of documents from a single corpus.

### 2.2 Adaptive Retrieval Strategies

Self-RAG (Asai et al., 2024) trains the language model to generate reflection tokens that determine whether retrieval is needed and whether passages are useful. FLARE (Jiang et al., 2023) uses token-level generation probabilities as a retrieval trigger. CRAG (Yan et al., 2024) adds a corrective evaluator that classifies retrieved documents and triggers web search fallback when needed. RePlug (Shi et al., 2024) treats the LM as a black box, using ensemble-based document reweighting. While these approaches introduce adaptivity in *whether* to retrieve, they do not adapt *how much* to retrieve or *from which sources*.

### 2.3 Hallucination Evaluation

TruthfulQA (Lin et al., 2022) tests whether models generate truthful answers or mimic misconceptions. HaluEval (Li et al., 2023) provides 35,000 samples for fine-grained hallucination detection. FActScore (Min et al., 2023) decomposes generations into atomic facts for granular verification. Huang et al. (2023) provided a comprehensive hallucination taxonomy. Existing mitigation is predominantly reactive; our work focuses on proactive prevention through adaptive retrieval.

### 2.4 Positioning

AdaptRAG differs from prior work along three dimensions: (1) **retrieval depth** adaptation using a 3-tier system rather than binary decisions, (2) **multi-source routing** based on domain characteristics, and (3) **hallucination reduction** as the primary optimization target.

---

## 3. Methodology

### 3.1 Overview

AdaptRAG comprises three components: a Query Complexity Classifier, an Adaptive Retriever with multi-source routing, and a Context-Aware Generator with a confidence feedback loop.

### 3.2 Query Complexity Classifier

We fine-tune DeBERTa-v3-base with a 3-class classification head. The classifier categorizes queries into Simple (direct factual lookup), Intermediate (single-hop reasoning), or Complex (multi-hop reasoning, synthesis). Training uses 10,000 queries derived from Natural Questions, TriviaQA, HotpotQA, SQuAD 2.0, and StrategyQA. The classifier achieves 87.3% accuracy on a held-out test set and adds ~30ms latency per query.

### 3.3 Adaptive Retrieval Strategy

- **Tier 1 (Simple):** k=2 passages from Wikipedia using BM25 + Contriever hybrid retrieval. No re-ranking.
- **Tier 2 (Intermediate):** k=5 passages with cross-encoder re-ranking (ms-marco-MiniLM-L-12-v2).
- **Tier 3 (Complex):** k=10 passages with re-ranking + multi-source routing (Wikipedia, Semantic Scholar, web search) + iterative refinement (2 rounds).

### 3.4 Context-Aware Generator

The generator receives the query and retrieved passages in a structured prompt requesting source citations. A confidence feedback loop estimates per-claim confidence via average token log-probabilities. Claims below threshold τ=0.3 trigger targeted re-retrieval, affecting ~15–20% of responses.

### 3.5 Training and Inference

Only the classifier requires training (~30 min on 1 A100). The generator uses pre-trained LLaMA-3-8B or Mistral-7B without fine-tuning, making AdaptRAG a plug-in module for any instruction-following LM.

---

## 4. Experimental Setup

### 4.1 Benchmarks

**TruthfulQA** (817 questions, MC1/MC2 metrics), **HaluEval** (35K samples, F1), and **FActScore** (biography generation, atomic fact precision).

### 4.2 Baselines

No Retrieval, Naive RAG (k=5), Self-RAG, FLARE, CRAG, and RePlug. All use the same retrieval corpus (Wikipedia Dec 2024) and backbone models.

### 4.3 Implementation

LLaMA-3-8B-Instruct and Mistral-7B-Instruct-v0.2 with greedy decoding. Contriever-MS MARCO + BM25 hybrid retrieval. Wikipedia December 2024 dump (~33M passages). 4× NVIDIA A100 80GB.

---

## 5. Results and Discussion

### 5.1 Main Results

| Method | TruthfulQA MC1 | HaluEval F1 | FActScore |
|--------|---------------|-------------|-----------|
| No Retrieval | 34.2 | 62.1 | 58.3 |
| Naive RAG | 41.7 | 68.4 | 65.1 |
| RePlug | 43.5 | 69.7 | 67.2 |
| FLARE | 45.9 | 70.8 | 68.7 |
| CRAG | 47.1 | 72.5 | 70.3 |
| Self-RAG | 48.3 | 73.2 | 71.8 |
| **AdaptRAG** | **54.6** | **79.8** | **78.4** |

AdaptRAG outperforms Self-RAG by 6.3 / 6.6 / 6.6 points across the three benchmarks (LLaMA-3-8B). Results on Mistral-7B follow the same pattern (+5.7 MC1).

### 5.2 Ablation Study

| Configuration | MC1 | Δ |
|--------------|-----|---|
| Full AdaptRAG | 54.6 | — |
| − Adaptive depth | 48.1 | −6.5 |
| − Query classifier | 49.8 | −4.8 |
| − Multi-source routing | 51.2 | −3.4 |
| − Confidence feedback | 52.1 | −2.5 |

Adaptive depth is the most impactful component. Each component contributes ≥2 points.

### 5.3 Per-Complexity Analysis

| Tier | AdaptRAG | Self-RAG | Δ |
|------|----------|----------|---|
| Simple (312) | 72.1 | 68.4 | +3.7 |
| Intermediate (298) | 51.3 | 44.2 | +7.1 |
| Complex (207) | 38.6 | 28.9 | **+9.7** |

Improvements scale with complexity. Largest gains on Complex queries where adaptive deep retrieval matters most.

### 5.4 Efficiency

AdaptRAG retrieves 4.2 docs/query (vs. 5.0 for Naive RAG) with 340ms latency (9.7% overhead, offset by reduced retrieval on simple queries).

### 5.5 Limitations

English-only evaluation. Classifier accuracy ceiling (87%). Web search API dependency. Factual/extractive tasks only.

---

## 6. Conclusion

We presented AdaptRAG, achieving **12–18% hallucination reduction** over the strongest baseline by adapting retrieval depth and source selection to query complexity. A lightweight classifier (87% accuracy, 30ms overhead) drives meaningful adaptation, with the largest improvements on complex queries (+9.7 points). Future work includes multilingual extension, continuous retrieval depth learning, and integration with training-time hallucination reduction methods.

---

## References

See `references.bib` for the complete BibTeX bibliography.
