# Repository Guidelines

## Project Structure & Module Organization

This repository is a UCAS thesis LaTeX template. `Thesis.tex` is the root document and loads content from `Tex/`. Keep front matter, main chapters, appendices, and back matter in the existing files such as `Tex/Frontmatter.tex`, `Tex/Chap_Intro.tex`, and `Tex/Appendix.tex`. Class, package, and configuration code lives in `Style/`; edit these files only for template-wide behavior. Bibliography styles and references are in `Biblio/`, with entries in `Biblio/ref.bib`. Figures and PDFs used by the document belong in `Img/`. `Tmp/` contains generated build artifacts and should not be treated as source.

## Build, Test, and Development Commands

- `bash artratex.sh xa Thesis.tex`: compile with `xelatex` and `bibtex`, then open `Tmp/Thesis.pdf`.
- `bash artratex.sh xb Thesis.tex`: compile with `xelatex` and `biber`.
- `bash artratex.sh x Thesis.tex`: compile without rebuilding bibliography.
- `bash artratex.sh pa Thesis.tex`: test `pdflatex` compatibility with `bibtex`.

The script writes auxiliary files and PDFs under `Tmp/`. For CI or headless environments, be aware the script tries to open the final PDF with `xdg-open` or `open` after compilation.

## Coding Style & Naming Conventions

Use two-space indentation for nested LaTeX environments and keep one sentence or command group per line when it improves diffs. Name chapter files with the existing `Tex/Chap_*` pattern and keep shared document metadata in `Tex/Frontinfo.tex`. Store reusable formatting in `Style/ucasthesis.cls`, `Style/ucasthesis.cfg`, or `Style/artratex.sty` rather than duplicating commands in chapters. Keep image names short, lowercase when possible, and stable after publication.

## Testing Guidelines

There is no automated test suite. Validate changes by compiling `Thesis.tex` with the engine affected by your edit, usually `bash artratex.sh xa Thesis.tex`. Check the log in `Tmp/Thesis.log` for warnings, missing references, font issues, and overfull boxes. For bibliography changes, run a bibliography build (`xa` or `xb`) and inspect citation output in `Tmp/Thesis.pdf`.

## Commit & Pull Request Guidelines

Recent history uses short, imperative or descriptive subjects such as `improve artratex.sty`, `minor updates`, and `Update Backmatter.tex`. Follow that style: keep the first line concise and mention the touched area when useful. Pull requests should describe the template behavior changed, list the compile command used, and include screenshots or PDF page references for visual layout changes. Link relevant issues when fixing reported formatting or compatibility problems.

## Generated Files & Assets

Do not commit temporary files from `Tmp/` unless the project explicitly decides to publish a sample PDF. Prefer vector PDF assets for logos and diagrams when available; otherwise use clear, publication-resolution raster images in `Img/`.
