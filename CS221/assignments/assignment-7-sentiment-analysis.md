# CS221 Assignment 7: Sentiment Analysis

## Problem Statement
The official [Sentiment Analysis](https://stanford-cs221.github.io/autumn2024/assignments/sentiment/index.html) assignment starts as a hand-worked binary classifier and ends as a small text ML pipeline. Problem 1 makes you step through hinge-loss SGD on four toy reviews. Problem 2 switches to logistic prediction and asks why the gradient shrinks when the output saturates, which is the neural-net bridge. Problem 3 turns that into code in [`sentiment/submission.py`](https://raw.githubusercontent.com/dchen327/stanford-cs221-code/master/sentiment/submission.py): bag-of-words features, SGD training, synthetic data generation, and character n-grams. Problem 4 adds a fairness lens with maximum group loss.

## Key Techniques
- Sparse word-count features in Python dicts.
- Hinge loss and SGD updates on active features only.
- Logistic squashing and vanishing-gradient intuition.
- Character n-grams without whitespace as a fallback feature space.
- Average loss vs maximum group loss for subgroup robustness.
- K-means as a side quest in sparse-vector arithmetic.

## Implementation Walkthrough
- `extractWordFeatures` is the starter task: split on whitespace, count tokens, return a sparse feature map.
- `learnPredictor` is standard online learning. The raw code initializes sparse weights, computes the hinge margin with `dotProduct`, and uses `increment` to add `eta * y * phi(x)` when the margin is below 1.
- `generateDataset` and its nested `generateExample` give you a controllable test harness: sample sparse examples, score them with a known weight vector, and label them by sign.
- `extractCharacterFeatures(n)` strips spaces and counts overlapping n-grams. That is the whole trick behind making the classifier less brittle to tokenization.
- The toxicity section is the conceptual twist. Classifiers D and T look similar in average loss, but group loss exposes whether a model is leaning on demographic mentions or toxicity words.
- The raw solution file [`sentiment/submission.py`](https://raw.githubusercontent.com/dchen327/stanford-cs221-code/master/sentiment/submission.py) shows the full shape of the homework in one place, including the later `kmeans` implementation.

## What You Learn
- A linear classifier is mostly sparse bookkeeping plus the right loss.
- Word features are enough to get surprisingly far, but character features rescue you when word boundaries fail.
- Neural nets inherit the same gradient problem in a more expensive costume.
- Average accuracy can hide subgroup damage; maximum group loss makes it visible.
- Most of the assignment is really about feature design, not model glamour.
