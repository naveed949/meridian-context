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

**Step 1** lives in `notebooks/phase1_dense_rag_baseline.ipynb` under **Step 1**: after the shape sanity check in §0.3, run the **pairwise cosine** cells on the curated sentence set. Written recap: [phase1-step1.md](./phase1-step1.md).

Then continue with **Step 2** (chunk → embed → top-k) per the notebook’s “Next (Step 2+)” cell and `docs/learning-plan.md` Phase 1.
