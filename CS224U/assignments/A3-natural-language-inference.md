# A3: Natural Language Inference

## Problem Statement

NLI asks whether a premise justifies an inference to a hypothesis. The label space is usually contradiction, entailment, or neutral. In the CS224U homework/bake-off, the point is to build models that go beyond strict logic and handle local textual inference with real language variation.

The course materials frame this as a progression: simple feature baselines, sentence-encoding models, chained models, attention, and then error analysis against dataset artifacts. The homework is not just about getting a score. It is about seeing which shortcuts the model takes and how much of the benchmark is really solved by surface cues.

## Key Techniques

- **Sparse pair features**: word overlap and word cross-product features.
- **Hypothesis-only baselines**: measuring dataset bias before trusting any higher score.
- **Sentence-encoding models**: separate encoders for premise and hypothesis.
- **Chained models**: premise first, hypothesis second, one shared representation flow.
- **Attention mechanisms**: local alignment between premise and hypothesis tokens.
- **MultiNLI error analysis**: using annotations to inspect systematic failures.

## Implementation Walkthrough

The notebook starts with sparse features because they are surprisingly strong. `word_overlap_phi` counts shared words between premise and hypothesis. `word_cross_product_phi` explodes that into lexical pair features. In the notebook, the cross-product model is a strong lexicalized baseline, similar in spirit to the original SNLI baseline.

The next sanity check is the **hypothesis-only baseline**. That matters because NLI datasets often contain artifacts: the hypothesis alone can predict a lot. The notebook explicitly calibrates against that bias so a model score means something.

From there, the implementation moves to **sentence-encoding**. A premise and hypothesis each get their own representation, often via averaged or GloVe-based embeddings, and those vectors are concatenated for classification. The notebook then wraps that idea in a dedicated PyTorch module, `TorchRNNSentenceEncoderClassifierModel`, which pairs two RNN encoders with a classifier head.

The **chained** architecture is the next step up: one RNN processes the premise, and the hypothesis is read with the premise already in state. That makes the relation between the two sequences more direct than the independent sentence-encoding setup.

Attention comes last. The notebook points out that many strong NLI systems use attention to align informative premise/hypothesis tokens, and it ties that back to error analysis. The goal is not just better accuracy; it is a clearer picture of which local correspondences the model is exploiting.

## What You Learn

- NLI is a pairwise comparison problem, not a single-sentence classification problem.
- Lexical shortcuts are strong enough to fool you if you do not test against them.
- Hypothesis-only baselines are mandatory calibration, not a curiosity.
- Separate encoders, chained encoders, and attention are different ways of answering the same question: what relation holds between these two texts?
- Dataset artifacts are part of the task. Ignoring them produces fake confidence.

## Sources

- Stanford 2022 NLI overview handout: https://web.stanford.edu/class/cs224u/2022/slides/cs224u-nli-part1-handout.pdf
- Stanford 2020 NLI handout, sections on hand-built features, sentence-encoding, chained models, and attention: https://web.stanford.edu/class/cs224u/2020/materials/cs224u-2020-nli-handout.pdf
- Archived NLI notebook mirror: https://searchcode.com/total-file/318773732/nli_02_models.ipynb/
- Stanford course index, NLI materials and homework links: https://web.stanford.edu/class/cs224u/2022/index.html
