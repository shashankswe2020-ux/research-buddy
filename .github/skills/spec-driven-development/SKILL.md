---
name: spec-driven-development
description: >
  Defines and tracks the complete end-to-end research paper lifecycle — from
  idea inception to published paper. Enforces readiness gates at every phase
  transition so the agent always reports current status and what's needed to
  advance. Use when starting a new paper, assessing progress, or deciding
  what to work on next.
---

# Spec-Driven Research Paper Lifecycle

## Overview

This skill defines the **complete end-to-end lifecycle** of a research paper —
from the first spark of an idea to a published, citable artifact. Every phase
has explicit **readiness criteria** that must be met before advancing. The agent
must always report the current phase, readiness status, and what's blocking
progress.

Writing without knowing where you are in the lifecycle is guessing. This skill
ensures you always know.

---

## The Complete Research Paper Lifecycle

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    RESEARCH PAPER LIFECYCLE                                  │
│                                                                             │
│  💡 IDEATION ──→ 📚 RESEARCH ──→ 📐 PLANNING ──→ ✍️ WRITING               │
│      │               │               │               │                      │
│      ▼               ▼               ▼               ▼                      │
│  ┌────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐                │
│  │Phase 1 │    │Phase 2   │    │Phase 3   │    │Phase 4   │                │
│  │Idea &  │───▶│Literature│───▶│Outline & │───▶│Section   │                │
│  │Scope   │    │Review    │    │Structure │    │Drafting  │                │
│  └────────┘    └──────────┘    └──────────┘    └──────────┘                │
│      🚦             🚦              🚦              🚦                      │
│                                                                             │
│  ──→ 📎 CITATIONS ──→ 🔄 REVISION ──→ 🔍 REVIEW ──→ 📦 SHIP ──→ 📢 POST │
│          │                │               │             │            │       │
│          ▼                ▼               ▼             ▼            ▼       │
│    ┌──────────┐    ┌──────────┐    ┌──────────┐  ┌─────────┐ ┌─────────┐  │
│    │Phase 5   │    │Phase 6   │    │Phase 7   │  │Phase 8  │ │Phase 9  │  │
│    │Citation  │───▶│Revision &│───▶│Peer Rev. │─▶│Final    │▶│Post-Sub │  │
│    │Mgmt     │    │Polishing │    │Simulation│  │Assembly │ │& Publish│  │
│    └──────────┘    └──────────┘    └──────────┘  └─────────┘ └─────────┘  │
│         🚦              🚦              🚦            🚦          🚦        │
│                                                                             │
│  🚦 = Readiness Gate — must pass before advancing to next phase            │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Phase Readiness Dashboard

**The agent MUST report this dashboard at the start of every interaction and
after completing any significant task.**

```
╔══════════════════════════════════════════════════════════════════════╗
║                    📊 RESEARCH PAPER STATUS                         ║
╠══════════════════════════════════════════════════════════════════════╣
║                                                                      ║
║  Phase 1: Idea & Scope           [ ✅ COMPLETE | 🔄 IN PROGRESS | ⬜ NOT STARTED ]  ║
║  Phase 2: Literature Review      [ ✅ | 🔄 | ⬜ ]                   ║
║  Phase 3: Outline & Structure    [ ✅ | 🔄 | ⬜ ]                   ║
║  Phase 4: Section Drafting       [ ✅ | 🔄 | ⬜ ]                   ║
║  Phase 5: Citation Management    [ ✅ | 🔄 | ⬜ ]                   ║
║  Phase 6: Revision & Polishing   [ ✅ | 🔄 | ⬜ ]                   ║
║  Phase 7: Peer Review Simulation [ ✅ | 🔄 | ⬜ ]                   ║
║  Phase 8: Final Assembly         [ ✅ | 🔄 | ⬜ ]                   ║
║  Phase 9: Post-Submission        [ ✅ | 🔄 | ⬜ ]                   ║
║                                                                      ║
║  Current Phase: [N]                                                  ║
║  Blocking Items: [list what's needed to advance]                    ║
║  Next Milestone: [what happens when current phase completes]        ║
║                                                                      ║
╚══════════════════════════════════════════════════════════════════════╝
```

---

## Phase Definitions & Readiness Gates

### Phase 1: Idea & Scope Definition

**Goal:** Define a clear, novel, and feasible research question with explicit scope.

