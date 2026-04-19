# Literature Review: Adaptive Retrieval-Augmented Generation for Reducing Hallucinations in LLMs

## Search Strategy

**Databases:** Google Scholar, Semantic Scholar, arXiv, ACL Anthology, OpenReview
**Queries:** "retrieval augmented generation hallucination", "adaptive RAG", "self-reflective retrieval LLM", "factual grounding language models", "corrective retrieval generation"
**Inclusion:** Peer-reviewed or reputable preprints (2022–2026), English, directly relevant to RAG or LLM hallucination
**Exclusion:** Blog posts, non-peer-reviewed reports, pre-2022 unless seminal

---

## Thematic Organization

### Theme 1: Foundations of Retrieval-Augmented Generation

| Paper | Venue | Year | Key Contribution |
|-------|-------|------|-----------------|
| Lewis et al. | NeurIPS | 2020 | Introduced RAG — combining parametric and non-parametric memory |
| Borgeaud et al. | ICML | 2022 | RETRO — retrieval-enhanced transformers at scale |
| Izacard & Grave | EACL | 2021 | Fusion-in-Decoder — aggregating multiple retrieved passages |

**Lewis et al. (2020)** introduced the RAG paradigm, combining a pre-trained retriever (DPR) with a seq2seq generator. The model retrieves relevant documents at generation time and conditions the output on retrieved context. This foundational work demonstrated significant improvements on knowledge-intensive tasks like open-domain QA and fact verification.

**Borgeaud et al. (2022)** scaled retrieval-augmented models with RETRO, using chunked cross-attention to integrate retrieved neighbors into a frozen transformer. This showed that retrieval can substitute for parameter count, achieving competitive performance with 25× fewer parameters.

**Izacard & Grave (2021)** proposed Fusion-in-Decoder (FiD), which encodes each retrieved passage independently and fuses them in the decoder. FiD scales better with the number of passages than concatenation-based approaches.

**Gap:** All three approaches use static retrieval — the same strategy regardless of query complexity. None adapts retrieval depth or source selection dynamically.

---

### Theme 2: Adaptive and Self-Reflective Retrieval

| Paper | Venue | Year | Key Contribution |
|-------|-------|------|-----------------|
| Asai et al. | ICLR | 2024 | Self-RAG — retrieve, generate, and critique with reflection tokens |
| Jiang et al. | EMNLP | 2023 | FLARE — forward-looking active retrieval |
| Yan et al. | arXiv | 2024 | CRAG — corrective retrieval with evaluator and web search fallback |
| Shi et al. | NAACL | 2024 | RePlug — retrieval-augmented black-box LMs |

**Asai et al. (2024)** introduced Self-RAG, training an LM to generate special reflection tokens that decide when to retrieve, assess relevance of retrieved passages, and critique its own output. Self-RAG achieves significant gains on open-domain QA and long-form generation by selectively retrieving only when needed.

*Strengths:* Learns when to retrieve (not always); self-critique reduces hallucination.
*Limitations:* Requires special token training; binary retrieve/don't-retrieve decision lacks granularity; trained on a single retrieval corpus.

**Jiang et al. (2023)** proposed FLARE (Forward-Looking Active REtrieval), which actively decides when to retrieve by detecting low-confidence tokens during generation. When the model generates a low-probability token, it triggers retrieval using the upcoming sentence as a query.

*Strengths:* No special training required; works with any LM.
*Limitations:* Token-level confidence is a noisy signal; no adaptation of retrieval depth.

**Yan et al. (2024)** introduced CRAG (Corrective RAG), adding a lightweight retrieval evaluator that scores retrieved documents and triggers corrective actions: if documents are irrelevant, CRAG falls back to web search; if ambiguous, it refines the query.

*Strengths:* Corrective mechanism handles retrieval failures gracefully.
*Limitations:* Binary relevant/irrelevant scoring; no multi-level adaptation; web search fallback adds latency.

**Shi et al. (2024)** presented RePlug, treating the LM as a black box and prepending retrieved documents to the input. An ensemble approach reranks documents by their likelihood under the LM.

*Strengths:* Works with any LM without modification.
*Limitations:* Fixed retrieval depth; no query-aware adaptation.

**Gap:** While Self-RAG and FLARE introduce *when-to-retrieve* decisions, none of these approaches adapts *how much* to retrieve (retrieval depth) or *where* to retrieve from (source selection) based on query characteristics. The decision granularity is binary.

---

### Theme 3: Hallucination Detection and Evaluation

