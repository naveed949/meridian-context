# Embeddings as semantic fingerprints

An **embedding** maps text into a fixed-length vector of real numbers. The model is trained so that texts with similar meaning produce vectors that point in similar directions in high-dimensional space.

For retrieval, you compare a **query vector** to many **chunk vectors**. A common score is **cosine similarity**: it measures alignment of direction (angle), not raw distance along the vector.

**Takeaway:** embeddings are not a database row; they are coordinates you can rank for “closeness of meaning.”
