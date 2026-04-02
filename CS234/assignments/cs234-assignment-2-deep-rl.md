# CS234 Assignment 2: Deep Q-Learning on Atari

## Problem Statement

This assignment moves from tabular RL into function approximation and then into full deep RL. The work is staged:

1. Implement linear exploration and learning-rate schedules on a small `TestEnv`.
2. Build a linear Q-function approximator in TensorFlow and verify the training pipeline on the toy environment.
3. Replace the linear model with the DeepMind DQN architecture.
4. Train on `Pong-v0`, compare linear approximation against the convolutional model, and reason about replay, target networks, and state stacking.

The assignment handout is explicit about the operational burden: training the Atari agent takes roughly 12 hours on GPU and the default config expects at least 5 million environment steps.

## Key Techniques

- **Epsilon-greedy exploration:** linearly decay exploration from fully random to mostly greedy behavior.
- **Learning-rate schedules:** coordinate optimization with the exploration schedule.
- **Linear Q approximation:** flatten the stacked frames and learn one affine map to all action values.
- **Experience replay:** sample decorrelated transitions from a replay buffer rather than training on adjacent frames.
- **Target network:** hold a lagged copy of the Q-network fixed during bootstrap updates.
- **Nature DQN architecture:** three convolutional layers, one hidden fully connected layer, and one output head over discrete actions.
- **Atari preprocessing:** max-pool over adjacent frames, grayscale, downsample to `80x80`, stack 4 frames, repeat each action for 4 emulator steps.

## Implementation Walkthrough

The starter code is modular. The easiest way to read it is in dependency order:

1. **Schedules in `q1_schedule.py`**
   - `LinearSchedule.update` decays epsilon linearly until the floor.
   - `LinearExploration.get_action` flips between a random action and the current greedy action.

2. **Shared DQN skeleton in `core/deep_q_learning.py`**
   - `build()` wires placeholders, online Q-network, target Q-network, target sync op, loss, and optimizer.
   - `process_state()` casts `uint8` image tensors to `float32` and rescales by `255`.
   - `update_step()` samples minibatches from replay memory and runs one gradient step.

3. **Training loop in `core/q_learning.py`**
   - Store frames into replay memory.
   - Build 4-frame observations.
   - Choose an action with exploration.
   - Store the transition.
   - After warmup, sample minibatches and train every few steps.
   - Periodically sync the target network, evaluate, save checkpoints, and record videos.

4. **Replay memory in `utils/replay_buffer.py`**
   - Store raw frames once instead of duplicating them per observation.
   - Reconstruct the last `frame_history_len` frames on demand.
   - Zero-pad early episode context and cut off context across terminal boundaries.

5. **Linear baseline in `q2_linear.py`**
   - Define placeholders for states, actions, rewards, next states, done mask, and learning rate.
   - Flatten the stacked state and produce one score per action with a fully connected layer.
   - Form the DQN target:
     - `r` if the transition is terminal;
     - `r + gamma * max_a' Q_target(s', a')` otherwise.
   - Optimize squared TD error with Adam.

6. **DeepMind network in `q3_nature.py`**
   - Replace the linear head with:
     - `32` filters, `8x8`, stride `4`
     - `64` filters, `4x4`, stride `2`
     - `64` filters, `3x3`, stride `1`
     - flatten
     - fully connected `512`
     - output layer over actions

7. **Full Pong training**
   - `q4_train_atari_linear.py` runs the weak baseline on Pong.
   - `q5_train_atari_nature.py` runs the convolutional DQN.
   - The provided config uses `5,000,000` training steps, replay buffer size `500,000`, target sync every `10,000` steps, batch size `32`, and frame history `4`.

This assignment is where "deep RL" stops being a phrase and becomes a systems problem: data pipeline, schedules, replay, target sync, network design, and long-running training all matter.

## What You Learn

- Why naive online Q-learning becomes unstable once you add nonlinear function approximation.
- How replay and target networks stabilize bootstrap targets in practice.
- Why stacked frames are needed when the current image does not encode motion.
- Why a linear baseline fails on raw-pixel Atari even when the rest of the training loop is correct.
- How RL experiments become infrastructure problems once the environment, buffer, network, and training horizon get large.

## Sources

- GitHub search used to locate candidate repos: https://github.com/search?q=cs234+stanford&type=repositories
- Primary repo: https://github.com/arowdy98/Stanford-CS234
- Assignment handout: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment2/assignment2.pdf
- Schedule code: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment2/starter_code/q1_schedule.py
- Linear model: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment2/starter_code/q2_linear.py
- Nature DQN model: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment2/starter_code/q3_nature.py
- DQN scaffold: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment2/starter_code/core/deep_q_learning.py
- Training loop: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment2/starter_code/core/q_learning.py
- Replay buffer: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment2/starter_code/utils/replay_buffer.py
- Pong training config: https://github.com/arowdy98/Stanford-CS234/blob/master/assignment2/starter_code/configs/q5_train_atari_nature.py
