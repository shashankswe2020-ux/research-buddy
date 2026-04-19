# 7. Conclusion

We presented **AdaptRAG**, an adaptive retrieval-augmented generation framework
that dynamically adjusts retrieval depth, source selection, and context
integration based on query complexity. By classifying queries into three
complexity tiers and applying tier-specific retrieval strategies — from shallow
single-source lookup for simple queries to deep multi-source iterative retrieval
for complex ones — AdaptRAG reduces hallucination rates by **12–18%** over the
strongest baseline (Self-RAG) across three established benchmarks: TruthfulQA,
HaluEval, and FActScore.

Our key finding is that **not all queries need the same retrieval effort**. A
lightweight query complexity classifier (87% accuracy, 30ms overhead) is
sufficient to drive meaningful adaptation, with the largest improvements on
complex multi-hop queries (+9.7 points) where fixed-strategy RAG struggles most.
The adaptive approach also improves efficiency, retrieving 16% fewer documents on
average than fixed k=5 baselines by using minimal retrieval for simple queries.

We propose three directions for future work. First, extending AdaptRAG to
**multilingual settings**, where retrieval corpus quality varies across languages
and the complexity classifier must handle code-switching and cross-lingual
queries. Second, learning **retrieval depth as a continuous parameter** rather
than discrete tiers, potentially using reinforcement learning to optimize the
retrieval–generation trade-off end-to-end. Third, integrating adaptive retrieval
with **training-time approaches** such as Direct Preference Optimization (DPO)
for hallucination reduction, combining proactive retrieval with parametric
knowledge calibration.
