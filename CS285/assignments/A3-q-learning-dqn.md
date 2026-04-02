# CS285 Homework 3: Deep Q-Learning

## Problem Statement

This homework asks you to make DQN actually work, first on low-dimensional control and then on Atari pixels. The assignment is not just "implement Bellman backup." It is a full loop problem: replay, epsilon scheduling, target lag, truncation handling, and pixel preprocessing all have to line up.

The handout asks for:
- a basic DQN implementation
- a double-Q variant
- sanity checks on CartPole and LunarLander
- an Atari run on MsPacman
- a hyperparameter sweep to show how brittle Q-learning is

## Key Techniques

- Epsilon-greedy exploration with a scheduled epsilon.
- Hard target-network updates.
- Double Q-learning to reduce overestimation.
- Replay buffers that treat time-limit truncation differently from true terminal failure.
- Frame skip, frame stack, and grayscale preprocessing for Atari.
- MLP critics for state vectors and a CNN critic for 4x84x84 Atari input.
- Gradient clipping and learning-rate scheduling to keep training from blowing up.

## Implementation Walkthrough

1. `run_hw3_dqn.py` owns the training loop: reset the env, pick an action, step the env, store the transition, then start sampling minibatches after `learning_starts`.
2. The replay path splits by observation shape. Low-dimensional states use `ReplayBuffer`; stacked Atari frames use `MemoryEfficientReplayBuffer`, with `on_reset` handling the first frame of each episode.
3. The runner stores `done and not truncated`, which keeps time-limit cutoffs out of TD targets while still resetting the environment.
4. `DQNAgent.get_action` implements epsilon-greedy action selection.
5. `update_critic` computes the bootstrap target from the target critic, uses the online critic for action selection when `use_double_q` is on, gathers `Q(s,a)` for the taken action, clips gradients, and steps the LR scheduler.
6. `dqn_basic_config.py` wires the MLP critic, the piecewise epsilon schedule, and the low-dimensional control defaults.
7. `dqn_atari_config.py` swaps in the Atari CNN, the Atari epsilon schedule, `Adam(eps=1e-4)`, and gradient clipping for the pixel setting.

## What You Learn

- DQN is a systems problem disguised as an equation.
- The target network and double-Q split are what make the update stable enough to study.
- Atari depends on the input pipeline almost as much as the network.
- Small hyperparameter changes can move Q-learning from stable to explosive.

## Sources

- `/Users/otonashi/thinking/tmp/homework_fall2023/hw3/template/hw3_text/02_dqn.tex:4-110`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw3/cs285/agents/dqn_agent.py:11-118`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw3/cs285/scripts/run_hw3_dqn.py:26-204`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw3/cs285/env_configs/dqn_basic_config.py:17-87`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw3/cs285/env_configs/dqn_atari_config.py:28-126`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw3/experiments/dqn/cartpole.yaml:1-4`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw3/experiments/dqn/lunarlander.yaml:1-4`
- `/Users/otonashi/thinking/tmp/homework_fall2023/hw3/experiments/dqn/mspacman.yaml:1-5`
