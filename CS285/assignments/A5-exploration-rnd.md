# CS285 Homework 5: Exploration with RND

## Problem Statement

This homework uses the Pointmass gridworlds to make exploration visible. You first generate baseline datasets with a random policy, then replace blind sampling with random network distillation and inspect the coverage you get.

The graded environments are:
- `PointmassEasy-v0`
- `PointmassMedium-v0`
- `PointmassHard-v0`

The handout later uses those datasets for offline RL, but the article here stays on the exploration half.

## Key Techniques

- A random-policy baseline for state coverage.
- Random Network Distillation as a novelty bonus.
- A frozen target network plus a trainable predictor.
- DQN-style replay and update plumbing around the exploration policy.
- State-coverage scatter plots plus an RND error heatmap for debugging.
- A reward gate in the runner: base env reward can be zeroed before the agent update, so the shaped bonus drives learning.

## Implementation Walkthrough

1. `random_agent_config.py` wraps each Pointmass env in `TimeLimit(..., 100)` and wires the random baseline.
2. `run_hw5_explore.py` is the main loop. It chooses an action, steps the env, stores the transition in a replay buffer, updates the agent, and logs both evaluation returns and coverage plots.
3. The runner uses an exploration schedule when one is present. The hw5 DQN config currently sets a constant epsilon of `0.3`, and `rnd_config.py` reuses that path for RND.
4. `run_hw5_explore.py` zeros the base reward unless `--use_reward` is passed. `RNDAgent.update` then adds `rnd_weight * rnd_error` before calling the base DQN update.
5. `rnd_agent.py` freezes the target network, trains the predictor on the current observations, and exposes `plot_aux` to render the prediction-error heatmap over `[0, 1] x [0, 1]`.
6. `rnd_config.py` swaps in the RND agent, sets the RND network shape, and keeps the DQN-style runner intact.
7. The YAMLs run random and RND variants on easy, medium, and hard with `total_steps: 10000`, which is enough to make the coverage plots readable and the dataset files small.

## What You Learn

- Coverage beats blind sampling only if you can see it.
- Novelty bonuses are simple to state and easy to wire incorrectly.
- Exploration quality is a data problem, not just a policy problem.
- The dataset is the real artifact here, because later training consumes it.

## Sources

- `/Users/otonashi/thinking/tmp/homework_fall2023/hw5/template/hw5.tex:130-273`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw5/cs285/scripts/run_hw5_explore.py:53-184`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw5/cs285/agents/random_agent.py:5-16`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw5/cs285/agents/rnd_agent.py:15-110`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw5/cs285/env_configs/random_agent_config.py:7-24`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw5/cs285/env_configs/rnd_config.py:6-39`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw5/cs285/env_configs/dqn_config.py:13-79`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw5/experiments/exploration/pointmass_easy_random.yaml:1-5`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw5/experiments/exploration/pointmass_easy_rnd.yaml:1-6`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw5/experiments/exploration/pointmass_medium_random.yaml:1-5`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw5/experiments/exploration/pointmass_medium_rnd.yaml:1-6`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw5/experiments/exploration/pointmass_hard_random.yaml:1-5`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw5/experiments/exploration/pointmass_hard_rnd.yaml:1-6`
