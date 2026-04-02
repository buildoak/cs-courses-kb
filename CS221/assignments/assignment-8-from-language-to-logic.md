# CS221 Assignment 8: From Language to Logic

## Problem Statement
The official [From Language to Logic](https://stanford-cs221.github.io/autumn2024/assignments/logic/index.html) assignment is the opposite of the sentiment homework: instead of squeezing meaning out of text, you write meaning down as formulas and let the inference engine do the rest. Problems 1-2 cover propositional and first-order translation. Problem 3 encodes the liar puzzle. Problem 4 turns parity and successor facts into a theorem proving task. The extra credit pushes the same machinery into semantic parsing, then the written sections finish with explainability and a soundness/completeness reflection. The implementation file is [`logic/submission.py`](https://raw.githubusercontent.com/dchen327/stanford-cs221-code/master/logic/submission.py).

## Key Techniques
- Propositional connectives: `Implies`, `Equiv`, `And`, `Or`, `Not`.
- First-order quantifiers and relations: `Forall`, `Exists`, `Atom`, `Constant`.
- Equality-based uniqueness constraints in puzzle encoding.
- Resolution-style reasoning through `createResolutionKB`, `tell`, and `ask`.
- Grammar rules that map surface syntax to logical forms.
- Quantifier order as a meaning-bearing choice, not a formatting detail.

## Implementation Walkthrough
- `formula1a` to `formula1c` are the warm-up. They force you to distinguish implication from equivalence and exclusive-or style encodings.
- `formula2a` to `formula2d` move into first-order logic. These definitions are the course's recurring pattern: quantify over objects, then define one predicate in terms of others.
- `liar()` is the first real inference payoff. Six formulas, one query, and the knowledge base can identify the culprit once the uniqueness constraints are encoded correctly.
- `ints()` is a theorem in disguise. The successor axioms, parity axioms, and transitivity together imply that every number has a larger even number.
- The extra-credit `createRule1` / `createRule2` / `createRule3` functions show the same logic language lifted into semantic parsing. English clause templates become grammar rules that generate formulas.
- The raw code in [`logic/submission.py`](https://raw.githubusercontent.com/dchen327/stanford-cs221-code/master/logic/submission.py) is a good reminder that the proof engine is doing the heavy work; the homework is mostly about not lying to it.
- The written questions at the end pull the same thread into systems thinking: what counts as an explanation, and what tradeoff exists between soundness and completeness.

## What You Learn
- Logical form is precise enough to solve puzzles and prove small theorems.
- Quantifiers are where most mistakes happen.
- Inference only works if the encoding is honest.
- Natural language translation is hard because surface syntax hides scope.
- Explainability matters even when the system is logically clean.
- Soundness and completeness are the right axes for judging proof systems, not "feels smart."
- Once you trust the KB, the line between understanding and deriving gets thin.
