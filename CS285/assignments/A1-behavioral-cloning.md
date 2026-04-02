# CS285 HW1: Behavioral Cloning

## Problem Statement
Behavioral cloning is the first pass at imitation learning in CS285 HW1. The task is simple to state and brittle to execute: fit a policy to expert demonstrations, then see whether that policy can survive outside the exact state distribution it was trained on.

The assignment uses continuous-control MuJoCo tasks and an expert policy that is already trained for you. The starter code asks you to wire up a supervised policy, sample trajectories from the environment, and compare the learned clone against the expert on multiple rollouts. The real lesson is not "can a neural net mimic actions". It is "what happens when the learner starts visiting states the expert never visited".

## Key Techniques
- Supervised imitation from expert-labeled `(observation, action)` pairs.
- A continuous-action MLP policy with a learned Gaussian parameterization.
- Rollout collection and replay-buffer style minibatch sampling.
- Evaluation with multiple trajectories so mean and standard deviation are meaningful.
- TensorBoard video logging for inspecting policy behavior, not just scalar return.
- Covariate shift as the failure mode that makes BC look good on paper and bad in the environment.

## Implementation Walkthrough
1. `scripts/run_hw1.py` is the orchestration layer.
   - On iteration 0 it loads the saved expert dataset from disk.
   - It creates `MLPPolicySL`, the replay buffer, the expert policy wrapper, and the logger.
   - It handles evaluation rollouts, video logging, and checkpoint saving.

2. `infrastructure/utils.py` provides the rollout primitive.
   - `sample_trajectory` resets the environment, queries the policy on the latest observation, steps the env, and records observations, actions, rewards, next observations, and terminals.
   - `sample_trajectories` repeats that until the requested batch size is reached.
   - `compute_metrics` turns a batch of rollouts into train and eval return statistics.

3. `policies/MLP_policy.py` holds the supervised learner.
   - `build_mlp` creates the feedforward trunk.
   - `forward` defines the policy output for a continuous control task.
   - `update` applies the supervised loss and steps Adam over the policy parameters.

4. The replay buffer is the bridge between demonstrations and on-policy data.
   - Expert trajectories seed the buffer in the first iteration.
   - Later iterations reuse the same training path, which is why the same learner code can be shared with DAgger.
   - Minibatches are sampled with randomized indices so the policy does not overfit to rollout order.

5. Logging is part of the assignment, not an afterthought.
   - The code logs average return, return standard deviation, episode length, and videos.
   - The README explicitly recommends keeping `--video_log_freq -1` while debugging so the experiment runs faster.

## What You Learn
- BC is the easiest imitation-learning baseline and the easiest one to break.
- A good training loss does not guarantee a good policy when the policy changes the state distribution.
- The difference between expert-state accuracy and rollout performance is the whole problem.
- Continuous-control imitation is less about classification and more about learning a stable action distribution.
- If you cannot measure rollout statistics cleanly, you cannot tell whether imitation worked.

## Sources
- HW1 README: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw1/README.md
- HW1 handout: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw1/cs285_fall2023_hw1.pdf
- Training loop: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw1/cs285/scripts/run_hw1.py
- Policy module: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw1/cs285/policies/MLP_policy.py
- Rollout utilities: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw1/cs285/infrastructure/utils.py
