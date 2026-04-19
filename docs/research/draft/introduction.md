# 1. Introduction

Large language models have demonstrated remarkable capabilities across a wide
range of natural language tasks, from question answering to summarization and
dialogue (Brown et al., 2020; Touvron et al., 2023). However, their tendency to
generate plausible-sounding but factually incorrect content — commonly termed
"hallucination" — poses a significant barrier to deployment in high-stakes
domains such as healthcare, legal analysis, and education (Huang et al., 2023).
Recent studies estimate that even state-of-the-art models hallucinate on 15–25%
of factual queries, with rates increasing for less common entities and multi-hop
reasoning tasks (Mallen et al., 2023; Min et al., 2023).

Retrieval-augmented generation (RAG) addresses this challenge by grounding the
generation process in externally retrieved evidence (Lewis et al., 2020). By
conditioning output on relevant documents, RAG reduces the model's reliance on
parametric memory and provides verifiable source attribution. The paradigm has
become a standard component in production LLM systems, with widespread adoption
across industry and research.

Despite its success, current RAG implementations suffer from a fundamental
limitation: they apply a **fixed retrieval strategy** regardless of query
characteristics. A simple factual query such as "What is the boiling point of
water?" receives the same retrieval treatment as a complex multi-hop question
like "How did the development of attention mechanisms in NLP influence the
architecture of modern protein folding models?" This one-size-fits-all approach
leads to two failure modes: (1) **under-retrieval** for complex queries, where
insufficient context causes the model to fall back on parametric memory and
hallucinate, and (2) **over-retrieval** for simple queries, where excessive
irrelevant context introduces noise and degrades generation quality (Yoran et
al., 2024).

Recent work has begun addressing the "when to retrieve" question. Self-RAG (Asai
et al., 2024) learns to generate reflection tokens that decide whether retrieval
is needed. FLARE (Jiang et al., 2023) triggers retrieval when the model generates
low-confidence tokens. CRAG (Yan et al., 2024) adds a corrective mechanism that
falls back to web search when initial retrieval fails. However, these approaches
make **binary** retrieval decisions (retrieve or not) and do not adapt **how
much** to retrieve or **from where**. The retrieval depth remains fixed at k
documents from a single corpus.

In this paper, we propose **AdaptRAG**, an adaptive retrieval-augmented
generation framework that addresses these limitations through three novel
components:

- **A query complexity classifier** that categorizes incoming queries into three
  tiers (simple, intermediate, complex) using a fine-tuned DeBERTa model, with
  87% classification accuracy.
- **A 3-tier adaptive retrieval strategy** that adjusts retrieval depth (k=2, 5,
  or 10), re-ranking intensity, and iteration rounds based on the classified tier.
- **Multi-source routing** that directs retrieval to appropriate knowledge sources
  (encyclopedic, academic, or web) based on query domain characteristics.
- **A confidence feedback loop** that detects low-confidence claims in the
  generated output and triggers targeted additional retrieval.

We evaluate AdaptRAG on three established hallucination benchmarks — TruthfulQA
(Lin et al., 2022), HaluEval (Li et al., 2023), and FActScore (Min et al.,
2023) — using LLaMA-3-8B and Mistral-7B as backbone models. Our experiments
demonstrate that AdaptRAG reduces hallucination rates by 12–18% over the
strongest baseline across all benchmarks, with the largest improvements on
complex multi-hop queries.

The remainder of this paper is organized as follows. Section 2 reviews related
work on retrieval-augmented generation and hallucination mitigation. Section 3
describes the AdaptRAG framework in detail. Section 4 presents our experimental
setup, and Section 5 reports results and analysis. Section 6 concludes with a
summary of contributions and directions for future work.
