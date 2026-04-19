# 6. Discussion

## Interpretation of Results

Our results provide strong evidence that query-adaptive retrieval is more
effective than fixed-strategy approaches for hallucination reduction. The
consistent improvements across three benchmarks and two backbone models
(Tables 1–3) suggest that the benefit is not an artifact of a particular
evaluation setup or model architecture.

The most striking finding is the **scaling of improvement with query
complexity** (Table 3). Complex multi-hop queries see a +9.7-point improvement
over Self-RAG, while simple queries improve by +3.7 points. This asymmetric
gain pattern has an intuitive explanation: complex queries require more evidence
to ground the generation, and AdaptRAG's deep multi-source retrieval provides
this evidence, whereas fixed k=5 retrieval leaves gaps. Conversely, simple
queries benefit from *less* retrieval — the k=2 setting in Tier 1 avoids
introducing irrelevant passages that can confuse the generator (consistent with
the findings of Yoran et al., 2024).

## The Role of Query Complexity Classification

The ablation study (Table 2) reveals that the query complexity classifier is the
second most important component (−4.8 points when removed), after adaptive depth
itself. This raises an interesting question: how good must the classifier be to
provide useful adaptation?

Our classifier achieves 87% accuracy. When we artificially degrade classifier
accuracy by injecting random noise, we find that adaptation remains beneficial
down to approximately 70% accuracy, below which the noise from misclassification
outweighs the adaptation benefits. This suggests that even a relatively simple
classifier can drive meaningful retrieval adaptation.

## Comparison with Reactive Approaches

CRAG (Yan et al., 2024) represents the reactive paradigm — retrieve first, then
correct if the retrieval is poor. AdaptRAG represents the proactive paradigm —
predict what the query needs, then retrieve accordingly. Our results suggest that
proactive adaptation is more effective: AdaptRAG outperforms CRAG by 7.5 points
on TruthfulQA (54.6 vs. 47.1) while retrieving fewer documents on average (4.2
vs. 6.2). Proactive strategies avoid the latency cost of corrective re-retrieval
for queries where the right strategy was predictable from the start.

## Threats to Validity

**Internal validity.** Our query complexity labels were derived from existing
dataset characteristics rather than independent human annotation. While we
validated a sample of 500 labels with human annotators (Cohen's κ = 0.81),
the full training set may contain label noise.

**External validity.** All evaluation benchmarks are in English with
Wikipedia-based knowledge sources. The effectiveness of AdaptRAG in low-resource
languages or specialized domains (e.g., biomedical, legal) remains untested.

**Construct validity.** The three-tier complexity classification is a
simplification. Query complexity is likely a continuous spectrum, and the optimal
number of tiers may vary by domain.
