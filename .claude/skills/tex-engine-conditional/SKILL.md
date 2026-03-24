---
name: tex-engine-conditional
description: Handles XeTeX vs pdfLaTeX compatibility by conditionally loading packages with iftex. Use when compile fails or when XeTeX-incompatible packages (e.g. soul) need to be loaded only for pdfLaTeX. Also use when fonts look wrong under Tectonic/XeTeX — conference style files (NeurIPS, CoRL, ICML, etc.) often set \rmdefault to ptm (Times) or \sfdefault to phv (Helvetica) via NFSS, which silently falls back to Latin Modern under XeTeX because TU-encoding font definition files (TUqtm.fd, TUphv.fd) don't exist. Trigger on: "font undefined" warnings mentioning TU/qtm or TU/phv, Times/Helvetica not rendering, or any font mismatch when building with Tectonic.
---

# TeX Engine Conditional (XeTeX / pdfLaTeX)

## When to Use

- Compilation fails because a package does not work with XeTeX (e.g. `soul`).
- **Fonts look wrong under Tectonic/XeTeX.** Conference style files (CoRL, NeurIPS, ICML, etc.) commonly set `\renewcommand{\rmdefault}{ptm}` (Times) and `\renewcommand{\sfdefault}{phv}` (Helvetica). These NFSS family codes require font definition files like `TUqtm.fd` that don't exist for XeTeX's TU encoding, so the engine silently falls back to Latin Modern. Symptoms include: `Font shape 'TU/qtm/m/n' undefined` warnings in the log, or the PDF visually using the wrong font.
- The document may be built with either XeTeX or pdfLaTeX and some packages or settings must differ by engine.

## How

1. Load the `iftex` package: `\usepackage{iftex}`.
2. Use `\ifXeTeX` … `\else` … `\fi` to branch.
3. Place the conditional block **after** `\documentclass` and the style package (`\usepackage{corl_2026}`, etc.) so that the style's font defaults are overridden.

## Font Fix for Tectonic / XeTeX

When a style file sets `\rmdefault` to `ptm` or `\sfdefault` to `phv`, add this **after** the style package:

```latex
\usepackage{iftex}
\ifXeTeX
  \usepackage{newtxtext,newtxmath}
\fi
```

`newtxtext` loads TeX Gyre Termes (Times clone) and TeX Gyre Heros (Helvetica clone) via fontspec internally, so no manual `\setmainfont` is needed. `newtxmath` provides matching math fonts.

**Why this works:** `newtxtext` detects XeTeX and uses fontspec to load the OpenType TeX Gyre fonts from the TeX distribution bundle (which Tectonic auto-downloads). This bypasses the missing NFSS `TUqtm.fd` / `TUphv.fd` problem entirely.

**Do not** try to use `tgtermes` / `tgheros` packages directly — they only provide T1/OT1 font definitions and also fall back to Latin Modern under TU encoding.

## General Package Compatibility

```latex
\usepackage{iftex}

\ifXeTeX
  % XeTeX-only packages and settings
\else
  \usepackage{soul}  % incompatible with XeTeX; use only for pdfLaTeX
\fi
```

Keep each branch minimal: only the packages or commands that must differ by engine.

## Notes

- `iftex` also provides `\ifLuaTeX`, `\ifpdfTeX` for other engines if needed.
- Prefer a single conditional block per logical choice so the preamble stays readable.
