# CS285 Homework 4: Model-Based RL

## Problem Statement

This homework turns learning into planning. You fit a dynamics model from rollouts, then use that model inside MPC to choose actions in continuous-control environments.

The graded environments are:
- `reacher-cs285-v0`
- `cheetah-cs285-v0`
- `obstacles-cs285-v0`

The codebase also supports an optional SAC branch, which turns the learned model into short synthetic rollouts for MBPO.

## Key Techniques

- An ensemble of MLP dynamics models.
- Normalizing both model inputs and model targets.
- Predicting observation deltas instead of raw next observations.
- MPC with either random shooting or CEM.
- Scoring action sequences with `env.get_reward`.
- Optional MBPO rollouts into a SAC replay buffer.

## Implementation Walkthrough

1. `run_hw4.py` initializes the environment, the model-based agent, and the replay buffer. If `sac_config` is supplied, it also initializes SAC and a separate replay buffer for MBPO.
2. Iteration 0 collects `initial_batch_size` transitions with a random policy. Later iterations collect `batch_size` transitions with the current actor.
3. After data collection, `update_statistics` tracks the mean and standard deviation of observation-action inputs and observation deltas.
4. `ModelBasedAgent.update` trains one ensemble member at a time on a different minibatch. The TODO comments explicitly call out normalization and delta prediction as required pieces.
5. `evaluate_action_sequences` tiles the current observation across the ensemble and across candidate sequences, rolls forward for `mpc_horizon` steps, and uses `env.get_reward` to score imagined trajectories.
6. `get_action` either does random shooting or CEM. The CEM path refits elite mean and standard deviation over repeated samples.
7. If MBPO is enabled, `run_hw4.py` samples model rollouts of length `mbpo_rollout_length` and inserts them into the SAC buffer before SAC updates.

## What You Learn

- Model error compounds over planning horizon.
- Normalization is not a side detail; it is the difference between trainable and brittle dynamics models.
- MPC is search over action sequences, not magic.
- MBPO is the clean handoff point between model-based learning and policy learning.

## Sources

- `/Users/otonashi/thinking/tmp/homework_fall2023/hw4/cs285/agents/model_based_agent.py:9-223`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw4/cs285/scripts/run_hw4.py:34-286`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw4/cs285/env_configs/mpc_config.py:9-80`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw4/cs285/env_configs/sac_config.py:18-116`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw4/experiments/mpc/reacher_multi_iter.yaml:1-14`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw4/experiments/mpc/halfcheetah_cem.yaml:1-17`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw4/experiments/mpc/halfcheetah_mbpo.yaml:1-12`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw4/experiments/mpc/obstacles_multi_iter.yaml:1-14`
