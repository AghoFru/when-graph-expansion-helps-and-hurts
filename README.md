# When Graph Expansion Helps and Hurts

A fixed-budget measurement of one-hop graph expansion for retrieval. On the 141,232-document
HotpotQA distractor corpus, where every bridge question's gold pair is hyperlinked by
construction, one-hop hyperlink pools reranked by BGE-reranker-v2-m3 raise complete recall@10 from
0.784 to 0.934 against equal-size hybrid pools. The gain is on bridge questions (+0.191), and
comparison questions show -0.013. On LitSearch, citation pools lower Recall@20 by 0.037
[-0.068, -0.007]. They help only on inline ACL queries.

The graph pool is the eight top hybrid (BM25 plus dense) documents and their one-hop graph
neighbors. The baseline is the same number of top hybrid documents. Both pools are reranked by the
same cross-encoder, so the comparison holds the reranker and the pool size fixed. No learned edge
scorer is used in these results. On LitSearch the cap of 16 neighbors per seed binds for 368 of
597 queries.

## Results

| Corpus | Metric | Hybrid pool | Graph pool | Difference [95% CI] |
|---|---|---:|---:|---:|
| HotpotQA distractor, 7,405 questions | Complete recall@10 | 0.7839 | 0.9337 | +0.1498 [0.1407, 0.1589] |
| HotpotQA bridge, 5,918 questions | Complete recall@10 | 0.7310 | 0.9218 | +0.1908 |
| HotpotQA comparison, 1,487 questions | Complete recall@10 | 0.9946 | 0.9812 | -0.0134 |
| LitSearch, 597 queries | Recall@20 | 0.7361 | 0.6992 | -0.0369 [-0.0681, -0.0066] |

All rows use E5-large-v2 and BGE-reranker-v2-m3. The paper has the LitSearch split by query source.

## Limits

- The HotpotQA corpus is 141,232 paragraphs from the `distractor` configuration, not full
  Wikipedia. All 5,918 validation bridge questions have their gold pair hyperlinked, against 1 of
  1,487 comparison questions. The HotpotQA gain is largely a property of this construction.
- No multi-hop retriever baseline (MDR, HippoRAG) was run. GAR was run only with MiniLM, on the
  hyperlink graph. At a fixed 32-document budget it ties the one-hop pool (0.7633 against 0.7600).
- The random-edge control draws uniform targets and falls below its own hybrid pool, so it is weak.
- Rescue coverage and the rescue strata reuse gold labels. Most of the in-degree contrast is
  one-hop reachability. The LitSearch degree contrast depends on the standardization variables.
- Intervals are per comparison and not adjusted for the number of reported slices.

## Build

The repository holds the manuscript source, bibliography, one figure, and the rendered PDF. The
experiments and datasets are not included. Building needs `latexmk` and `pdflatex`.

```bash
latexmk -pdf -jobname="When Graph Expansion Helps and Hurts" \
  -interaction=nonstopmode -halt-on-error paper.tex
```

The output is `When Graph Expansion Helps and Hurts.pdf`.
