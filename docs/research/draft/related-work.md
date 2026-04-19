# 2. Related Work

We organize related work into three areas: retrieval-augmented generation,
adaptive retrieval strategies, and hallucination evaluation.

## 2.1 Retrieval-Augmented Generation

The RAG paradigm was introduced by Lewis et al. (2020), who combined a
pre-trained Dense Passage Retriever (DPR) with a BART-based generator to
condition text generation on retrieved documents. This approach demonstrated
significant improvements on open-domain question answering, fact verification,
and knowledge-intensive generation tasks compared to closed-book models.

Subsequent work extended the paradigm in several directions. Borgeaud et al.
(2022) proposed RETRO, scaling retrieval-augmented transformers to 7.5 billion
parameters and demonstrating that retrieval can effectively substitute for model
size — achieving comparable performance to a 25× larger non-retrieval model.
Izacard and Grave (2021) introduced Fusion-in-Decoder (FiD), which encodes each
retrieved passage independently before fusing them in the decoder, enabling
better scaling with the number of passages.

These foundational approaches share a common limitation: retrieval is performed
with a fixed number of documents (typically k=5 or k=10) and from a single
corpus, regardless of query characteristics.

## 2.2 Adaptive Retrieval Strategies

Recent work has introduced varying degrees of adaptivity into the retrieval
process. Self-RAG (Asai et al., 2024) trains the language model to generate
special reflection tokens — `[Retrieve]`, `[IsRelevant]`, `[IsSupported]` — that
determine whether retrieval is needed and whether retrieved passages are useful.
This enables selective retrieval, where the model only retrieves when its
parametric knowledge is insufficient.

FLARE (Jiang et al., 2023) takes a different approach, using token-level
generation probabilities as a signal for retrieval. When the model generates
low-probability tokens — indicating uncertainty — FLARE triggers retrieval using
the upcoming sentence as a query. This forward-looking strategy requires no
special training but relies on probability calibration.

CRAG (Yan et al., 2024) adds a corrective dimension by evaluating the quality
of retrieved documents. A lightweight evaluator classifies retrieved documents as
correct, incorrect, or ambiguous, triggering different actions: direct use,
web search fallback, or query refinement, respectively.

RePlug (Shi et al., 2024) treats the language model as a black box, prepending
retrieved documents to the input and using an ensemble strategy to reweight
documents by their likelihood under the LM.

While these approaches introduce adaptivity in *whether* to retrieve, they do
not adapt *how much* to retrieve. The retrieval depth (number of documents) and
source (which corpus) remain fixed. Table 1 in Section 3 provides a detailed
comparison highlighting this gap.

## 2.3 Hallucination Evaluation and Mitigation

The study of hallucination in LLMs has produced several evaluation benchmarks and
taxonomies. Lin et al. (2022) introduced TruthfulQA, a benchmark of 817
questions specifically designed to test whether models generate truthful answers
or mimic common human misconceptions. Notably, larger models sometimes perform
*worse* on TruthfulQA, suggesting that scale alone does not solve hallucination.

Li et al. (2023) created HaluEval, a large-scale benchmark with 35,000 samples
covering question answering, dialogue, and summarization. HaluEval provides both
hallucinated and non-hallucinated reference outputs, enabling fine-grained
detection evaluation.

Min et al. (2023) proposed FActScore, which decomposes long-form generations
into atomic facts and verifies each against a knowledge source. This granular
evaluation reveals that even high-quality generations contain a mix of supported
and unsupported claims.

Huang et al. (2023) provided a comprehensive survey classifying hallucinations
into factual (incorrect world knowledge), faithful (inconsistent with input), and
instruction-following (deviating from task requirements) categories.

Mitigation strategies in prior work are predominantly *reactive* — detecting and
correcting hallucination after generation. In contrast, our work focuses on
*proactive* prevention by adapting retrieval strategy to query complexity before
generation occurs.

## 2.4 Positioning of Our Work

AdaptRAG differs from prior work along three dimensions. First, we adapt
**retrieval depth** using a 3-tier system rather than making a binary
retrieve/don't-retrieve decision. Second, we introduce **multi-source routing**,
directing queries to appropriate knowledge sources based on domain
characteristics. Third, our primary optimization target is **hallucination
reduction**, directly measured across multiple established benchmarks, rather
than general QA accuracy.
