# 🚀 pixi-tex
[![Platforms](https://img.shields.io/badge/platforms-linux--64%20%7C%20linux--aarch64%20%7C%20osx--64%20%7C%20osx--arm64-blue)](https://pixi.sh)
[![Tectonic](https://img.shields.io/badge/powered%20by-Tectonic-orange)](https://tectonic-typesetting.github.io/)
[![Pixi](https://img.shields.io/badge/powered%20by-Pixi-yellow)](https://pixi.prefix.dev/latest/)
[![License: MIT](https://img.shields.io/badge/License-MIT-red.svg)](https://opensource.org/licenses/MIT)
> ⚡ Super handy Pixi + Tectonic based TeX environment

## 🎯 Quick Start

### 📦 Installation
```bash
pixi install
pixi run setup-jmlr     # or any other conference / journal format
pixi run link           # create .cursor -> .claude symlink (if needed)
```

### ✍️ Writing & Typesetting
1. Install the [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) extension in VS Code
2. Open your `.tex` file
3. Press `Ctrl+S` to automatically typeset! 🎉

### 🤖 AI-Assisted Writing & Building
Skills in `.claude/skills/` are automatically used by [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Cursor users can also access them via the `.cursor` symlink (created by `pixi run link`).

### 🎨 Font Configuration

> [!CAUTION]
> The project uses Tectonic for typesetting, which is based on XeTeX. Conference style files (CoRL, NeurIPS, ICML, etc.) typically set fonts via pdfLaTeX-era NFSS commands like `\renewcommand{\rmdefault}{ptm}` (Times) or `\renewcommand{\sfdefault}{phv}` (Helvetica). Under XeTeX's TU encoding, the required font definition files (`TUqtm.fd`, `TUphv.fd`) don't exist, so fonts **silently fall back to Latin Modern** — the PDF builds without errors but looks different.

After loading conference-specific style files, add the following to fix font rendering:

```tex
\usepackage{iftex}
\ifXeTeX
  \usepackage{newtxtext,newtxmath}
\fi
```

`newtxtext` internally uses `fontspec` to load TeX Gyre Termes (Times) and TeX Gyre Heros (Helvetica) from the TeX distribution bundle, which Tectonic auto-downloads. `newtxmath` provides matching math fonts.

> [!TIP]
> `tex-engine-conditional` skill automatically guides the fix when font issues are detected.

## 🔧 Advanced Features

### 🛡️ Pre-commit Hooks (Optional)
Enable code quality checks with prek:
```bash
pixi install -a
pixi run prek-install
```

## :art: Customization

Add new rules to the `.claude/skills/` directory to enhance the writing checks.

## 📄 License

MIT
