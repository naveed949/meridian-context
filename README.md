# Meridian Context

**Grounded context retrieval for production LLM applications.**

Meridian Context is a retrieval stack—not a demo notebook—built for teams that need **hybrid search**, **reranking**, **citation-backed answers**, and a **real evaluation loop**. It targets the failure modes that keep showing up in the wild: weak chunking, dense-only retrieval, no rerank, and no way to know if a change actually helped.

## What this is

- Ingestion pipeline with structure-aware chunking and optional contextual enrichment  
- Hybrid retrieval (semantic + lexical) and a second-stage reranker  
- Query API designed for **provenance** (sources you can show to users and auditors)  
- Eval harness (golden sets + automated metrics) so refactors don’t fly blind  
- Docker-friendly layout for shipping, not just local tinkering  

## Documentation

- **[Learning plan & roadmap](docs/learning-plan.md)** — phased curriculum, target architecture, checklist, and “done” criteria  
- **[Docs index](docs/README.md)**  
- **[Phase 1 — Step 0 setup](docs/phase1-step0.md)** — Python 3.11+ venv, `requirements-phase1.txt`, corpus `data/phase1/`, notebook `notebooks/phase1_dense_rag_baseline.ipynb`
- **[Phase 1 — Step 1](docs/phase1-step1.md)** — pairwise cosine on a small sentence set (same notebook, after Step 0)

## Repository

**GitHub:** [`naveed949/meridian-context`](https://github.com/naveed949/meridian-context)

## Getting started (local)

```bash
cd ~/LEARNING/AI/meridian-context
git init -b main
git add README.md .gitignore LICENSE
git commit -m "chore: initial project scaffold"
```

Publish the **public** repository and push:

```bash
gh repo create meridian-context --public --source=. --remote=origin --push
```

(`gh` creates `naveed949/meridian-context` when you’re logged in as `naveed949`.)

### If you already created `production-rag-starter` on GitHub

Rename the remote repo (run from any clone of that repo, or use `-R`):

```bash
gh repo rename meridian-context -R naveed949/production-rag-starter
```

Then point this folder at the new URL:

```bash
git remote set-url origin https://github.com/naveed949/meridian-context.git
git push -u origin main
```

If you prefer a **new** repo instead, delete the old one on GitHub and use `gh repo create` as above.

## Status

Active development. Implementation follows a phased plan: ingestion → hybrid + rerank → eval → production hardening.

## License

MIT — see [LICENSE](LICENSE).
