---
name: shipping-and-launch
description: Handles final assembly, formatting, and delivery of research papers. Use when compiling a manuscript for submission, formatting for a target venue, running pre-submission checklists, or packaging supplementary materials.
---

# Shipping and Launch

## Overview

Shipping a research paper is more than clicking "submit." It requires careful assembly, formatting compliance, completeness verification, and packaging. This skill ensures nothing is forgotten between "the paper is written" and "the paper is submitted."

## When to Use

- Assembling a final manuscript from individual section drafts
- Formatting a paper for a specific conference or journal
- Running a pre-submission quality checklist
- Generating LaTeX from Markdown drafts
- Packaging supplementary materials (appendices, code, data)
- Preparing camera-ready versions after acceptance

**When NOT to use:** Don't use this for drafting content, literature review, or revision — those have their own stages.

## Pre-submission Checklist

Before any submission, verify every item:

### Content Completeness
- [ ] All sections present: abstract, introduction, related work, methodology, experiments, discussion, conclusion
- [ ] All figures and tables referenced in text and vice versa
- [ ] All claims supported by citations or experimental evidence
- [ ] No TODO markers, placeholder text, or incomplete sentences
- [ ] Acknowledgments section included (if not double-blind)

### Formatting Compliance
- [ ] Paper meets page limit for the target venue
- [ ] Abstract within word limit (typically 150–300 words)
- [ ] Correct document class and style file used
- [ ] Margins, font size, and column layout match venue requirements
- [ ] Section numbering follows venue convention
- [ ] Figure and table captions are descriptive and consistently formatted

### References
- [ ] All BibTeX entries are complete (author, title, year, venue, DOI)
- [ ] No duplicate references
- [ ] No unreferenced entries in the bibliography
- [ ] Citation style matches venue (APA, IEEE numbered, ACM, etc.)
- [ ] All cited papers are real and verifiable

### Anonymization (Double-Blind Venues)
- [ ] Author names and affiliations removed
- [ ] Self-citations neutralized ("We previously showed…" → "Prior work showed…")
- [ ] No identifying information in headers, footers, or metadata
- [ ] Supplementary material does not reveal authorship
- [ ] Repository links anonymized or removed

### Final Packaging
- [ ] PDF compiles without errors or warnings
- [ ] All fonts are embedded in the PDF
- [ ] Supplementary materials organized in a separate archive if required
- [ ] Filename follows venue convention
- [ ] Submission system metadata matches the paper (title, abstract, authors)

## Venue Formatting Reference

| Venue          | Class / Template   | Columns | Typical Page Limit | Citation Style |
| -------------- | ------------------ | ------- | ------------------ | -------------- |
| ACM (general)  | `acmart`           | 2       | 10–12 pages        | ACM Reference  |
| IEEE (general) | `IEEEtran`         | 2       | 8–10 pages         | IEEE numbered  |
| NeurIPS        | `neurips_20XX.sty` | 1       | 9 pages + refs     | Author-year    |
| ICML           | `icml20XX.sty`     | 2       | 8 pages + refs     | Author-year    |
| Springer LNCS  | `llncs`            | 1       | 12–15 pages        | Numbered       |
| arXiv          | `article`          | 1       | No limit           | Flexible       |

## Output Convention

All shipping artifacts go to:

```
docs/research/final/
├── paper.md
├── paper.tex          (if LaTeX requested)
├── references.bib
├── figures/
└── supplementary/
```

## Tips

- **Compile early, compile often** — don't wait until the deadline to generate the PDF.
- **Use a consistent figure naming scheme** — `fig-01-architecture.pdf`, `fig-02-results.pdf`.
- **Keep a submission log** — record venue, deadline, submission ID, and any reviewer feedback.
- **Archive each submission** — tag or copy the exact version that was submitted.
