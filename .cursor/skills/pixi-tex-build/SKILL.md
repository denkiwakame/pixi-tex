---
name: pixi-tex-build
description: Builds PDF from TeX using the project's pixi task. Use when building the PDF, when the user asks to build or typeset, or when running a LaTeX build in this project.
---

# pixi-tex PDF Build

## Instructions

When building the PDF in this project:

1. Run `pixi run build` for the default document (`draft.tex`).
2. To build a specific `.tex` file, run `pixi run build <file>.tex` (e.g. `pixi run build other.tex`).

Do not invoke `tectonic` or `pixi run tectonic` directly; use the `build` task so that options (e.g. `--outdir build`, `--keep-logs`, `--keep-intermediates`) stay consistent.

Output is written to the `build/` directory.
