# CS285 HW2: Policy Gradients and REINFORCE

## Problem Statement
HW2 moves from imitation to direct reinforcement learning. The policy is no longer trying to copy an expert action-by-action. It is trying to maximize expected return by nudging the policy parameters toward trajectories that paid off.

The starter code is built around REINFORCE-style policy gradients, then layers on the usual variance-reduction machinery: reward-to-go, a learned value baseline, advantage normalization, and optional GAE. The assignment is useful because it shows how much of policy-gradient "performance" is actually variance management.

## Key Techniques
- The policy gradient objective `E[log pi(a|s) * advantage]`.
- Monte Carlo return estimates for on-policy trajectories.
- Reward-to-go instead of full-trajectory return when you want lower variance.
- A learned value critic as a baseline.
- Advantage normalization to stabilize updates.
- Generalized Advantage Estimation as the recursive middle ground between Monte Carlo and bootstrapping.
- Separate policy parameterizations for discrete and continuous action spaces.

## Implementation Walkthrough
1. `scripts/run_hw2.py` is the outer loop.
   - It creates `PGAgent`, the logger, and the environment.
   - It samples `batch_size` transitions with `utils.sample_trajectories`.
   - It converts the trajectory list into the dictionary-of-lists shape that the agent update code expects.

2. `infrastructure/utils.py` handles rollouts.
   - `sample_trajectory` resets the env, asks the policy for an action, steps the env, and stores the resulting transition data.
   - `sample_trajectories` keeps collecting until the batch is large enough for a gradient step.
   - `compute_metrics` computes the eval/train summary statistics used in the logs.

3. `agents/pg_agent.py` is where the algorithm becomes concrete.
   - `_calculate_q_vals` chooses between full-trajectory returns and reward-to-go.
   - `_estimate_advantage` optionally subtracts a learned baseline and can compute GAE if `gae_lambda` is set.
   - The update path flattens trajectories into batch arrays so the actor and critic can train vectorized.

4. `networks/policies.py` implements the policy network.
   - `get_action` converts a NumPy observation into a sampled action.
   - `forward` branches on discrete versus continuous action spaces.
   - `MLPPolicyPG.update` applies the policy-gradient loss using the advantages from the agent.

5. `networks/critics.py` implements the baseline.
   - `forward` predicts a state value.
   - `update` regresses the critic toward the Monte Carlo Q targets.
   - The baseline is not the main algorithm. It is the variance-reduction layer that makes the main algorithm behave.

6. The assignment's experiments are really a sequence of ablations.
   - Compare trajectory-centric returns vs reward-to-go.
   - Compare no baseline vs baseline.
   - Try different batch sizes and advantage-normalization settings.
   - The point is to see which knobs reduce variance without hiding the actual learning signal.

## What You Learn
- REINFORCE is conceptually clean and statistically noisy.
- Reward-to-go is an easy win because it throws away irrelevant early reward terms.
- A critic baseline is not about changing the optimum. It is about making the gradient less chaotic.
- Advantage normalization is one of those tiny engineering tricks that changes training behavior more than the algorithm name does.
- Policy gradients are flexible across discrete and continuous control, but sample efficiency is still the tax you pay.

## Sources
- HW2 README: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw2/README.md
- Official course PDF: https://rail.eecs.berkeley.edu/deeprlcourse/deeprlcourse/static/homeworks/hw2.pdf
- Training loop: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw2/cs285/scripts/run_hw2.py
- Agent logic: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw2/cs285/agents/pg_agent.py
- Policy network: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw2/cs285/networks/policies.py
- Critic network: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw2/cs285/networks/critics.py
- Rollout utilities: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw2/cs285/infrastructure/utils.py