**Activities:**
- Identify research area and initial ideas
- Conduct gap analysis against existing work
- Formulate SMART research question
- Define expected contributions (2–4 concrete items)
- Set scope boundaries (in-scope / out-of-scope)
- Identify target venue and its requirements
- Assess feasibility (data, compute, timeline)

**Output:** `docs/research/research-brief.md`

**🚦 Readiness Gate → Phase 2:**

| # | Criterion | Required |
|---|-----------|----------|
| 1 | Research question is specific and answerable | ✅ |
| 2 | Expected contributions are explicitly listed (2–4) | ✅ |
| 3 | Scope boundaries defined (in-scope / out-of-scope) | ✅ |
| 4 | Target venue identified with deadline and format requirements | ✅ |
| 5 | Feasibility assessed (data availability, tools, timeline) | ✅ |
| 6 | Success criteria are measurable | ✅ |
| 7 | Research brief saved to `docs/research/research-brief.md` | ✅ |
| 8 | Collaborator / user has reviewed and approved | ✅ |

**Readiness command:** `"check phase 1 readiness"`

---

### Phase 2: Literature Review

**Goal:** Build a comprehensive, synthesized review of related work.

**Activities:**
- Define systematic search strategy (queries, databases, inclusion/exclusion)
- Search and collect relevant papers (Google Scholar, arXiv, ACM, IEEE, Springer)
- Analyze each paper (summary, methodology, strengths, limitations, relevance)
- Synthesize and organize by theme / methodology / chronology
- Build comparison table of key approaches
- Identify gaps, contradictions, and open problems

**Output:** `docs/research/literature-review.md`

**🚦 Readiness Gate → Phase 3:**

| # | Criterion | Required |
|---|-----------|----------|
| 1 | ≥15 relevant papers analyzed with structured summaries | ✅ |
| 2 | Papers organized thematically (not just listed) | ✅ |
| 3 | Comparison table of ≥5 key approaches | ✅ |
| 4 | Research gaps clearly identified and linked to your RQ | ✅ |
| 5 | All references have complete bibliographic info (author, title, year, venue, DOI) | ✅ |
| 6 | No fabricated references — all papers verified via search | ✅ |
| 7 | Literature review saved to `docs/research/literature-review.md` | ✅ |
| 8 | Initial references saved to `docs/research/references.bib` | ✅ |

---

### Phase 3: Outline & Structure

**Goal:** Create a detailed, logical skeleton for the entire paper.

**Activities:**
- Select paper type (empirical, survey, position, systems, short/workshop)
- Propose 3–5 candidate titles
- Outline every section at the paragraph level
- Map the argument flow (problem → gap → method → evidence → conclusion)
- Verify the "red thread" — narrative coherence from intro to conclusion
- Identify where evidence is needed (citations, experiments, figures)

**Output:** `docs/research/paper-outline.md`

**🚦 Readiness Gate → Phase 4:**

| # | Criterion | Required |
|---|-----------|----------|
| 1 | Paper type selected with justification | ✅ |
| 2 | Every section outlined at paragraph level | ✅ |
| 3 | Argument flow mapped — each section builds on the previous | ✅ |
| 4 | Every claim in the conclusion traceable to body content | ✅ |
| 5 | Evidence needs identified (which claims need citations/data) | ✅ |
| 6 | Outline saved to `docs/research/paper-outline.md` | ✅ |

---

### Phase 4: Section Drafting

**Goal:** Draft every section with proper academic tone, evidence, and flow.

**Activities:**
- Draft each section following the outline and style guide
- Apply paragraph structure: topic sentence → evidence → analysis → transition
- Ensure every claim has a citation or is flagged for one
- Use consistent terminology and define acronyms at first use
- Save each section as a separate file for modular review

**Output:** `docs/research/draft/` — individual section files

**🚦 Readiness Gate → Phase 5:**

| # | Criterion | Required |
|---|-----------|----------|
| 1 | All sections drafted: abstract, introduction, related work, methodology, experiments, discussion, conclusion | ✅ |
| 2 | Each section saved as `docs/research/draft/<section>.md` | ✅ |
| 3 | Every factual claim has a citation or is flagged `[CITATION NEEDED]` | ✅ |
| 4 | Consistent terminology used throughout all sections | ✅ |
| 5 | All contributions from the research brief are addressed in the body | ✅ |
| 6 | Figures and tables are described (even if placeholders) | ✅ |

