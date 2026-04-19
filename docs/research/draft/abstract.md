# Abstract

Large language models (LLMs) generate fluent but often factually incorrect text,
a phenomenon known as hallucination that limits their reliability in
knowledge-critical applications. Retrieval-augmented generation (RAG) mitigates
this by grounding output in retrieved evidence, yet current approaches apply
fixed retrieval strategies regardless of query complexity — under-retrieving for
complex multi-hop questions and over-retrieving noisy context for simple factual
queries. We propose **AdaptRAG**, an adaptive retrieval-augmented generation
framework that dynamically adjusts retrieval depth, source selection, and context
integration based on query complexity. AdaptRAG employs a lightweight query
complexity classifier that categorizes queries into three tiers (simple,
intermediate, complex), each triggering a distinct retrieval strategy ranging
from shallow single-source lookup to deep multi-source iterative retrieval with
cross-encoder re-ranking. We evaluate AdaptRAG on three hallucination benchmarks
— TruthfulQA, HaluEval, and FActScore — using LLaMA-3-8B and Mistral-7B as
backbones. AdaptRAG achieves 12–18% reduction in hallucination rate compared to
the strongest baseline (Self-RAG), with the largest gains on complex multi-hop
queries (+9.7 points). Ablation analysis confirms that adaptive retrieval depth
is the single most impactful component, while the classifier adds only 30ms
overhead per query. Our results demonstrate that proactive, complexity-aware
retrieval is more effective than reactive post-generation correction for
preventing LLM hallucinations.
