# arXiv Submission

This document is the practical checklist for submitting `Deterministic Boundary Layers` to arXiv.

## Target

- Primary category: `cs.AI` or `cs.SE`, depending on how the submission is framed
- Submission type: TeX source upload, not PDF-only
- arXiv accepts a packaged `.tar` or `.zip` archive for TeX submissions

## Manuscript state

Before submission, confirm:

- `paper/main.tex` has the intended public title, author line, and date
- the manuscript builds cleanly from source
- the PDF title, abstract, and references match the intended public version
- the companion relationship to `execution-without-normativity` is stated consistently

## Upload contents

Prepare a clean source bundle from `paper/` only.

Include:

- `main.tex`
- `sections/*.tex`
- `refs.bib` or a generated `.bbl`
- any image files actually referenced by the paper

Do not include:

- `main.pdf`
- `*.aux`
- `*.log`
- `*.out`
- `*.fls`
- `*.fdb_latexmk`
- other local build artifacts

## Metadata

Set in the arXiv web form:

- title: `Deterministic Boundary Layers: Governing Non-Deterministic Execution`
- author: `Lukas Pfister`
- abstract: use the abstract from `paper/main.tex`
- category: choose the primary category deliberately based on positioning
- comments: optional, e.g. `Architectural theory paper; companion execution theory available on GitHub.`

## Final pre-submit check

1. Build the paper once more locally.
2. Verify that the upload bundle contains only source files.
3. Check that references resolve correctly.
4. Preview the arXiv-rendered PDF.
5. Submit only after the preview matches the intended public version.
