# A2: Neural Dependency Parsing

## Problem Statement

The second half of Stanford's public Assignment 2 asks you to build a neural transition-based dependency parser and optimize it for unlabeled attachment score (UAS). Conceptually, the parser must decide whether to `SHIFT`, `LEFT-ARC`, or `RIGHT-ARC` at each step while incrementally constructing a tree. Practically, this assignment is where CS224N makes the jump from "derive gradients on paper" to "wire a real neural model around structured state."

## Key Techniques

- **Transition-based parsing**: maintain a stack, buffer, and dependency list, then predict the next transition.
- **Arc-standard style actions**: `SHIFT`, `LEFT-ARC`, and `RIGHT-ARC` mutate parser state one step at a time.
- **Feature extraction from parse state**: encode the top of stack, front of buffer, and left/right children into a fixed-length input vector.
- **Embedding lookup without `nn.Embedding`**: manually gather token vectors and flatten them into a classifier input.
- **Feed-forward classification**: map parser-state features into logits over three transition classes.
- **Adam + dropout**: use modern optimization and regularization rather than bare SGD.

## Implementation Walkthrough

The parser mechanics live in `parser_transitions.py`. `PartialParse` initializes with `stack = ['ROOT']`, `buffer = sentence[:]`, and an empty dependency list. `parse_step(...)` is intentionally small: `SHIFT` pops the next buffered token onto the stack, `LEFT-ARC` removes the second item on the stack and records it as a dependent of the top item, and `RIGHT-ARC` removes the top item and records it as a dependent of the second item. `minibatch_parse(...)` then runs many partial parses at once, repeatedly asking the model for the next action until each sentence is fully reduced.

The structural feature engineering is in `parser_utils.py`. The parser vectorizes CoNLL examples and extracts a fixed feature set from the current parse state. In the public unlabeled configuration, `n_features` becomes 36: 18 word IDs plus 18 POS-tag IDs. Those come from the top three stack items, the first three buffer items, and left/right children plus grandchildren for the top two stack elements. This fixed layout is the crucial trick that converts a variable-size parse state into a neural network input.

`parser_model.py` implements the classifier. It treats the pretrained embedding table as a trainable `nn.Parameter`, manually gathers the embeddings for each feature ID, flattens them into a single vector, projects that vector to a 200-unit hidden layer, applies ReLU, then dropout with probability 0.5, and finally projects to 3 logits. Xavier initialization is used for both learned weight matrices. This is not a large model, but it is exactly the kind of architecture that makes the parser fast enough to train and expressive enough to beat hand-written scoring rules.

Training is driven by `run.py`. The script uses Adam with learning rate `5e-4`, cross-entropy loss, minibatches of 1,024 examples, and 10 epochs. After every epoch it evaluates on the development set, tracks the best dev UAS, saves the best weights, and restores them for final test evaluation. That training loop matters because the assignment is not just about implementing transitions correctly; it is about turning those transitions into a learned policy that generalizes to unseen sentences.

One source wrinkle is worth being explicit about: Stanford's current public A2 handout groups dependency parsing into Assignment 2, while the widely mirrored 2021 public codebase places the parser under `a3/`. The underlying parser design is the same style of exercise, and the 2021 repo remains the clearest public code reference for the implementation details.

## What You Learn

- Transition systems turn parsing into a sequence of local classification decisions.
- Good parser performance depends as much on feature design as on the neural network itself.
- Embeddings are useful beyond similarity tasks; they can encode parser state in a learnable way.
- Dropout and Adam are not abstract lecture topics here; they directly stabilize parser training.
- UAS is a clean task metric because it measures whether the parser attached each token to the correct head.

## Sources

- Stanford CS224N A2 handout: https://cs224n.stanford.edu/assignments_w25/a2.pdf
- Public implementation reference, transition system: https://raw.githubusercontent.com/amanchadha/stanford-cs224n-assignments-2021/main/a3/parser_transitions.py
- Public implementation reference, feature extraction: https://raw.githubusercontent.com/amanchadha/stanford-cs224n-assignments-2021/main/a3/utils/parser_utils.py
- Public implementation reference, model: https://raw.githubusercontent.com/amanchadha/stanford-cs224n-assignments-2021/main/a3/parser_model.py
- Public implementation reference, training loop: https://raw.githubusercontent.com/amanchadha/stanford-cs224n-assignments-2021/main/a3/run.py
