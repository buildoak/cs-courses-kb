# A2: Multi-Domain Sentiment

## Problem Statement

This homework is supervised sentiment analysis in a ternary label setting: positive, negative, neutral. The twist is that the model has to work across domains. The core data comes from DynaSent Round 1, DynaSent Round 2, and SST-3, and the bake-off asks you to generalize to a hidden test set that includes mystery examples outside the familiar domains.

The notebook is half homework, half discipline check. You need to build baseline systems, compare them fairly, and make sure you are not peeking at the public test labels while tuning.

## Key Techniques

- **Sparse lexical features**: unigram count dictionaries.
- **`DictVectorizer` discipline**: keeping train and test in the same feature space.
- **Linear classifiers**: softmax models as a strong baseline.
- **Transformer tokenization**: using the model’s tokenizer instead of ad hoc splitting.
- **Contextual representations**: extracting hidden states for classification.
- **Fine-tuning**: adapting pretrained encoders to the sentiment task.
- **Original-system design**: combining the best pieces into one bake-off submission.

## Implementation Walkthrough

The notebook starts with dataset triage. DynaSent Round 1 and Round 2 are adversarially collected, while SST-3 carries phrase-level supervision that the homework reduces to sentence-level labels. That matters because the assignment is explicitly about cross-domain robustness, not just in-domain accuracy.

Question 1 is the classic sparse baseline. You write a feature function that counts unigrams, vectorize the resulting dictionaries, and train a scikit-learn classifier. The critical implementation detail is not to call `fit_transform` on test data. The notebook spends time on `DictVectorizer` because almost every bug in this kind of pipeline is a train/test representation mismatch.

Question 2 moves to transformers. You batch-tokenize with the model’s own tokenizer, construct contextual representations, and then fine-tune a pretrained encoder. The notebook makes the batching and masking mechanics explicit because this is where most of the engineering friction lives.

Question 3 is the open-ended system. You can mix sparse features, transformer outputs, or any other sensible strategy, as long as the result is defensible and reproducible.

Question 4 is the bake-off export. The notebook expects a CSV with a `prediction` column, preserving the original unlabeled rows. The assignment is strict about naming because the grader is looking for one well-formed file, not a family of ad hoc experiments.

## What You Learn

- Sparse baselines are still strong when the feature space is well chosen.
- Cross-domain sentiment is harder than single-domain sentiment.
- Tokenization is part of the model, not a preprocessing footnote.
- Fine-tuning beats hand-built features when you have enough structure and enough data.
- The bake-off is mostly an exercise in restraint: tune on dev, export once, stop.

## Sources

- CS224U sentiment homework notebook: https://raw.githubusercontent.com/cgpotts/cs224u/main/hw_sentiment.ipynb
- Stanford course index, sentiment assignment link and schedule: https://web.stanford.edu/class/cs224u/2022/index.html
