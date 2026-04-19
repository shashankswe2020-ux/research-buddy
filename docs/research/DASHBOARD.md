# 📊 Research Dashboard

> **Paper:** AdaptRAG — Adaptive Retrieval-Augmented Generation for Reducing Hallucinations in LLMs
> **Target Venue:** ACL 2026 (ARR) · Long Paper · Double-Blind
> **Last Updated:** 19 April 2026

---

## 🧭 Phase Progress

| # | Phase | Status | Key Output | Gate |
|---|-------|--------|------------|------|
| 1 | Idea & Scope | ✅ Complete | [`research-brief.md`](research-brief.md) | ✅ Passed |
| 2 | Literature Review | ✅ Complete | [`literature-review.md`](literature-review.md) | ✅ Passed |
| 3 | Outline & Structure | ✅ Complete | [`paper-outline.md`](paper-outline.md) | ✅ Passed |
| 4 | Section Drafting | ✅ Complete | [`draft/`](draft/) (7 sections) | ✅ Passed |
| 5 | Citation Management | ✅ Complete | [`references.bib`](references.bib) · [`references.md`](references.md) | ✅ Passed |
| 6 | Revision & Polishing | ✅ Complete | [`revision-notes.md`](revision-notes.md) | ✅ Passed |
| 7 | Peer Review Simulation | ✅ Complete | [`reviews/`](reviews/) (3 reviewers + response plan) | ✅ Passed |
| 8 | Final Assembly | ✅ Complete | [`final/paper.tex`](final/paper.tex) · [`final/paper.md`](final/paper.md) | ✅ Passed |
| 9 | Post-Submission | ✅ Complete | [`submissions/`](submissions/) · [`dissemination/`](dissemination/) | ✅ Passed |

**Overall Progress: █████████████████████████ 9/9 phases (100%)**

---

## 📚 Citation Library (20 references)

| # | Key | Authors | Year | Venue | Category |
|---|-----|---------|------|-------|----------|
| 1 | `Lewis2020RAG` | Lewis et al. | 2020 | NeurIPS | Core RAG |
| 2 | `Asai2024SelfRAG` | Asai et al. | 2024 | ICLR | Adaptive RAG |
| 3 | `Yan2024CRAG` | Yan et al. | 2024 | arXiv | Adaptive RAG |
| 4 | `Jiang2023FLARE` | Jiang et al. | 2023 | EMNLP | Adaptive RAG |
| 5 | `Shi2024RePlug` | Shi et al. | 2024 | NAACL | Core RAG |
| 6 | `Lin2022TruthfulQA` | Lin et al. | 2022 | ACL | Benchmark |
| 7 | `Li2023HaluEval` | Li et al. | 2023 | EMNLP | Benchmark |
| 8 | `Min2023FActScore` | Min et al. | 2023 | EMNLP | Benchmark |
| 9 | `Huang2023HallucinationSurvey` | Huang et al. | 2023 | ACM Surv. | Hallucination |
| 10 | `Gao2024RAGSurvey` | Gao et al. | 2024 | arXiv | Survey |
| 11 | `Borgeaud2022RETRO` | Borgeaud et al. | 2022 | ICML | Core RAG |
| 12 | `Izacard2021FiD` | Izacard & Grave | 2021 | EACL | Core RAG |
| 13 | `Mallen2023Popularity` | Mallen et al. | 2023 | ACL | Hallucination |
| 14 | `Yoran2024Robust` | Yoran et al. | 2024 | EMNLP | Robustness |
| 15 | `Brown2020GPT3` | Brown et al. | 2020 | NeurIPS | Foundation Model |
| 16 | `Touvron2023LLaMA` | Touvron et al. | 2023 | arXiv | Foundation Model |
| 17 | `Izacard2022Contriever` | Izacard et al. | 2022 | TMLR | Retrieval |
| 18 | `Nogueira2020CrossEncoder` | Nogueira et al. | 2020 | EMNLP | Retrieval |
| 19 | `Yang2018HotpotQA` | Yang et al. | 2018 | EMNLP | Benchmark |
| 20 | `Geva2021StrategyQA` | Geva et al. | 2021 | TACL | Benchmark |

### By Category

