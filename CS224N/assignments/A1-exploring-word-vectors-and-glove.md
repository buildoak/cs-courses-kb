# A1: Exploring Word Vectors and GloVe

## Problem Statement

Assignment 1 is an intuition-building lab for word embeddings. Stanford's public notebook asks you to build count-based vectors from scratch, reduce them to a plottable space, and then compare that geometry against pretrained GloVe vectors. The goal is not to train a large model end to end. The goal is to internalize what distributional semantics looks like when you can inspect every intermediate artifact.

## Key Techniques

- **Co-occurrence matrices**: represent each word by the counts of nearby words inside a fixed context window.
- **Vocabulary indexing**: map sorted distinct tokens to matrix rows and columns so every later step is deterministic.
- **Truncated SVD**: compress a large sparse co-occurrence matrix into a low-dimensional embedding space you can visualize.
- **Pretrained GloVe vectors**: load dense vectors trained elsewhere and compare their neighborhood structure to the count-based baseline.
- **Cosine similarity and analogies**: use vector direction, not raw magnitude, to retrieve neighbors and analogy completions.
- **Bias inspection**: look at where pretrained vectors encode social regularities that are statistically present but normatively undesirable.

## Implementation Walkthrough

The notebook starts with a small Reuters corpus loader that lowercases text and wraps each document with `<START>` and `<END>` tokens. From there, the first coding task is `distinct_words(corpus)`: gather the unique tokens across all documents, sort them, and return both the list and its size. That sorted vocabulary becomes the canonical ordering for the entire assignment.

Next comes `compute_co_occurrence_matrix(corpus, window_size=4)`. The implementation is conceptually simple: for every token position, look left and right up to the window boundary, and increment the symmetric count matrix for every center-context pair. The matrix is large but interpretable: each row is a count-based embedding whose dimensions are actual neighboring words.

After that, the notebook asks for `reduce_to_k_dim(M, k=2)` using `sklearn.decomposition.TruncatedSVD` with `n_iter=10`. This is the bridge from raw counts to visual intuition. You take the huge co-occurrence matrix, project it down to two dimensions, normalize the returned vectors, and then plot hand-picked words. The point is to see which semantic relations survive compression and which ones smear together.

Part 2 switches from count-based vectors to pretrained GloVe embeddings loaded through Gensim `KeyedVectors`. Instead of recomputing semantics from a small corpus, you sample from a much richer pretrained space, build a matrix for required words, and visualize it the same way. The notebook then uses `most_similar(...)` to inspect neighbors, test analogies, and surface failure cases. This is where the assignment moves from "how do we build vectors?" to "what do these vectors actually encode?"

Pedagogically, the assignment is well designed because it makes the comparison concrete. Co-occurrence plus SVD shows the old-school linear-algebra route. GloVe shows what happens when you move to dense, pretrained embeddings that still retain geometric structure but behave much better on similarity and analogy tasks.

## What You Learn

- Count-based embeddings are easy to reason about because every dimension has a lexical interpretation, but they become unwieldy fast.
- Dimensionality reduction is not just for plotting; it exposes whether semantic structure is actually present in the counts.
- GloVe improves local neighborhood quality because it compresses broad corpus statistics into dense vectors.
- Cosine similarity is the right default retrieval metric when vector direction matters more than magnitude.
- The same embedding space that captures useful semantics can also preserve dataset bias.

## Sources

- Stanford A1 preview notebook: https://web.stanford.edu/class/cs224n/assignments/a1_preview/exploring_word_vectors.html
- Public GitHub notebook mirror: https://raw.githubusercontent.com/amanchadha/stanford-cs224n-assignments-2021/main/a1/exploring_word_vectors.ipynb
- GloVe paper linked by the assignment: https://nlp.stanford.edu/pubs/glove.pdf