---

### Phase 5: Citation Management

**Goal:** Ensure all citations are accurate, complete, and properly formatted.

**Activities:**
- Maintain master BibTeX file with complete entries
- Verify every cited paper exists (search for DOI/title)
- Ensure citation keys follow naming convention
- Apply correct citation style (APA, IEEE, ACM per venue)
- Flag and resolve all `[CITATION NEEDED]` markers
- Remove orphan references (cited but not in text, or in text but not in bib)

**Output:** `docs/research/references.bib` + `docs/research/references.md`

**🚦 Readiness Gate → Phase 6:**

| # | Criterion | Required |
|---|-----------|----------|
| 1 | All `[CITATION NEEDED]` markers resolved | ✅ |
| 2 | Every BibTeX entry has: author, title, year, venue, DOI/URL | ✅ |
| 3 | All cited papers verified as real (no fabricated references) | ✅ |
| 4 | No orphan references (every bib entry cited, every citation in bib) | ✅ |
| 5 | Citation style matches target venue requirements | ✅ |
| 6 | References saved to both `.bib` and `.md` formats | ✅ |

---

### Phase 6: Revision & Polishing

**Goal:** Elevate the draft from "complete" to "submission-quality."

**Activities:**
- **Pass 1 — Structure & Argumentation:** Red thread, contribution coverage, section balance
- **Pass 2 — Writing Quality:** Filler words, passive voice, sentence length, tense consistency
- **Pass 3 — Technical Accuracy:** Equations, figure/table references, statistics, cited results

**Output:** `docs/research/revision-notes.md`

**🚦 Readiness Gate → Phase 7:**

| # | Criterion | Required |
|---|-----------|----------|
| 1 | All three revision passes completed with documented findings | ✅ |
| 2 | All critical revision issues resolved | ✅ |
| 3 | No filler words or unjustified hedge words remain | ✅ |
| 4 | All figure/table numbers match their text references | ✅ |
| 5 | Consistent tense throughout (present for facts, past for experiments) | ✅ |
| 6 | All acronyms defined at first use | ✅ |
| 7 | Revision notes saved to `docs/research/revision-notes.md` | ✅ |

---

### Phase 7: Peer Review Simulation

**Goal:** Anticipate reviewer feedback and strengthen the paper proactively.

**Activities:**
- Simulate three reviewer perspectives (Domain Expert, Methodologist, Skeptic)
- Generate structured reviews with scores, strengths, weaknesses, questions
- Create a response plan addressing every concern
- Iterate on the draft based on simulated feedback

**Output:** `docs/research/reviews/` — individual review files + response plan

**🚦 Readiness Gate → Phase 8:**

| # | Criterion | Required |
|---|-----------|----------|
| 1 | Three simulated reviews generated with structured feedback | ✅ |
| 2 | Average simulated score is Weak Accept or better | ✅ |
| 3 | All Critical weaknesses have been addressed in the draft | ✅ |
| 4 | Response plan created for every reviewer concern | ✅ |
| 5 | Reviews saved to `docs/research/reviews/` | ✅ |
| 6 | Response plan saved to `docs/research/reviews/review-response-plan.md` | ✅ |

---

### Phase 8: Final Assembly & Formatting

**Goal:** Produce a submission-ready manuscript formatted for the target venue.

**Activities:**
- Merge all sections into a single document
- Apply venue formatting (LaTeX template or clean Markdown)
- Run pre-submission checklist (page limit, anonymization, completeness)
- Package supplementary materials
- Generate final PDF (if LaTeX)

**Output:** `docs/research/final/` — complete submission package

**🚦 Readiness Gate → Phase 9:**

| # | Criterion | Required |
|---|-----------|----------|
| 1 | Full paper assembled in `docs/research/final/paper.md` | ✅ |
| 2 | LaTeX version generated (if venue requires it) | ✅ |
| 3 | Paper within page limit | ✅ |
| 4 | All figures/tables referenced in text | ✅ |
| 5 | All references complete (no missing BibTeX fields) | ✅ |
| 6 | Abstract within word limit | ✅ |
| 7 | Anonymized for double-blind review (if required) | ✅ |
| 8 | No TODO markers or placeholder text remain | ✅ |
| 9 | Supplementary materials organized | ✅ |
| 10 | Pre-submission checklist passed with all items ✅ | ✅ |

