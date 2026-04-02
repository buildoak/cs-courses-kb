# CS221 Assignment 1: Foundations

## Problem Statement
Stanford CS221's foundations track is the "make the Python stop being the problem" assignment. The Autumn 2025 lecture repo places foundations first, then linear regression and classification, which is the right mental model for this homework: get fluent with the primitives before the course turns into ML machinery. The matching homework code in [`foundations/submission.py`](https://github.com/dchen327/stanford-cs221-code/blob/master/foundations/submission.py) covers string/list work, sparse vectors, counting, and a memoized recurrence.

The ML hook is explicit: the sparse-vector helpers are annotated as later-useful for linear classifiers.

## Key Techniques
- Dictionary and set operations for text munging.
- `collections.Counter` for token frequency counting.
- Sparse vectors represented as maps from feature keys to numeric values.
- Recursive dynamic programming with memoization for the longest palindromic subsequence.
- O(n^2) subproblem reuse instead of brute-force substring search.

## Implementation Walkthrough
- Start with the low-friction pieces: compare words lexicographically, split text, and compute Euclidean distance from coordinate pairs.
- Treat a sparse vector as a dictionary keyed by features, then implement dot products and in-place accumulation by iterating only over present keys.
- `findSingletonWords` is the clean counting pass: split on whitespace, count, filter to frequency 1.
- `computeLongestPalindromeLength` is the real algorithmic turn. Cache on substrings, compare end characters, and recurse inward when they match.
- The assignment's sparse-vector helpers exist for a reason. They are the bridge from toy Python to linear classifiers later in the course.

## What You Learn
- Small Python utilities are the substrate for the rest of the class.
- Sparse representations are the practical shape of ML features.
- Memoization is the first serious "algorithmic compression" trick in the course.
- Once you can move comfortably between text, sets, dicts, and vectors, the later ML assignments stop feeling like a language problem.
