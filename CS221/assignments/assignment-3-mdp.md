# CS221 Assignment 3: Markov Decision Processes

## Problem Statement
The MDP assignment is where the course stops asking "what is the next move?" and starts asking "what is the value of a policy under uncertainty?" The Autumn 2025 lecture repo gives the recurrence story in `mdp.py`, and the Blackjack homework code in [`blackjack/submission.py`](https://github.com/dchen327/stanford-cs221-code/blob/master/blackjack/submission.py) shows the assignment shape: build MDP instances, solve them with value iteration, then compare that policy with Q-learning.

The state space here is concrete, not abstract. The file uses `BlackjackMDP(cardValues=[...], multiplicity=..., threshold=..., peekCost=...)` test cases and a state tuple like `(total, nextCard, counts)`.

## Key Techniques
- Bellman-style recurrence for policy evaluation and value iteration.
- Finite-state MDP modeling with explicit actions and transitions.
- Value iteration as the exact planner when the model is known.
- Q-learning as the model-free counterpoint.
- Feature extraction for generalizing across states and actions.

## Implementation Walkthrough
- Read the recurrence first. The lecture repo states the policy-evaluation equation and the value-iteration equation side by side in [`mdp.py`](https://github.com/stanford-cs221/autumn2025-lectures/blob/main/mdp.py).
- Use the given `BlackjackMDP` instances as the test harness. The submission file sets up small and large MDPs, then runs `ValueIteration().solve(mdp)`.
- Compare the planner with `QLearningAlgorithm(mdp.actions, mdp.discount(), featureExtractor)`, then simulate enough trials to see whether the learned policy converges.
- Build features that actually encode the state: current total, card presence, and remaining counts. The homework code spells those out in `blackjackFeatureExtractor`.
- The real lesson is that feature design and environment design are coupled. If the MDP changes, the learned policy can drift fast.

## What You Learn
- An MDP is not a buzzword; it is a precise state-action-reward machine.
- Value iteration gives you certainty when the model is known.
- Q-learning gives you adaptability when the model is only sampled.
- Good state features are the difference between generalization and table lookup.
- The assignment makes the bridge from planning to reinforcement learning visible.
