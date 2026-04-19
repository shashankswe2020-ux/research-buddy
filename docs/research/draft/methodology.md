# 3. Methodology: The AdaptRAG Framework

We present AdaptRAG, an adaptive retrieval-augmented generation framework that
dynamically adjusts retrieval strategy based on query complexity. The framework
comprises three components: a query complexity classifier (§3.1), an adaptive
retriever with multi-source routing (§3.2), and a context-aware generator with
a confidence feedback loop (§3.3). Figure 1 provides an overview of the
architecture.

```
┌──────────────┐     ┌─────────────────────┐     ┌────────────────────┐
│   Query q    │────▶│ Query Complexity     │────▶│ Adaptive Retriever │
│              │     │ Classifier (DeBERTa) │     │                    │
└──────────────┘     └─────────────────────┘     │ Tier 1: k=2, 1src │
                              │                   │ Tier 2: k=5, rerank│
                     ┌────────┴────────┐          │ Tier 3: k=10, multi│
                     │ Simple │ Inter │ Complex   └────────┬───────────┘
                     └────────────────┘                     │
                                                            ▼
                                                 ┌────────────────────┐
                                          ┌─────▶│ Context-Aware      │
                                          │      │ Generator (LLM)    │
                                          │      └────────┬───────────┘
                                          │               │
                                          │      ┌────────▼───────────┐
                                          │      │ Confidence Check   │
                                          │      │ (per-claim score)  │
                                          │      └────────┬───────────┘
                                          │               │
                                          └───────────────┘
                                          Low-confidence → re-retrieve
```

## 3.1 Query Complexity Classifier

The query complexity classifier assigns each input query q to one of three
complexity tiers: **Simple** (direct factual lookup), **Intermediate**
(single-hop reasoning with context), or **Complex** (multi-hop reasoning,
synthesis, or comparison).

**Architecture.** We fine-tune DeBERTa-v3-base (He et al., 2023) with a
3-class classification head. The model takes the raw query text as input and
outputs a probability distribution over the three tiers.

**Training data.** We construct a 10,000-query training set by aggregating and
re-labeling existing QA datasets:

- **Simple (3,400 queries):** Single-entity factual questions from Natural
  Questions (Kwiatkowski et al., 2019) and TriviaQA (Joshi et al., 2017) that
  require only entity lookup.
- **Intermediate (3,300 queries):** Questions from HotpotQA (Yang et al., 2018)
  requiring 1-hop reasoning, and contextual questions from SQuAD 2.0 (Rajpurkar
  et al., 2018).
- **Complex (3,300 queries):** Multi-hop questions from HotpotQA (2+ hops),
  comparison questions from StrategyQA (Geva et al., 2021), and open-ended
  analytical queries manually curated from academic contexts.

**Training details.** We train for 3 epochs with a learning rate of 2e-5,
batch size 32, and AdamW optimizer. The classifier achieves **87.3% accuracy**
on a held-out test set of 2,000 queries (Simple: 92.1%, Intermediate: 84.7%,
Complex: 85.1%).

**Inference cost.** Classification adds approximately 30ms per query on a single
A100 GPU — negligible compared to the retrieval and generation stages.

## 3.2 Adaptive Retrieval Strategy

Based on the classified complexity tier, AdaptRAG applies a distinct retrieval
strategy:

### Tier 1: Simple Queries (k=2)

For simple factual queries, we retrieve only **2 passages** from the primary
encyclopedic corpus (Wikipedia) using BM25 + Contriever hybrid retrieval (Izacard
et al., 2022). No re-ranking is applied. This minimal retrieval minimizes noise
while providing sufficient grounding for direct factual answers.

### Tier 2: Intermediate Queries (k=5)

For intermediate queries, we retrieve **5 passages** from the primary corpus
using hybrid retrieval, followed by **cross-encoder re-ranking** (Nogueira et
al., 2020) to prioritize the most relevant passages. Re-ranking improves
precision for queries that require contextual understanding beyond keyword
matching.

### Tier 3: Complex Queries (k=10, multi-source, iterative)

For complex queries, we employ a three-stage retrieval process:

1. **Initial retrieval:** Retrieve 10 passages from the primary corpus using
   hybrid retrieval with cross-encoder re-ranking.
2. **Multi-source routing:** Based on domain signals in the query (detected via
   keyword patterns and the classifier's intermediate representations), route
   additional retrieval to specialized sources:
   - Academic queries → Semantic Scholar API
   - Recent events → Web search (Bing API)
   - Technical definitions → domain-specific knowledge bases
3. **Iterative refinement:** Use the initial retrieved context to reformulate the
   query and perform a second retrieval round, capturing information missed in
   the first pass.

### Source Routing Mechanism

We implement source routing using a lightweight rule-based system augmented by
the classifier's penultimate layer representations. Domain-specific keyword lists
(academic terms, temporal markers, technical jargon) provide initial routing
signals, which are refined by a linear probe on the classifier's hidden states.
This avoids the need for a separate domain classifier.

## 3.3 Context-Aware Generator

The generator receives the original query and retrieved passages as input,
formatted as a retrieval-conditioned prompt:

```
Given the following evidence:
[1] {passage_1}
[2] {passage_2}
...
[k] {passage_k}

Answer the following question: {query}
Provide your answer with source citations [N] for each claim.
```

**Confidence feedback loop.** After initial generation, we estimate per-claim
confidence using the model's token-level probabilities. For each atomic claim
in the output (identified via simple sentence segmentation), we compute the
average log-probability of content tokens. Claims with confidence below a
threshold τ = 0.3 are flagged, and targeted retrieval is triggered using the
low-confidence claim as a query. The generator then revises its output
incorporating the additional evidence.

This feedback loop typically adds 1 additional retrieval round for 15–20% of
generated responses, with the remaining 80–85% passing the confidence threshold
on the first attempt.

## 3.4 Training and Inference

**Training.** Only the query complexity classifier requires training (DeBERTa
fine-tuning, ~30 minutes on 1 A100). The retriever components (BM25, Contriever,
cross-encoder) use pre-trained checkpoints. The generator LLMs (LLaMA-3-8B,
Mistral-7B) are used without fine-tuning, making AdaptRAG a **plug-in module**
compatible with any instruction-following LM.

**Inference pipeline.** For a given query:
1. Classify complexity tier (~30ms)
2. Execute tier-appropriate retrieval (~100–500ms depending on tier)
3. Generate response with retrieved context (~200–800ms depending on LM)
4. (Optional) Confidence check and re-retrieval (~200ms if triggered)

Total latency: 330–1,530ms per query, with an average of 340ms (most queries
are Simple or Intermediate).
