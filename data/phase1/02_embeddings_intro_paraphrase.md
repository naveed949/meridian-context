# Semantic vectors (paraphrased)

**Semantic embeddings** turn language into a numeric vector with a fixed size. Models learn to place paraphrases and related ideas near each other in that space.

When you search, you turn the user question into a vector and ask: **which stored passages align best in direction?** Cosine similarity is a standard way to score that alignment because it focuses on angle, not magnitude.

**Lesson:** similar *meaning* should beat similar *wording*—when the embedding model cooperates.
