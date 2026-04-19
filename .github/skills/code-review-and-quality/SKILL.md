---
name: code-review-and-quality
description: Conducts multi-dimensional manuscript review. Use when evaluating draft quality across argumentation, clarity, evidence, structure, and academic style before submission.
---

# Manuscript Review and Quality

## Overview

Multi-dimensional manuscript review with quality gates. Every draft section gets
reviewed before inclusion in the final paper. Review covers five axes:
argumentation, clarity, evidence, structure, and academic style.

**The approval standard:** A section is ready when it clearly advances the
paper's argument, is supported by evidence, and meets venue standards — even if
it could still be polished further. Perfect prose doesn't exist; the goal is
continuous improvement toward submission readiness.

## When to Use

- After drafting any section of the paper
- Before assembling the final manuscript
- When revising based on peer review feedback
- After updating the literature review with new sources
- When verifying citation accuracy and completeness

**When NOT to use:** During initial brainstorming or topic exploration where
rough notes are expected.

## The Five-Axis Review

### 1. Argumentation & Logic

Does the paper present a sound, well-reasoned argument?

- Is the research question clearly stated and motivated?
- Does the gap analysis justify why this work is needed?
- Are all claims logically connected to evidence?
- Are contributions in the introduction substantiated in the body?
- Are limitations and threats to validity honestly discussed?
- Does the conclusion follow from the results (no overclaiming)?

### 2. Clarity & Readability

Can another researcher understand this without the author explaining it?

- Are key terms defined on first use and consistent throughout?
- Are sentences concise (target ≤25 words average)?
- Is jargon minimized or clearly explained?
- Are acronyms defined at first use?
- Is signposting effective ("Building on this…", "In contrast…")?
- Do figures and tables have descriptive, self-contained captions?

### 3. Evidence & Citations

Is every claim backed by verifiable evidence?

- Does every factual assertion have a citation or reference to original data?
- Are all cited papers real and verifiable (no fabricated references)?
- Do cited results accurately reflect the source papers?
- Are there unsupported claims ("It is well-known…" without citation)?
- Are baselines and comparisons fair and well-justified?
- Is the literature review comprehensive for the field?

### 4. Structure & Flow

Does the paper follow a logical progression?

- Does it use the appropriate structure for its type (IMRaD, survey, position)?
- Do section lengths match their importance to the argument?
- Are transitions between sections smooth?
- Does each paragraph follow: topic sentence → evidence → analysis → transition?
- Are all figures, tables, and equations referenced in the text?
- Does the abstract faithfully summarize the paper?

### 5. Academic Style & Conventions

Does the writing meet academic publishing standards?

- Is the tone formal but accessible?
- Is active voice used where appropriate ("We propose…")?
- Are filler words removed ("very", "really", "basically", "quite")?
- Is tense consistent (present for established facts, past for experiments)?
- Does formatting match the target venue's requirements?
- Are citation formats correct (APA, IEEE, ACM as appropriate)?

## Finding Categories

| Severity | Meaning | Action Required |
|----------|---------|-----------------|
| **Critical** | Blocks submission | Logical fallacy, fabricated citation, missing key section |
| **Important** | Should fix | Weak argument, missing related work, unclear methodology |
| **Suggestion** | Nice to have | Better word choice, additional analysis, formatting polish |

## Section-Specific Checks

### Abstract
- [ ] States problem, approach, key results, and significance
- [ ] Within word limit (typically 150–300 words)
- [ ] No citations (unless venue allows)
- [ ] Accurately represents the paper's content

### Introduction
- [ ] Opens with compelling motivation
- [ ] Clearly states the problem and research gap
- [ ] Enumerates contributions explicitly
- [ ] Includes paper organization paragraph

### Related Work
- [ ] Organized thematically (not just chronologically)
- [ ] Distinguishes this work from closest related work
- [ ] Includes comparison table for ≥5 approaches
- [ ] No missing seminal or recent key papers

### Methodology
- [ ] Starts with high-level overview before details
- [ ] Justifies every design choice
- [ ] Defines all notation before use
- [ ] Reproducible from description alone

### Experiments & Results
- [ ] Datasets, metrics, and baselines clearly described
- [ ] Results tables use consistent formatting (bold best)
- [ ] Analysis explains what results mean, not just reports numbers
- [ ] Statistical significance reported where applicable

### Discussion & Conclusion
- [ ] Interprets results in context of research questions
- [ ] Addresses unexpected findings honestly
- [ ] States limitations and threats to validity
- [ ] Conclusion mirrors introduction contributions
- [ ] Proposes specific future work directions

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "The reviewer will understand what I mean" | If it's ambiguous to you, it's ambiguous to them. Be explicit. |
| "I'll add citations later" | Unsupported claims become habits. Cite as you write. |
| "The results speak for themselves" | Results need interpretation. Explain what they mean and why they matter. |
| "This limitation is obvious, no need to state it" | Reviewers respect transparency. Unstated limitations look like blind spots. |
| "It's just a first draft" | First drafts set the structure. Review early to avoid compounding problems. |

## Red Flags

- Claims without citations
- Contributions listed in the intro but not demonstrated in the body
- Results reported without analysis or interpretation
- Missing or fabricated references
- Inconsistent terminology across sections
- Abstract that doesn't match the paper's actual content
- "We solve the problem" without precise scope qualification

## Verification

After review is complete:

- [ ] All Critical issues are resolved
- [ ] All Important issues are resolved or explicitly deferred with justification
- [ ] Every claim has a supporting citation or data reference
- [ ] All sections follow the target venue's formatting
- [ ] Review document is saved to `docs/research/reviews/`
