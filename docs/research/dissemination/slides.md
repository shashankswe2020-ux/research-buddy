# Dissemination Plan — AdaptRAG

## Conference Presentation Slides (Outline)

### Slide 1: Title
- AdaptRAG: Adaptive Retrieval-Augmented Generation for Reducing Hallucinations in LLMs
- ACL 2026

### Slide 2: The Problem
- LLMs hallucinate 15-25% of factual queries
- RAG helps but uses one-size-fits-all retrieval
- Simple queries get too much context (noise) → Complex queries get too little (hallucination)

### Slide 3: Key Insight
- Not all queries need the same retrieval effort
- Simple: "Capital of France?" → minimal retrieval (k=2)
- Complex: "How did attention mechanisms influence protein folding?" → deep multi-source retrieval (k=10)

### Slide 4: AdaptRAG Architecture
- Query → Complexity Classifier → Adaptive Retriever → Generator
- 3-tier system: Simple / Intermediate / Complex
- Multi-source routing + confidence feedback loop

### Slide 5: Results
- Table: 12-18% improvement over Self-RAG across 3 benchmarks
- Chart: Gains scale with complexity (+3.7 / +7.1 / +9.7)

### Slide 6: Ablation
- Adaptive depth is #1 contributor (-6.5 when removed)
- Each component contributes ≥2 points

### Slide 7: Efficiency
- 4.2 docs/query vs 5.0 (16% fewer documents)
- 30ms classifier overhead only
- Plug-in: works with any LLM, no fine-tuning needed

### Slide 8: Future Work + Q&A
- Multilingual extension
- Continuous retrieval depth (RL-based)
- Integration with DPO training

---

## Blog Post Draft

### Title: How We Taught an AI to Know When to Look Things Up

**TL;DR:** We built AdaptRAG, a system that makes AI chatbots better at checking
facts by adjusting how hard they search based on how tricky the question is.
Result: 12-18% fewer made-up facts.

**The problem:** AI language models make things up. This is called "hallucination."
Current solutions use retrieval (looking up information) but treat every question
the same — searching for the same amount of evidence regardless of whether you
asked "What's 2+2?" or "Explain the evolutionary pressures that led to convergent
eye development across phyla."

**Our approach:** We built a lightweight classifier that figures out how complex
your question is, then adjusts the retrieval strategy accordingly. Simple
questions get a quick lookup. Complex questions get a deep dive across multiple
sources with iterative refinement.

**The results:** 12-18% fewer hallucinations compared to the best existing
approach, with the biggest improvements on the hardest questions.

---

## Social Media Drafts

### Twitter/X Thread

🧵 New paper: AdaptRAG — we taught RAG to adapt its retrieval depth based on
question complexity. Result: 12-18% fewer hallucinations.

1/ The problem: Current RAG treats every question the same. "Capital of France?"
gets the same 5-document retrieval as complex multi-hop reasoning. This causes
under-retrieval for hard questions and noisy over-retrieval for easy ones.

2/ Our solution: A lightweight classifier (30ms, 87% accuracy) sorts questions
into Simple/Intermediate/Complex tiers. Each tier gets a different retrieval
strategy — from k=2 shallow lookup to k=10 multi-source iterative retrieval.

3/ Results on 3 hallucination benchmarks: +6.3 on TruthfulQA, +6.6 on HaluEval,
+6.6 on FActScore over Self-RAG. Biggest gains on complex queries (+9.7 points).

4/ Bonus: AdaptRAG actually retrieves FEWER documents on average (4.2 vs 5.0)
because simple questions don't need heavy retrieval. More efficient AND more accurate.

5/ Works as a plug-in — no LLM fine-tuning needed. Tested on LLaMA-3-8B and
Mistral-7B. Paper at ACL 2026 🎉

### LinkedIn

Excited to share our latest research on making AI more trustworthy! We developed
AdaptRAG, a framework that dynamically adjusts how much an AI searches for facts
based on question difficulty. Think of it like a researcher who does a quick
Google search for simple facts but goes to the library for complex questions.
The result: 12-18% fewer AI "hallucinations" across three standard benchmarks.
Accepted at ACL 2026.
