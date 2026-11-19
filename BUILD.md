# Build the manuscript

Requirements: `latexmk`, `pdflatex`, and the LaTeX packages used by the source.

From this directory, run:

```bash
latexmk -pdf -jobname="When Graph Expansion Helps and Hurts" \
  -interaction=nonstopmode -halt-on-error paper.tex
```

The reviewed output is `When Graph Expansion Helps and Hurts.pdf`.
