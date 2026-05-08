# Repository Guidelines

## Project Structure & Module Organization

This directory is a Chinese `hithesisbook` LaTeX example. The main entry point is `thesis.tex`, which selects class options and includes all thesis parts.

- `front/`: cover metadata and optional front matter such as notation.
- `body/`: main chapters included from `thesis.tex`.
- `back/`: conclusion, appendices, publications, indexes, authorization, acknowledgements, and resume files.
- `figures/`: graphics referenced through `\graphicspath{{figures/}}`.
- `reference.bib`: bibliography database.
- `hithesisbook.cls`, `hithesis.sty`, `*.bst`, `*.cfg`, `*.ist`: local template and bibliography/index support files.

Files such as `*.aux`, `*.log`, `*.xdv`, `*.idx`, `*.ind`, and `thesis.pdf` are build artifacts.

## Build, Test, and Development Commands

- `make thesis`: builds `thesis.pdf` with the current `Makefile` configuration (`METHOD = xelatex`).
- `make viewthesis`: builds the PDF and opens it with the platform default viewer.
- `latexmk -xelatex thesis`: alternative build path using `latexmkrc`.
- `make clean`: removes common intermediate files.
- `make cleanall`: removes intermediates and `thesis.pdf`.

The build expects XeLaTeX, BibTeX, `makeindex`, and `splitindex` to be available.

## Coding Style & Naming Conventions

Keep LaTeX source UTF-8 encoded. Use one logical chapter or back-matter file per `\include`/`\input`; name new files with lowercase ASCII words when possible, for example `body/literature_review.tex`. Keep template configuration in `thesis.tex` and content in the section files. Follow the existing comment language in nearby files, which is mostly Chinese for thesis-facing guidance.

Use BibTeX keys that are stable and readable, such as `authorYearTopic`. Store figures in `figures/` and reference them without hard-coded relative prefixes.

## Testing Guidelines

There is no automated test suite. Treat a clean PDF build as the primary validation:

1. Run `make clean`.
2. Run `make thesis`.
3. Check `thesis.log` for LaTeX errors, missing references, missing citations, and overfull boxes introduced by your change.

For bibliography or index changes, verify that `thesis.bbl`, `*.idx`, and `*.ind` are regenerated as expected.

## Commit & Pull Request Guidelines

This checkout has no existing commit history to infer a local convention. Use concise, imperative commit subjects, for example `Update bachelor cover metadata` or `Fix bibliography style selection`. Keep unrelated content, template, and generated-file changes in separate commits.

Pull requests should describe the affected thesis area, list the build command used, and mention any intentional changes to generated artifacts such as `thesis.pdf`.

## Agent-Specific Instructions

Read `thesis.tex` before changing section order, bibliography style, document class options, or front/back matter. Do not delete generated artifacts or run Git operations unless explicitly requested.
