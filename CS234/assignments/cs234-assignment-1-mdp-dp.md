# CS234 Assignment 1: MDPs and Dynamic Programming

## Problem Statement

This assignment is the planning-heavy entry point into Stanford CS234. It mixes theory and implementation across four blocks:

1. Analyze how reward-model and transition-model errors change policy evaluation in a finite-horizon MDP.
2. Prove properties of value iteration and quantify the suboptimality of a greedy policy extracted from an approximate value function.
3. Reason about optimal policies in a gridworld with terminal reward and penalty states.
4. Implement policy iteration and value iteration for Frozen Lake, then compare deterministic and stochastic dynamics.

The coding section is deliberately small in file count and large in conceptual payoff: one file, `vi_and_pi.py`, forces you to translate Bellman-style equations into actual loops over states, actions, and transition tuples.

## Key Techniques

- **Finite-horizon policy evaluation:** work with time-indexed value functions and separate the effect of reward error from transition-model error.
- **Bellman expectation and optimality backups:** evaluate one policy exactly enough, then improve it greedily.
- **Policy iteration:** alternate full policy evaluation with greedy improvement until the policy stabilizes.
- **Value iteration:** apply the optimality backup directly, then extract a greedy policy from the converged value function.
- **Environment reasoning under stochasticity:** compare how deterministic and stochastic Frozen Lake dynamics change convergence speed and behavior.

## Implementation Walkthrough

Start in `assignment1/vi_and_pi.py`. The core implementation path is short:

1. **`policy_evaluation`**
   - Initialize `V` to zeros.
   - For each state, look up the action prescribed by the current policy.
   - Sum over `P[state][action]`, using `probability * (reward + gamma * V[nextstate])`.
   - Repeat until the maximum coordinate change is below tolerance.

2. **`policy_improvement`**
   - For every state, compute a one-step lookahead value for every action.
   - Reuse the same transition structure `P[state][action]`.
   - Select the action with the largest backup value. That gives the greedy policy under the current value estimate.

3. **`policy_iteration`**
   - Initialize a simple policy.
   - Alternate:
     - evaluate the current policy;
     - improve the policy greedily.
   - Stop when value updates are below tolerance and return both the value function and the improved policy.

4. **`value_iteration`**
   - For each state, compute the Bellman optimality backup by taking the maximum action value.
   - Iterate until convergence.
   - Run one final greedy improvement step to recover the policy from the converged value function.

5. **Run on both Frozen Lake variants**
   - The handout asks for `Deterministic-4x4-FrozenLake-v0` and `Stochastic-4x4-FrozenLake-v0`.
   - Deterministic dynamics usually make the optimal route and convergence pattern easier to interpret.
   - Stochastic dynamics force you to think in expected value, not shortest path alone.

The point of the assignment is not just "make DP work." It is to make the relationship between equations and code feel mechanical. Once you can map Bellman updates to loops, larger RL algorithms become much easier to read.

## What You Learn

- The difference between **planning with a model** and **learning from sampled experience**.
- Why policy iteration and value iteration are different computational tradeoffs over the same Bellman structure.
- How stochastic transitions can change the optimal policy even when the map stays the same.
- How to debug RL code by inspecting backups, convergence criteria, and greedy policy extraction instead of guessing.

## Sources

- GitHub search used to locate candidate repos: https://github.com/search?q=cs234+stanford&type=repositories
- Primary repo: https://github.com/arowdy98/Stanford-CS234
- Assignment handout: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment1/assignment1.pdf
- Starter/solution code: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment1/vi_and_pi.py
