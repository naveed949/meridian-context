# Phase 1 — Step 0 walkthrough

Step 0 from the foundations plan: **pick a minimal stack**, **create a tiny corpus**, **list it from the notebook**.

## 1. Pick the stack (decision)

| Piece | Choice for this repo |
|--------|----------------------|
| Runtime | Python **3.11+** |
| Notebook | **Jupyter** (VS Code can open the same `.ipynb`) |
| Linear algebra | **NumPy** (cosine / dot products later) |
| Embeddings | **sentence-transformers** locally, model **`all-MiniLM-L6-v2`** |

You could swap in a hosted embedding API later; Phase 1 stays simpler and reproducible with a local model.

## 2. Create the venv and install

From the repository root:

```bash
cd /path/to/meridian-context
python3.11 -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -U pip
pip install -r requirements-phase1.txt
```

Optional: install a **CPU-only** PyTorch wheel first if you want to avoid the default large CUDA stack:

```bash
pip install torch --index-url https://download.pytorch.org/whl/cpu
pip install -r requirements-phase1.txt
```

## 3. Tiny corpus (`data/phase1`)

Eight short `.md` files plus `data/phase1/README.md`:

- **Near-duplicates:** `01_*` / `02_*`, `07_*` / `08_*`
- **Synonym / vocabulary mismatch:** `05_*` (automobile) vs `06_*` (car)
- **Multi-topic:** ML/RAG notes vs recipe noise

## 4. Open the notebook and finish Step 0

```bash
jupyter notebook notebooks/phase1_dense_rag_baseline.ipynb
```

Run **Step 0** cells through **0.3**: Python version assert, corpus listing, import + tiny encode.

**Done criteria for Step 0:** listing shows all `data/phase1/*.md` files; embedding sanity cell runs once deps are installed.

## 5. Hand off to Step 1

In `notebooks/phase1_dense_rag_baseline.ipynb`, run **Step 1** (after Step 0):

1. **1.1** — One sentence → `(384,)` vector; bar chart of the first 24 dimensions; optional random 2D projection printout.
2. **1.2** — Table of cosine similarities: meeting paraphrases vs unrelated sentences (uses a small `cosine_sim` helper reused in Step 2).
3. **1.3** — Synonym-swap checkpoint: three pairs (synonym vs nonsense control); confirm cosine is higher for real synonyms.

Install plot support once: `pip install matplotlib` (included in `requirements-phase1.txt`). Then continue with **Step 2** in the same notebook (chunk → embed → top-k). **Step 3** (ANN theory only) is documented in [phase1-step3-ann-theory.md](./phase1-step3-ann-theory.md), with a short pointer in the notebook.