```
Core RAG          ████████████  4 refs   (Lewis, Shi, Borgeaud, Izacard'21)
Adaptive RAG      █████████     3 refs   (Asai, Yan, Jiang)
Hallucination     ██████        2 refs   (Huang, Mallen)
Benchmarks        ████████████  4 refs   (Lin, Li, Min, Yang, Geva)
Retrieval         ██████        2 refs   (Izacard'22, Nogueira)
Foundation Models ██████        2 refs   (Brown, Touvron)
Surveys           ██████        2 refs   (Gao, Huang)
Robustness        ███           1 ref    (Yoran)
```

---

## 📝 Drafted Sections

| Section | File | Word Count (est.) | Status |
|---------|------|--------------------|--------|
| Abstract | [`draft/abstract.md`](draft/abstract.md) | ~250 | ✅ Final |
| Introduction | [`draft/introduction.md`](draft/introduction.md) | ~1,200 | ✅ Final |
| Related Work | [`draft/related-work.md`](draft/related-work.md) | ~1,000 | ✅ Final |
| Methodology | [`draft/methodology.md`](draft/methodology.md) | ~1,500 | ✅ Final |
| Experiments | [`draft/experiments.md`](draft/experiments.md) | ~1,200 | ✅ Final |
| Discussion | [`draft/discussion.md`](draft/discussion.md) | ~800 | ✅ Final |
| Conclusion | [`draft/conclusion.md`](draft/conclusion.md) | ~400 | ✅ Final |

**Estimated Total: ~6,350 words · 8 pages (excl. references)**

---

## 🔍 Peer Review Summary

| Reviewer | Role | Score | Recommendation | Key Concern |
|----------|------|-------|----------------|-------------|
| Reviewer 1 | Domain Expert | 7/10 | Weak Accept | Generalization to non-English domains |
| Reviewer 2 | Methodologist | 6/10 | Borderline | Statistical significance tests needed |
| Reviewer 3 | Skeptic | 5/10 | Weak Reject | Real-world deployment feasibility |

**Response Plan:** [`reviews/review-response-plan.md`](reviews/review-response-plan.md)

---

## 📦 Submission Status

| Field | Value |
|-------|-------|
| **Venue** | ACL 2026 (ARR) |
| **Track** | Main Conference — Long Paper |
| **Format** | ACL 2026 LaTeX style |
| **Anonymized** | Yes (double-blind) |
| **Submission Date** | February 2026 |
| **Notification** | May 2026 (est.) |
| **Camera-Ready** | June 2026 (est.) |

---

## 🗂️ All Artifacts

```
docs/research/
├── DASHBOARD.md                 ← You are here
├── research-brief.md            Phase 1: Idea & Scope
├── literature-review.md         Phase 2: Literature Review
├── paper-outline.md             Phase 3: Outline & Structure
├── draft/                       Phase 4: Section Drafting
│   ├── abstract.md
│   ├── introduction.md
│   ├── related-work.md
│   ├── methodology.md
│   ├── experiments.md
│   ├── discussion.md
│   └── conclusion.md
├── references.bib               Phase 5: Citations (BibTeX)
├── references.md                Phase 5: Citations (formatted)
├── revision-notes.md            Phase 6: Revision Notes
├── reviews/                     Phase 7: Peer Review Simulation
│   ├── reviewer-1-domain-expert.md
│   ├── reviewer-2-methodologist.md
│   ├── reviewer-3-skeptic.md
│   └── review-response-plan.md
├── final/                       Phase 8: Final Assembly
│   ├── paper.md
│   ├── paper.tex
│   └── references.bib
├── submissions/                 Phase 9: Post-Submission
│   └── submission-log.md
├── dissemination/               Phase 9: Dissemination
│   └── slides.md
└── decisions/                   Research Decision Records
    └── rdr-001.md
```

---

## ⚡ Quick Commands

| Command | Description |
|---------|-------------|
| `@research-paper status` | Refresh phase status and readiness gates |
| `@research-paper advance` | Attempt to advance to next phase |
| `@research-paper review` | Run peer review simulation |
| `@research-paper cite <query>` | Search and add a citation |
| `@research-paper compile` | Compile final LaTeX paper |

---

*Dashboard auto-generated by Research Buddy. Open [`DASHBOARD.md`](DASHBOARD.md) for the latest view.*
