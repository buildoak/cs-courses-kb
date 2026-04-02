# CS285 HW1: DAgger

## Problem Statement
DAgger fixes the part of behavioral cloning that fails first: the learner does not stay on the expert's state distribution.

Instead of cloning only from expert demonstrations, DAgger alternates between collecting trajectories from the current learner and relabeling those learner-visited states with the expert's actions. The assignment asks you to use the same codebase as BC, then show that the policy improves over iterations because the training data now includes the states the learner actually drifts into.

## Key Techniques
- Dataset aggregation over learner-generated states.
- Expert relabeling of the learner's rollout observations.
- On-policy trajectory collection before each retraining round.
- Reusing the same supervised policy and optimizer from BC.
- Iterative comparison against the expert and the original BC baseline.
- A warm-start + refinement loop instead of a single fit.

## Implementation Walkthrough
1. `scripts/run_hw1.py` switches from BC to DAgger when `--do_dagger` is set.
   - The code asserts `n_iter > 1`, because DAgger needs at least one warm-start iteration plus at least one relabel-and-refit round.
   - Iteration 0 still uses the expert dataset to bootstrap the policy.

2. Each later iteration gathers learner-state coverage.
   - `utils.sample_trajectories` collects `batch_size` transitions by rolling out the current policy.
   - That means the policy is no longer being trained only on states the expert happened to visit.

3. The key DAgger step is relabeling.
   - For every sampled path, the code queries `expert_policy.get_action` on `paths[i]["observation"]`.
   - Those expert labels replace the learner's own actions in the training batch.
   - The replay buffer then stores the relabeled trajectories, not the raw learner actions.

4. Training stays supervised.
   - The actor still trains through `MLPPolicySL.update`.
   - Minibatches are sampled from the replay buffer with shuffled indices.
   - The only real change is the data distribution, which is exactly the point of DAgger.

5. Evaluation shows whether the relabeling loop is doing anything useful.
   - The assignment logs mean and standard deviation of eval returns.
   - The video rollouts let you see whether the policy has actually stopped drifting off-distribution.

## What You Learn
- The dangerous part of imitation learning is not fitting the expert. It is staying near the expert's state support after deployment.
- DAgger is a data-collection algorithm disguised as a learning algorithm.
- Relabeling learner states with expert actions is the core trick that BC does not have.
- The same supervised optimizer can work much better once the dataset starts matching the learner's behavior.
- Iteration matters because the policy changes the data it will later be trained on.

## Sources
- HW1 README: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw1/README.md
- HW1 handout: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw1/cs285_fall2023_hw1.pdf
- Training loop: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw1/cs285/scripts/run_hw1.py
- Policy module: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw1/cs285/policies/MLP_policy.py
- Rollout utilities: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw1/cs285/infrastructure/utils.py
- Expert policy wrapper: https://raw.githubusercontent.com/berkeleydeeprlcourse/homework_fall2023/main/hw1/cs285/policies/loaded_gaussian_policy.py
