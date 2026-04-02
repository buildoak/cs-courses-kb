# 6.S191 Lab 3: Reinforcement Learning in Cartpole and VISTA

## Problem Statement
Lab 3 leaves supervised learning behind. The agent does not get labels. It acts in an environment, receives rewards, and has to improve from that feedback alone.

The lab has two control problems. First: Cartpole, where the action space is tiny and the dynamics are clean. Second: VISTA driving, where the agent has to learn from RGB camera input and output a continuous control command. Same learning loop. Much harder world.

## Key Techniques
- **Policy gradients.** The agent updates its policy toward trajectories that produced higher return.
- **Memory buffers.** Observations, actions, and rewards are stored per episode so the agent can learn after the episode ends.
- **Discounted returns.** Later rewards are downweighted so the agent prefers actions that pay off sooner.
- **Reward normalization.** Returns are normalized to reduce update noise.
- **Discrete and continuous policies.** Cartpole uses a simple discrete action policy; VISTA uses a continuous distribution over steering curvature.
- **Environment-specific preprocessing.** The driving task adds image preprocessing and trajectory handling on top of the core RL loop.

## Implementation Walkthrough
1. Build the Cartpole environment with OpenAI Gym and inspect the observation and action spaces.
2. Define a small feed-forward policy network that outputs left/right action probabilities.
3. Add a memory buffer and implement discounted return computation.
4. Train Cartpole with a REINFORCE-style loss that scales log probabilities by normalized returns.
5. Switch to VISTA, where the world is built from a driving trace and the agent controls speed plus curvature.
6. Inspect trajectories, define crash conditions, and preprocess the camera stream into inputs the policy can use.
7. Replace the discrete action head with a continuous policy loss for steering.
8. Train the self-driving policy and evaluate it by how far it travels before crashing.

## What You Learn
- RL is not a label-matching problem. It is a delayed-credit problem.
- The reward function is part of the model, not an afterthought.
- Cartpole is the easy case. Continuous control from pixels is where the engineering tax shows up.
- Policy gradients are simple to state and annoying to stabilize, which is why the memory, discounting, and reward design all matter.

## Sources
- `https://github.com/MITDeepLearning/introtodeeplearning/blob/master/README.md`
- `https://github.com/MITDeepLearning/introtodeeplearning/blob/master/xtra_labs/rl_pong/RL.ipynb`
- `https://github.com/MITDeepLearning/introtodeeplearning/blob/master/xtra_labs/rl_selfdriving/RL.ipynb`
- `/tmp/introtodeeplearning/README.md`
- `/tmp/introtodeeplearning/xtra_labs/rl_pong/RL.ipynb`
- `/tmp/introtodeeplearning/xtra_labs/rl_selfdriving/RL.ipynb`