---

### Phase 9: Post-Submission & Publication

**Goal:** Handle everything after the paper is submitted — through to publication.

**Activities:**

#### 9a. Submission
- Submit to target venue via their submission system
- Record submission metadata (venue, deadline, submission ID, track)
- Archive the exact submitted version (tag in git or copy to `docs/research/submissions/`)

#### 9b. Rebuttal & Revision (if reviews received)
- Analyze actual reviewer feedback
- Write a point-by-point rebuttal / response letter
- Revise the paper to address reviewer concerns
- Document every change made in response to reviews
- Save response letter to `docs/research/rebuttal/`

#### 9c. Camera-Ready Preparation (if accepted)
- Apply camera-ready formatting changes
- Update author information (de-anonymize if double-blind)
- Finalize acknowledgments and funding statements
- Submit camera-ready version by deadline
- Register for the conference (if applicable)

#### 9d. Publication & Dissemination
- Upload preprint to arXiv (if appropriate)
- Share on academic social media (Twitter/X, LinkedIn, ResearchGate)
- Prepare a 1-page summary / blog post for broader audience
- Create presentation slides for conference talk or poster
- Update personal/lab website with the new publication
- Deposit in institutional repository

**Output:**
- `docs/research/submissions/` — submission records and archives
- `docs/research/rebuttal/` — reviewer responses and revision logs
- `docs/research/camera-ready/` — final published version
- `docs/research/dissemination/` — slides, blog post, social media drafts

**🚦 Readiness Gate → COMPLETE:**

| # | Criterion | Required |
|---|-----------|----------|
| 1 | Paper submitted to target venue | ✅ |
| 2 | Submission archived with metadata | ✅ |
| 3 | Rebuttal written and submitted (if applicable) | ✅ |
| 4 | Camera-ready version prepared and submitted (if accepted) | ✅ |
| 5 | Preprint uploaded to arXiv (if appropriate) | ✅ |
| 6 | Presentation materials created (slides or poster) | ✅ |
| 7 | Dissemination plan executed (social media, blog, website) | ✅ |

---

## Readiness Assessment Protocol

When asked to check readiness or report status, the agent MUST:

1. **Scan the `docs/research/` directory** for existing artifacts
2. **Evaluate each phase's readiness criteria** against what exists
3. **Report the dashboard** showing all phases with status indicators
4. **Identify blocking items** — specific criteria not yet met
5. **Recommend next actions** — what to do to advance

### Status Indicators

| Icon | Meaning |
|------|---------|
| ✅ | Phase complete — all readiness criteria met |
| 🔄 | In progress — some criteria met, work ongoing |
| ⬜ | Not started — no artifacts for this phase |
| 🚫 | Blocked — cannot start until a prior phase completes |
| ⚠️ | Needs attention — issues found that must be resolved |

### Example Readiness Report

```
📊 RESEARCH PAPER STATUS — "Attention-Based Multi-Scale Feature Fusion"
═══════════════════════════════════════════════════════════════════════

  Phase 1: Idea & Scope           ✅  Complete (research-brief.md exists, all criteria met)
  Phase 2: Literature Review      ✅  Complete (22 papers analyzed, comparison table present)
  Phase 3: Outline & Structure    ✅  Complete (IMRaD structure, argument flow mapped)
  Phase 4: Section Drafting       🔄  In Progress (5/7 sections drafted)
      → Missing: discussion.md, conclusion.md
  Phase 5: Citation Management    ⚠️  Needs Attention (3 [CITATION NEEDED] markers remain)
  Phase 6: Revision & Polishing   🚫  Blocked by Phase 4
  Phase 7: Peer Review Simulation 🚫  Blocked by Phase 6
  Phase 8: Final Assembly         🚫  Blocked by Phase 7
  Phase 9: Post-Submission        🚫  Blocked by Phase 8

  Current Phase: 4 (Section Drafting)
  Blocking Items:
    - Draft discussion.md and conclusion.md
    - Resolve 3 [CITATION NEEDED] markers
  Next Milestone: Complete all drafts → advance to Phase 5 (Citation Management)
```

---

## Lifecycle State Machine

Every phase transition is guarded. The agent must verify readiness before
advancing.

