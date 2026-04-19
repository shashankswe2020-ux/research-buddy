---
name: planning-and-task-breakdown
description: Breaks research paper work into ordered tasks. Use when you have a research topic or outline and need to break the writing into manageable stages. Use when the paper feels too large to start, when you need to estimate scope, or when sections can be drafted in parallel.
---

# Planning and Task Breakdown

## Overview

Decompose a research paper into small, verifiable writing tasks with explicit
acceptance criteria. Good task breakdown is the difference between a paper that
gets finished and one that stalls. Every task should be small enough to draft,
review, and verify in a single focused session.

## When to Use

- You have a research question and need to plan the full paper workflow
- A paper feels too large or vague to start writing
- Work needs to be parallelized (e.g., literature review while drafting methodology)
- You need to communicate scope and timeline to collaborators
- The writing order isn't obvious

**When NOT to use:** Quick edits to a single section, formatting fixes, or
citation corrections where the scope is already clear.

## The Planning Process

### Step 1: Assess Current State

Before planning, determine what already exists:

- Read the research brief (`docs/research/research-brief.md`) — is the research question defined?
- Check for a literature review (`docs/research/literature-review.md`)
- Check for a paper outline (`docs/research/paper-outline.md`)
- Check for any existing drafts in `docs/research/draft/`
- Check for references in `docs/research/references.bib`

**Do NOT start drafting during planning.** The output is a plan, not prose.

### Step 2: Identify the Dependency Graph

Map what depends on what in paper writing:

```
Research Question & Scope
    │
    ├── Literature Review
    │       │
    │       ├── Related Work Section
    │       │
    │       └── Gap Identification
    │               │
    │               └── Contribution Framing
    │
    ├── Methodology Design
    │       │
    │       └── Methodology Section
    │
    ├── Experiments / Data Collection
    │       │
    │       ├── Results Section
    │       │       │
    │       │       └── Discussion Section
    │       │
    │       └── Figures & Tables
    │
    └── Introduction (written last — needs full picture)
            │
            └── Abstract (written last — summarizes everything)
```

### Step 3: Slice by Section, Not by Activity

Instead of "do all research, then all writing, then all revision" — complete one
section at a time so progress is visible and reviewable.

**Bad (horizontal slicing):**
```
Task 1: Read all papers
Task 2: Write all sections
Task 3: Add all citations
Task 4: Revise everything
```

**Good (vertical slicing):**
```
Task 1: Draft methodology (with citations)
Task 2: Draft related work (with citations)
Task 3: Draft experiments and results
Task 4: Draft discussion
Task 5: Draft introduction (needs full picture)
Task 6: Write abstract
```

Each vertical slice produces a reviewable, cited section.

### Step 4: Write Tasks

Each task follows this structure:

```markdown
## Task [N]: [Short descriptive title]

**Description:** One paragraph explaining what this task produces.

**Acceptance criteria:**
- [ ] [Specific, verifiable condition]
- [ ] [Specific, verifiable condition]

**Verification:**
- [ ] Section saved to `docs/research/draft/[section].md`
- [ ] All claims have citations
- [ ] Section follows paper outline structure
- [ ] Reviewed for clarity and academic tone

**Dependencies:** [Task numbers this depends on, or "None"]

**Estimated scope:** [Short | Medium | Long]
```

### Step 5: Order and Checkpoint

Arrange tasks so that:

1. Foundation tasks come first (research question, literature review)
2. Each task produces a standalone, reviewable artifact
3. Verification checkpoints occur after every 2-3 tasks
4. Risky or uncertain sections are tackled early

Add explicit checkpoints:

```markdown
## Checkpoint: After Tasks 1-3
- [ ] Research brief is complete and approved
- [ ] Literature review covers the field adequately
- [ ] Paper outline is finalized
- [ ] Review with collaborators before proceeding to drafting
```

## Task Sizing Guidelines

| Size | Scope | Example |
|------|-------|---------|
| **S** | Single subsection or focused edit | Write one subsection of related work |
| **M** | One full section | Draft the complete methodology section |
| **L** | Multi-section or cross-cutting | Complete literature review with synthesis |
| **XL** | **Too large — break it down** | "Write the paper" |

If a task is L or larger, consider breaking it into smaller tasks.

## Plan Document Template

```markdown
# Writing Plan: [Paper Title / Topic]

## Overview
[One paragraph: research question, target venue, expected contributions]

## Current State
- [x] Research question defined
- [ ] Literature review complete
- [ ] Outline finalized
- [ ] Sections drafted: [list]

## Task List

### Phase 1: Foundation
- [ ] Task 1: Define research question and scope
- [ ] Task 2: Conduct literature review
- [ ] Task 3: Create paper outline

### Checkpoint: Foundation
- [ ] Research brief approved
- [ ] Outline reviewed by collaborators

### Phase 2: Drafting
- [ ] Task 4: Draft methodology
- [ ] Task 5: Draft related work
- [ ] Task 6: Draft experiments and results
- [ ] Task 7: Draft discussion

### Checkpoint: Draft Complete
- [ ] All sections have first drafts
- [ ] All claims cited

### Phase 3: Assembly & Polish
- [ ] Task 8: Draft introduction
- [ ] Task 9: Write abstract
- [ ] Task 10: Revision passes (structure, writing, technical accuracy)
- [ ] Task 11: Simulate peer review
- [ ] Task 12: Final assembly and formatting

### Checkpoint: Submission Ready
- [ ] All review feedback addressed
- [ ] Pre-submission checklist passed

## Risks and Mitigations
| Risk | Impact | Mitigation |
|------|--------|------------|
| Key related work missing | High | Search multiple databases, ask domain experts |
| Experiments inconclusive | High | Plan ablation studies, define success criteria upfront |
| Venue deadline tight | Medium | Prioritize core sections, defer supplementary material |

## Open Questions
- [Questions needing collaborator input]
```

## Parallelization Opportunities

When multiple sessions or collaborators are available:

- **Safe to parallelize:** Literature review and methodology design; figures and text drafting
- **Must be sequential:** Introduction depends on all other sections; abstract depends on everything
- **Needs coordination:** Related work and methodology (shared framing of contributions)

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I'll just start writing and see where it goes" | Unplanned papers meander. An outline saves rewriting entire sections. |
| "The tasks are obvious" | Write them down anyway. Explicit tasks surface forgotten dependencies. |
| "Planning is overhead" | A 15-minute plan prevents days of unstructured work. |
| "I'll write the intro first" | The introduction requires the full picture. Draft it last. |

## Red Flags

- Starting to draft without a research question or outline
- Tasks that say "write the paper" without section-level breakdown
- No verification steps in the plan
- All tasks are XL-sized
- No checkpoints between phases
- Introduction scheduled as the first writing task

## Verification

Before starting to write, confirm:

- [ ] Every task has acceptance criteria
- [ ] Every task has a verification step
- [ ] Task dependencies are identified and ordered correctly
- [ ] Checkpoints exist between major phases
- [ ] Collaborators have reviewed and approved the plan
