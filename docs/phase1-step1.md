# Phase 1 — Step 1 walkthrough

**Goal:** See how a single embedding model scores **pairs** of sentences before you build an index over the whole corpus.

## What you do

1. Complete [Step 0](./phase1-step0.md) (venv, corpus listing, model load in `notebooks/phase1_dense_rag_baseline.ipynb`).
2. Run **Step 1** in the same notebook: a small set of sentences (paraphrase-style intro lines, **automobile** vs **car** fleet lines, one off-topic recipe line), then a **pairwise cosine similarity** matrix built with NumPy.

## Done criteria

- You can read the matrix and explain why **(0,1)** (same topic, different wording) is usually **higher** than **(0,4)** (unrelated topics).
- You notice whether **(2,3)** (same idea, different vocabulary) scores as high as you expect; if not, that is a preview of **lexical / synonym mismatch** in dense retrieval—useful for the Phase 1 failure-mode list later.

## Next

**Step 2** in the notebook: chunk `data/phase1/*.md`, embed chunks, brute-force top-k retrieval, then optional minimal prompting—see the “Next (Step 2+)” cell at the end of the notebook and the Phase 1 practice bullet in [learning-plan.md](./learning-plan.md).
