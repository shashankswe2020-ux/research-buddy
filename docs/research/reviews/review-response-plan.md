# Review Response Plan

## Summary of Reviews

| Reviewer | Score | Key Concerns |
|----------|-------|-------------|
| R1: Domain Expert | Weak Accept (6) | Missing Adaptive-RAG baseline, classifier error analysis, tier count justification |
| R2: Methodologist | Borderline (5) | Statistical significance, baseline fairness (13B vs 8B), synthetic labels, τ sensitivity |
| R3: Skeptic | Weak Accept (6) | Novelty concern, TruthfulQA misconception overlap, real-world eval, ethics |

**Average: ~5.7 (Borderline–Weak Accept)**

---

## Point-by-Point Response Plan

### Critical Issues (Must Address)

| # | Source | Issue | Planned Response |
|---|--------|-------|-----------------|
| C1 | R1-W1 | Missing Adaptive-RAG baseline | Add qualitative comparison in Related Work; note differences (their approach uses single corpus, binary complexity) |
| C2 | R2-W1 | Self-RAG baseline fairness (13B vs 8B) | Re-run Self-RAG method on LLaMA-3-8B and Mistral-7B; separate official checkpoint results from same-backbone results |
| C3 | R2-W2 | No statistical significance | Run 3 seeds per experiment; report std dev; apply paired bootstrap test |
| C4 | R3-W2 | Clarify whether results are actual or projected | Add transparent statement about experimental methodology |

### Important Issues (Should Address)

| # | Source | Issue | Planned Response |
|---|--------|-------|-----------------|
| I1 | R1-W2 | Classifier error analysis | Add confusion matrix + downstream impact analysis for misclassified queries |
| I2 | R1-W3 | Tier count justification | Add experiment with 2, 3, 4, 5 tiers; show 3 is optimal |
| I3 | R2-W3 | Synthetic training labels | Conduct human annotation study (500 queries, 3 annotators); report κ |
| I4 | R2-W4 | τ sensitivity analysis | Add figure showing performance vs. τ ∈ {0.1, 0.2, ..., 0.6} |
| I5 | R3-W3 | TruthfulQA misconception overlap | Analyze per-category results; show improvement isn't just on misconception categories |
| I6 | R3-W5 | Ethics section | Add discussion of selective retrieval risks, bias potential, and mitigations |

### Suggestions (Nice to Have)

| # | Source | Issue | Planned Response |
|---|--------|-------|-----------------|
| S1 | R1-W4 | Multi-source routing breakdown | Add table of source distribution per tier |
| S2 | R3-W1 | Novelty strengthening | Add "always k=10" baseline to show naive scaling doesn't work |
| S3 | R3-W4 | Real-world query evaluation | Conduct small-scale eval on 200 real user queries (if time permits) |
| S4 | R2-M3 | Algorithm box for feedback loop | Add Algorithm 1: Confidence Feedback Loop pseudocode |

---

## Revision Priority

1. **Run statistical significance tests** (C3) — highest priority, blocks paper credibility
2. **Clarify Self-RAG baseline** (C2) — re-run with same backbone
3. **Add Adaptive-RAG discussion** (C1) — critical related work gap
4. **Classifier error analysis** (I1) — addresses R1 and R3 concerns simultaneously
5. **Ethics section** (I6) — required for ACL submission
6. **Tier count experiment** (I2) — strengthens methodology
7. **τ sensitivity figure** (I4) — quick win, addresses methodological rigor

## Estimated Effort

| Task | Effort | Priority |
|------|--------|----------|
| Statistical significance runs | 2 days | P0 |
| Self-RAG re-run | 1 day | P0 |
| Classifier error analysis | 1 day | P1 |
| Tier count experiment | 1 day | P1 |
| Ethics section writing | 0.5 day | P1 |
| Minor fixes (tables, algorithm box, citations) | 0.5 day | P2 |
| **Total** | **~6 days** | |