```
                    ┌─────────────────────────────────────┐
                    │         STATE TRANSITIONS            │
                    └─────────────────────────────────────┘

  ⬜ NOT STARTED ──[begin work]──→ 🔄 IN PROGRESS
                                        │
                                        ├──[readiness gate PASSED]──→ ✅ COMPLETE
                                        │                                   │
                                        │                          [next phase unlocked]
                                        │
                                        └──[readiness gate FAILED]──→ ⚠️ NEEDS ATTENTION
                                                                           │
                                                                    [fix issues, re-check]
                                                                           │
                                                                           ▼
                                                                    🔄 IN PROGRESS
```

**Rules for phase transitions:**
1. Cannot skip phases (Phase 1 → Phase 3 is not allowed)
2. Can revisit completed phases (new insights may update the research brief)
3. Readiness gate must be explicitly checked before advancing
4. Blocking items must be listed when a gate fails
5. The dashboard must be updated after every significant change

---

## Quick Status Commands

| Command | What it does |
|---------|-------------|
| `"status"` or `"where am I?"` | Show full readiness dashboard |
| `"check phase N readiness"` | Evaluate readiness gate for phase N |
| `"what's blocking me?"` | List all blocking items across all phases |
| `"what should I work on next?"` | Recommend the highest-priority next action |
| `"advance to phase N"` | Check readiness and advance if gate passes |

---

## Output Directory Structure (Complete Lifecycle)

```
docs/research/
├── research-brief.md              # Phase 1: Research question & scope
├── literature-review.md           # Phase 2: Organized literature review
├── paper-outline.md               # Phase 3: Detailed section outline
├── draft/                         # Phase 4: Individual section drafts
│   ├── abstract.md
│   ├── introduction.md
│   ├── related-work.md
│   ├── methodology.md
│   ├── experiments.md
│   ├── discussion.md
│   └── conclusion.md
├── references.bib                 # Phase 5: BibTeX references
├── references.md                  # Phase 5: Human-readable reference list
├── revision-notes.md              # Phase 6: Revision checklist & notes
├── reviews/                       # Phase 7: Simulated peer reviews
│   ├── reviewer-1-domain-expert.md
│   ├── reviewer-2-methodologist.md
│   ├── reviewer-3-skeptic.md
│   └── review-response-plan.md
├── final/                         # Phase 8: Submission-ready output
│   ├── paper.md
│   ├── paper.tex
│   ├── references.bib
│   ├── figures/
│   └── supplementary/
├── submissions/                   # Phase 9a: Submission records
│   └── submission-log.md
├── rebuttal/                      # Phase 9b: Reviewer responses
│   ├── reviewer-responses.md
│   └── revision-changelog.md
├── camera-ready/                  # Phase 9c: Published version
│   ├── paper-final.pdf
│   └── paper-final.tex
├── dissemination/                 # Phase 9d: Outreach materials
│   ├── slides.md
│   ├── blog-post.md
│   ├── social-media-drafts.md
│   └── poster.md
└── decisions/                     # Research Decision Records
    ├── rdr-001.md
    └── rdr-002.md
```

---

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I'll figure out the contribution as I write" | Unfocused papers get rejected. Define contributions first (Phase 1). |
| "The research question is obvious" | Write it down. Explicit questions prevent scope creep. |
| "I'll skip the literature review for now" | Reviewers will ask "how is this different from X?" Be ready. |
| "I can write the intro first" | The introduction needs the full picture. Draft it after other sections. |
| "Readiness gates slow me down" | They catch problems early. A failed gate at Phase 3 is cheaper than a desk-reject. |
| "I'll add citations later" | Unsupported claims become habits. Cite as you write or flag `[CITATION NEEDED]`. |
| "The paper is done when the draft is done" | The draft is Phase 4 of 9. Revision, review, and formatting are half the work. |

---

## Red Flags

- Starting Phase 4 (drafting) without completing Phase 1 (scope)
- No literature review before writing related work
- `[CITATION NEEDED]` markers surviving into Phase 8
- Skipping the peer review simulation — it catches 80% of actual reviewer concerns
- Submitting without running the pre-submission checklist
- No post-submission plan (rebuttal prep, dissemination)

---

## Verification

At any point, the agent can verify overall project health:

- [ ] Current phase is accurately identified
- [ ] All completed phases have their artifacts saved
- [ ] No readiness gate was skipped
- [ ] Blocking items are documented and actionable
- [ ] The readiness dashboard matches the actual state of `docs/research/`
