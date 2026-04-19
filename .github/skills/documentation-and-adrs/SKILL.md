---
name: documentation-and-adrs
description: Records research methodology decisions and documentation. Use when making structural decisions about the paper, choosing between analysis approaches, selecting venues, or when you need to record rationale that collaborators and future reviewers will need to understand the research choices.
---

# Documentation and Decision Records

## Overview

Document decisions, not just the paper. The most valuable documentation captures
the *why* — the context, constraints, and trade-offs that led to a research
decision. The paper shows *what* was found; decision records explain *why this
approach was chosen* and *what alternatives were considered*.

## When to Use

- Choosing between research methodologies or analysis approaches
- Selecting a target venue or paper type
- Deciding which baselines, datasets, or metrics to use
- Choosing a framing or contribution angle
- When collaborators need context on why a decision was made
- When you find yourself re-debating the same methodological choice

**When NOT to use:** Don't document obvious choices. Don't write decision records
for trivial formatting decisions or self-evident selections.

## Research Decision Records (RDRs)

RDRs capture the reasoning behind significant research decisions. They prevent
re-litigating the same choices and help collaborators understand the rationale.

### When to Write an RDR

- Choosing between competing methodologies
- Selecting datasets or benchmarks for evaluation
- Deciding on evaluation metrics and success thresholds
- Choosing the paper type (empirical, survey, position)
- Selecting the target venue
- Framing the contribution angle
- Any decision that shapes the paper's direction

### RDR Template

Store RDRs in `docs/research/decisions/` with sequential numbering:

```markdown
# RDR-001: Use Transformer architecture as baseline

## Status
Accepted | Superseded by RDR-XXX | Deprecated

## Date
2026-04-19

## Context
We need to select baselines for comparing our proposed method on image
classification. Key requirements:
- Baselines must be state-of-the-art within the last 2 years
- Results must be reproducible with publicly available code
- Baselines should span different architectural families for fair comparison

## Decision
Use ViT-B/16, ResNet-50, and ConvNeXt-T as primary baselines.

## Alternatives Considered

### Only CNN baselines (ResNet, EfficientNet)
- Pros: Well-established, easy to reproduce
- Cons: Missing the transformer trend; reviewers will ask "what about ViT?"
- Rejected: Incomplete picture of the field

### Include 10+ baselines
- Pros: Comprehensive comparison
- Cons: Page limit constraints; diminishing returns after 5 baselines
- Rejected: 3-5 well-chosen baselines are more informative than 10 poorly analyzed ones

## Consequences
- Must obtain or reproduce ViT-B/16, ResNet-50, and ConvNeXt-T results
- Comparison table will have 3 baseline rows plus our method
- Reviewers may request additional baselines — have DeiT results ready as backup
```

### RDR Lifecycle

```
PROPOSED → ACCEPTED → (SUPERSEDED or DEPRECATED)
```

- **Don't delete old RDRs.** They capture the reasoning at a point in time.
- When a decision changes (e.g., new baseline needed), write a new RDR that
  references and supersedes the old one.

## Research Notes

### Documenting Literature Review Decisions

When reading papers, record not just what you found but why certain papers were
included or excluded:

```markdown
## Search Strategy Notes

### Included
- Vaswani et al. (2017) — foundational transformer architecture
  → Reason: Seminal work, required citation in any attention-based method paper

### Excluded
- Smith et al. (2024) — transformer pruning for edge devices
  → Reason: Out of scope — we focus on accuracy, not deployment efficiency

### Flagged for Discussion
- Chen et al. (2025) — concurrent work with similar approach
  → Reason: Need to discuss novelty differentiation; may need direct comparison
```

### Documenting Methodology Choices

```markdown
## Why We Use Cross-Entropy Loss (Not Focal Loss)

Our class distribution is balanced (ImageNet). Focal loss helps with class
imbalance (Lin et al., 2017) but adds a hyperparameter (γ) without benefit
when classes are balanced. We use standard cross-entropy for simplicity and
reproducibility.

If future experiments use imbalanced datasets, reconsider this choice.
See RDR-003.
```

## README for Research Projects

Every research project should have documentation covering:

```markdown
# [Paper Title]

One-paragraph description of the research question and approach.

## Quick Start
1. Clone the repo
2. Read the research brief: `docs/research/research-brief.md`
3. Review the paper outline: `docs/research/paper-outline.md`
4. See current drafts: `docs/research/draft/`

## Research Artifacts
| Artifact | Location | Status |
|----------|----------|--------|
| Research Brief | `docs/research/research-brief.md` | ✅ Complete |
| Literature Review | `docs/research/literature-review.md` | 🔄 In Progress |
| Paper Outline | `docs/research/paper-outline.md` | ✅ Complete |
| Draft Sections | `docs/research/draft/` | 🔄 In Progress |
| References | `docs/research/references.bib` | 🔄 In Progress |

## Key Decisions
- [RDR-001](docs/research/decisions/rdr-001.md): Selected baselines
- [RDR-002](docs/research/decisions/rdr-002.md): Chose NeurIPS as target venue

## Target Venue
[Venue name] — Deadline: [date]
```

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "The methodology is self-explanatory" | It shows what you did, not why you chose it over alternatives. |
| "We'll document decisions when we write the paper" | By then you've forgotten the alternatives you considered and why you rejected them. |
| "Nobody reads research notes" | Collaborators do. Reviewers ask "why not X?" and your RDR has the answer. |
| "Decision records are overhead" | A 10-minute RDR prevents a 2-hour debate about the same baseline selection later. |

## Red Flags

- Methodology choices with no documented rationale
- "We chose X" in the paper with no explanation of why not Y
- Collaborators repeatedly asking why a decision was made
- Baseline selection that can't be justified to reviewers
- No record of which papers were considered and excluded from the literature review

## Verification

After documenting:

- [ ] RDRs exist for all significant methodology decisions
- [ ] Literature review inclusion/exclusion criteria are documented
- [ ] Research brief is current and reflects the actual paper scope
- [ ] All decision records are saved to `docs/research/decisions/`
- [ ] README reflects the current state of all research artifacts
