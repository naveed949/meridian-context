# Approximate nearest neighbors (ANN) and HNSW — elaborated

You store **many chunk embeddings** (thousands to billions). A **nearest-neighbor search** answers: *which stored vectors are closest to this query vector?* “Closest” is usually **cosine similarity** or **Euclidean distance** in the embedding space—they are related if vectors are **normalized** (unit length), which is common for semantic search.

This note contrasts **exact** search with **ANN** indexes (HNSW is one popular family) and uses **Mermaid** diagrams you can open in GitHub, VS Code, or many notebook renderers.

---

## 1. Exact search (brute force): compare everyone

**Idea:** There is no magic. For each chunk embedding `c_i`, score how similar it is to the query `q`, then sort and take **top-k**.

- **Pros:** You always see the *true* global best neighbors for your chosen metric (up to ties).
- **Cons:** Cost grows **linearly** with the number of chunks `N`: you touch every vector every query.

```mermaid
flowchart TD
  Q[Query embedding q]
  subgraph scan [Linear scan over all N chunks]
    A[Load chunk c_1, score sim q, c_1]
    B[Load chunk c_2, score sim q, c_2]
    M[...]
    Z[Load chunk c_N, score sim q, c_N]
    A --> B --> M --> Z
  end
  Q --> scan
  scan --> Sort[Sort or heap: keep top-k]
  Sort --> Out[Best k chunk ids]
```

**When brute force is fine:** Phase 1 with tens or hundreds of chunks on a laptop—your notebook can do a NumPy dot-product matrix multiply and never think about ANN.

---

## 2. ANN: trade a little accuracy for a lot of speed

**Idea:** Precompute a **data structure** (an **index**) so that at query time you **do not** visit all `N` points. You follow **hints**—“likely neighbors”—and stop early.

- **Pros:** Query time often sublinear or much smaller constant factors; essential at huge scale.
- **Cons:** **Approximate:** the returned neighbors might **miss** the true best point if the index “navigated” wrong. Quality is measured by **recall** (e.g. *what fraction of the true top-10 did we actually return?*).

```mermaid
flowchart LR
  subgraph exact [Exact search]
    E1[Touches all N vectors]
    E2[Guaranteed best-k for the metric]
  end
  subgraph ann [ANN search]
    A1[Touches only a subset of vectors]
    A2[Usually finds good neighbors fast]
    A3[Can miss the true argmax]
  end
  exact --> Latency1[Latency grows with N]
  ann --> Latency2[Latency much flatter in practice]
```

**Important:** Even if your **embedding model were perfect**, ANN could still return a slightly wrong set, because the **mistake is in the index traversal**, not in the vector arithmetic.

---

## 3. HNSW (Hierarchical Navigable Small World): intuition in layers

**HNSW** builds a **graph**: points are **nodes**, and **edges** connect “neighborhood” points so you can **greedily walk** from a start node toward the query.

To avoid getting stuck in local dead ends, HNSW uses **several layers**:

- **Upper layers:** **Fewer** nodes and **longer-range** links—think of them as a **highway** that quickly moves you to the right region of the space.
- **Lower layers:** **More** nodes and **shorter** links—**local roads** that refine the search.
- **Bottom layer:** Contains (typically) **all** vectors so nothing is “skipped” as a possible neighbor—but you still only **visit** a fraction of them thanks to the graph.

```mermaid
flowchart TB
  subgraph L2 ["Layer 2 — sparse, long jumps"]
    a((entry))
    b((far neighbor))
    a --- b
  end
  subgraph L1 ["Layer 1 — medium density"]
    c((node))
    d((node))
    c --- d
  end
  subgraph L0 ["Layer 0 — all vectors, local links"]
    e((v1))
    f((v2))
    g((v3))
    e --- f --- g
  end
  b -.->|"descend"| c
  d -.->|"descend"| e
```

**Query sketch (conceptual):**

1. Enter at a **fixed entry point** on the **top** layer.
2. **Greedy walk:** repeatedly move to the neighbor **closest to the query** until you can’t improve.
3. **Drop down** one layer; repeat greedy walk.
4. Continue until the **bottom** layer, then return the best candidates you actually visited (often with a **beam** or **efSearch**-style parameter: how greedy vs. how thorough).

```mermaid
flowchart TD
  Start[Start at entry node, top layer] --> Greedy1[Greedy walk: keep moving to neighbor closest to q]
  Greedy1 --> Stuck1[No better neighbor on this layer]
  Stuck1 --> Down{More layers below?}
  Down -->|yes| Drop[Move down one layer]
  Drop --> Greedy1
  Down -->|no| Refine[Return best candidates found on bottom layer]
```

That’s why ANN can miss the global best: **greedy** steps on a **sparse graph** might never enter the “correct” basin of attraction, or might stop exploring too early.

---

## 4. Vocabulary: what people mean in tuning

| Term | Plain meaning |
|------|----------------|
| **Recall@k** | Of the *true* top-k neighbors, how many appear in *your* top-k (or in a candidate pool before reranking). |
| **efSearch** (HNSW) | Larger → explores more nodes → **higher recall**, **slower** queries. |
| **M** (HNSW) | Roughly, max neighbors per node in the graph; affects index build and connectivity. |
| **IVFFlat** (another ANN family) | **Cluster** vectors into “cells”; search only a **shortlist** of cells near the query. Also approximate. |

You don’t need to implement HNSW in Phase 1; understanding **exact vs. approximate** and **why recall < 100%** is enough before you plug in **FAISS**, **pgvector** HNSW, or **Qdrant** later.

---

## 5. One-line summary

**Brute force** = *check everyone, correct, slow*. **ANN (e.g. HNSW)** = *follow a pre-built graph/layers, fast, may skip the true winner—tune recall vs. latency.*