| Paper | Venue | Year | Key Contribution |
|-------|-------|------|-----------------|
| Lin et al. | ACL | 2022 | TruthfulQA — benchmark for measuring truthful generation |
| Li et al. | EMNLP | 2023 | HaluEval — large-scale hallucination evaluation |
| Min et al. | EMNLP | 2023 | FActScore — atomic fact-level evaluation of precision |
| Huang et al. | ACM Survey | 2023 | Comprehensive survey on LLM hallucination |

**Lin et al. (2022)** created TruthfulQA, a benchmark of 817 questions designed to elicit common human misconceptions. It measures whether models generate truthful answers rather than mimicking popular falsehoods. TruthfulQA revealed that larger models can be *less* truthful due to learning human misconceptions from training data.

**Li et al. (2023)** introduced HaluEval, a large-scale benchmark with 35,000 samples spanning QA, dialogue, and summarization. It includes both hallucinated and non-hallucinated reference outputs for fine-grained evaluation.

**Min et al. (2023)** proposed FActScore, which decomposes long-form generations into atomic facts and verifies each against a knowledge source. This fine-grained metric provides more actionable feedback than binary hallucination labels.

**Huang et al. (2023)** provided a comprehensive taxonomy of hallucination types (factual, faithful, instruction-following) and mitigation strategies, establishing a common framework for the field.

**Gap:** These benchmarks evaluate hallucination *after* generation. There is limited work on using query-level difficulty signals *before* generation to guide retrieval and prevent hallucination proactively.

---

### Theme 4: Query Complexity and Adaptive Systems

| Paper | Venue | Year | Key Contribution |
|-------|-------|------|-----------------|
| Mallen et al. | ACL | 2023 | When not to trust LMs — popularity-based knowledge assessment |
| Yoran et al. | EMNLP | 2024 | Making retrieval-augmented LMs robust to irrelevant context |
| Wang et al. | NeurIPS | 2023 | Query complexity estimation for information retrieval |

**Mallen et al. (2023)** showed that LLMs' factual knowledge is strongly correlated with entity popularity — models know frequent entities but fail on rare ones. This suggests retrieval should be triggered more aggressively for queries about less popular entities.

**Yoran et al. (2024)** demonstrated that retrieved context can sometimes *hurt* generation quality when irrelevant. They proposed a method to make LMs robust to noisy retrieval, but the better solution is to avoid irrelevant retrieval in the first place.

**Gap:** No existing work combines query complexity classification with adaptive retrieval depth and source selection in a unified framework.

---

## Comparison Table of Key Approaches

| Approach | When to Retrieve | Retrieval Depth | Source Selection | Hallucination Focus | Backbone |
|----------|-----------------|----------------|-----------------|--------------------| ---------|
| Naive RAG | Always | Fixed (k=5) | Single corpus | ❌ Not addressed | Any |
| Self-RAG | Learned (reflection tokens) | Fixed | Single corpus | ✅ Self-critique | Trained |
| FLARE | Low-confidence tokens | Fixed | Single corpus | ⬜ Indirect | Any |
| CRAG | Always + corrective | Fixed | Corpus + web fallback | ⬜ Indirect | Any |
| RePlug | Always | Fixed | Single corpus | ❌ Not addressed | Black-box |
| **AdaptRAG (Ours)** | **Complexity-driven** | **Adaptive (3-tier)** | **Multi-source routing** | **✅ Primary goal** | **Any** |

---

## Identified Research Gaps

1. **No query-adaptive retrieval depth:** All existing approaches use a fixed number
   of retrieved documents regardless of query complexity.

2. **No multi-source routing:** Current systems retrieve from a single corpus.
   Different query types may benefit from different sources (Wikipedia for facts,
   academic papers for methods, web for recent events).

3. **Binary retrieval decisions:** Self-RAG and FLARE decide "retrieve or not" but
   lack granularity in *how much* and *from where*.

4. **Reactive vs. proactive hallucination prevention:** Existing methods detect
   hallucination post-generation. Adapting retrieval based on query characteristics
   is a proactive prevention strategy.

5. **Missing query complexity integration:** While Mallen et al. showed entity
   popularity matters, no system uses a comprehensive query complexity signal to
   drive retrieval strategy.

---

## Summary

The RAG landscape has evolved from static retrieval (Lewis et al., 2020) through
selective retrieval (Self-RAG, FLARE) to corrective retrieval (CRAG). However,
all existing approaches lack multi-dimensional adaptation: adjusting retrieval
depth, source selection, and context integration based on query characteristics.
Our proposed AdaptRAG framework addresses this gap by introducing a query
complexity classifier that drives a 3-tier adaptive retrieval strategy, directly
targeting hallucination reduction as the primary objective.
