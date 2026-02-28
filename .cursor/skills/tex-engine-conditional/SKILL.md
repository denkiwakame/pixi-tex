---
name: tex-engine-conditional
description: Handles XeTeX vs pdfLaTeX compatibility by conditionally loading packages with iftex. Use when compile fails or when XeTeX-incompatible packages (e.g. soul) need to be loaded only for pdfLaTeX.
---

# TeX Engine Conditional (XeTeX / pdfLaTeX)

## When to Use

- Compilation fails and the cause is a package that does not work well with XeTeX (e.g. `soul`).
- The document may be built with either XeTeX or pdfLaTeX and some packages or settings must differ by engine.

## How

1. Load the `iftex` package: `\usepackage{iftex}`.
2. Use `\ifXeTeX` … `\else` … `\fi` to branch:
   - In the `\ifXeTeX` branch: XeTeX-only packages and settings.
   - In the `\else` branch: pdfLaTeX-only packages and settings (e.g. packages that conflict with XeTeX).

## Example

```latex
\usepackage{iftex}

\ifXeTeX
  % XeTeX-only: avoid packages that conflict with XeTeX
\else
  \usepackage{soul}  % incompatible with XeTeX; use only for pdfLaTeX
  % pdfLaTeX-only packages/settings
\fi
```

Use `\usepackage` (not `\usepacakge`). Keep each branch minimal: only the packages or commands that must differ by engine.

## Notes

- `iftex` also provides `\ifLuaTeX`, `\ifpdfTeX` for other engines if needed.
- Prefer a single conditional block per logical choice so the preamble stays readable.
