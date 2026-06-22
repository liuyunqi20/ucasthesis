# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ucasthesis is a LaTeX thesis template for the University of Chinese Academy of Sciences (UCAS). It supports Bachelor, Master, Doctor, and Postdoctoral degree types, and is compatible with XeLaTeX, LuaLaTeX, and pdfLaTeX engines.

## Build Commands

Compile the thesis (run from project root):
```bash
# Full build using the provided script
bash artratex.sh

# Or manually with xelatex + bibtex
xelatex Thesis
bibtex Thesis
xelatex Thesis
xelatex Thesis
```

On Windows: use `artratex.bat` instead of `artratex.sh`.

## Architecture

### Document Structure

The entry point is `Thesis.tex`, which `\input{}`s files from the `Tex/` directory in this order:

1. **`Tex/Frontinfo.tex`** — Thesis metadata: title, author, advisor, degree, major, institute, dates. These use custom commands defined in `ucasthesis.cls` (e.g., `\title`, `\author`, `\advisor`, `\degree`, `\DEGREE`, `\TITLE`, `\ADVISOR`). Chinese metadata uses lowercase commands; English uses UPPERCASE.

2. **`Tex/Prematter.tex`** — Pre-title-page content (opening report, etc.)

3. **`Tex/Frontmatter.tex`** — Front matter: title pages (`\maketitle`, `\MAKETITLE`), declaration (`\makedeclaration`), abstracts, table of contents, list of figures/tables.

4. **`Tex/Mainmatter.tex`** — Main content chapters. Each chapter is a separate file included via `\input{Tex/Chap_Xxx}`. The guide chapter is `Tex/Chap_Guide.tex`; the user's actual content starts at `Tex/Chap_Intro.tex`.

5. **`Tex/Appendix.tex`** — Appendices.

6. **`Tex/Backmatter.tex`** — Back matter: bibliography, acknowledgment, resume.

### Style System (3-layer architecture)

The style system follows a separation-of-concerns pattern:

1. **`Style/ucasthesis.cls`** — Document class. Loads `ctexbook`, handles page layout (margins, line spacing), defines title page templates for each degree type, declaration page, keywords environments, TOC formatting, and chapter/section heading styles via `\ctexset`. Metadata commands (`\title`, `\author`, `\advisor`, `\DEGREE`, etc.) are defined here with internal storage macros (`\ucas@value@ch@*` and `\ucas@value@en@*`). At end of package, loads the config file.

2. **`Style/artratex.sty`** — Functional package. Handles font configuration (platform-specific CJK fonts, XITS math fonts for XeLaTeX), bibliography engine selection (bibtex vs biber with natbib vs biblatex), citation style (numbers/super/authoryear/alpha), optional packages (tikz, color, geometry, list/algorithm, table), hyperref setup, header/footer styles (fancyhdr), and caption configuration (bicaption for bilingual captions).

3. **`Style/ucasthesis.cfg`** — User configuration. Loaded at end of class. This is where users customize: font options, citation style, bibliography engine, and feature toggles passed as options to `artratex.sty`. Contains both Chinese and English label strings for all degree types.

### Key Relationships

- `Thesis.tex` uses `\documentclass[...]{Style/ucasthesis}` and `\usepackage[...]{Style/artratex}`
- `ucasthesis.cls` loads `ucasthesis.cfg` at `\AtEndOfPackage`
- `ucasthesis.cfg` configures `artratex.sty` options and provides label text strings
- `artratex.sty` handles all package loading and low-level formatting

### Bibliography

- References are in `Biblio/ref.bib`
- BST files for GB/T 7714 citation standard are in `Biblio/`
- The bibliography engine (bibtex/biber) and citation style are configured in `ucasthesis.cfg` via options passed to `artratex.sty`

### Images

- Place figures in `Img/` directory (already set as `\graphicspath`)
- Logo file: `Img/ucas_logo.pdf`

## When Editing

- To change thesis content: edit files in `Tex/` (Frontinfo.tex for metadata, Chap_*.tex for chapters)
- To change formatting/style: edit `Style/ucasthesis.cfg` first; only modify `.cls`/`.sty` for deep changes
- To add new chapters: create `Tex/Chap_NewChapter.tex` and add `\input{Tex/Chap_NewChapter}` in `Tex/Mainmatter.tex`
- The `\DEGREE` value in Frontinfo.tex determines which title page template is used (Bachelor/Master/Doctor/Postdoctor)