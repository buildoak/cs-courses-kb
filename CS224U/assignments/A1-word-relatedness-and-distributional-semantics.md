# A1: Word Relatedness and Distributional Semantics

## Problem Statement

The Word Relatedness homework asks you to predict how similar two words are on a 0-1 human rating scale, then beat that baseline on a hidden bake-off test set. The development split is on the order of 5,000 scored examples; the bake-off test set has 1,500 pairs, with no pair overlap between dev and test. The score to optimize is Spearman correlation, not accuracy.

The assignment is really a compact lab in distributional semantics. You are not training a single giant model. You are trying different ways to represent words and different ways to measure distance between them, then checking which combination lines up best with human judgments.

## Key Techniques

- **Count-based vector spaces**: word-by-word co-occurrence matrices.
- **Matrix reweighting**: positive PMI, observed/expected, TF-IDF, and t-test style weighting.
- **Similarity metrics**: Euclidean, cosine, and matching-based distances.
- **Dimensionality reduction**: LSA/SVD for compressed semantic spaces.
- **Contextual-to-static pooling**: deriving fixed word vectors from BERT-style encoders.
- **Learned distance functions**: k-NN style scoring over the dev set.
- **Error analysis**: comparing the best and worst predicted pairs to the human scores.

## Implementation Walkthrough

The homework starts by loading a Pandas data frame of word pairs and human scores. The first thing to inspect is the score distribution, because the test set is drawn from the same kind of relatedness scale even though the word pairs are disjoint from dev.

The first baseline is **positive PMI**. That fits directly with the lecture notebook on distributional representations: build a co-occurrence matrix, reweight it so informative co-occurrences stand out, then score word pairs from the resulting vectors. The lecture notebook lays out the matrix-design choices, the distance functions, and the rationale for moving away from raw counts.

From there, the assignment asks for **LSA-style compression**. Instead of using the raw sparse matrix, you test different low-dimensional projections and see which dimensionality best preserves relatedness. That is the first point where the homework stops being about "what does the matrix contain?" and starts being about "what geometry survives projection?"

The next baseline is **t-test reweighting**, which gives you another way to suppress generic counts and amplify pair-specific signal. The implementation is mostly about wiring the reweighting into the same evaluation loop, so you can compare it against PMI and LSA on the same dev set.

Then the assignment moves out of classic count-based VSMs. One branch pools **BERT representations** into static vectors for the words in the pair. Another branch treats relatedness as a **learned distance problem**, where a k-nearest-neighbor style model learns a scoring rule from the development data instead of relying on a fixed metric like cosine.

The last step is the bake-off entry. The point is not a single canonical solution. The point is to assemble the strongest pipeline you can justify, run it once on the hidden test vocabulary, and generate the submission file.

## What You Learn

- Relatedness is a scoring problem, not a class-label problem.
- Representation choice matters more than model branding.
- PMI, LSA, and BERT are all ways of forcing semantic structure into vector form.
- Similarity metrics are not interchangeable; the ranking they produce changes the outcome.
- A good bake-off system is usually a small stack of simple pieces, not one clever trick.

## Sources

- Stanford course index, word relatedness assignment link and schedule: https://web.stanford.edu/class/cs224u/2022/index.html
- Word relatedness overview video transcript: `summarize --extract https://youtu.be/egEzcwbej1E --plain --timestamps`
- CS224U distributional semantics notebook: https://raw.githubusercontent.com/cgpotts/cs224u/main/vsm_01_distributional.ipynb
- CS224U lecture notebook README context: https://github.com/cgpotts/cs224u
