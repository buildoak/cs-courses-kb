# CS234 Gems

These are the recurring seams, not the lecture summaries. Citations point to the lecture markdown files under `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/`.

## Top Insights

- **RL is a feedback-conversion stack.** The course keeps turning weak signals into sharper targets: returns become values, values become advantages, demonstrations become reward models, preferences become implicit rewards, and search traces become training data. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L03.md:12-20`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L07.md:12-22`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L08.md:12-22`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L09.md:12-22`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L14.md:18-23`.

- **The real bottleneck is support, not just optimization.** Offline RL, DPO, reward hacking, and value alignment all break when the policy or target steps outside the data and feedback support. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L09.md:24-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L10.md:24-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L16.md:29-35`.

- **Stability is mostly local trust.** Replay buffers, target networks, baselines, KL bounds, pessimism, and reference models are all guardrails that keep bootstrapping, preference optimization, and offline learning close to what the data can justify. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L04.md:23-28`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L06.md:22-27`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L07.md:24-29`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L09.md:24-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L10.md:24-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L16.md:29-35`.

- **The course separates the target from the estimator.** Policy/value, reward/policy, preference/best interest, and morality/common-sense morality are distinct questions; the algorithms are mostly different estimators layered onto those targets. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L06.md:22-27`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L08.md:24-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L15.md:24-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L16.md:29-35`.

- **Search is a compute-allocation strategy.** UCB, Thompson sampling, PSRL, MBIE-EB, and MCTS all spend more compute where uncertainty is highest, not where the answer is already known. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L11.md:25-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L12.md:26-31`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L13.md:25-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L14.md:24-29`.

## Surprising Connections

- **Bellman backups and MCTS are the same shape at different scales.** Dynamic programming sweeps over the full state space; MCTS does the same backup logic on a rooted tree and turns each node into a bandit. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L02.md:16-20`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L11.md:20-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L14.md:14-23`.

- **UCB and Thompson sampling are sibling responses to uncertainty.** One turns uncertainty into a deterministic bonus; the other samples from a posterior. Their MDP versions are MBIE-EB and PSRL. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L11.md:20-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L12.md:16-31`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L13.md:13-20`.

- **GAE and DPO both remove an intractable middleman.** GAE compresses long-horizon credit assignment into telescoping TD deltas; DPO collapses reward modeling by canceling the partition function inside the preference loss. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L07.md:12-17`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L09.md:16-18`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L09.md:24-30`.

- **Offline RL pessimism, RLHF reference models, and alignment safety regions are the same defense against extrapolation.** Each keeps the policy close to what the data, simulator, or human feedback can actually support. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L10.md:21-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L08.md:19-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L16.md:19-35`.

- **Imitation learning, RLHF, and moral alignment are all answers to "whose signal counts?"** Action traces, preferences, and moral theory each become candidate targets once plain reward is no longer enough. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L07.md:19-29`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L08.md:6-10`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L15.md:6-10`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L16.md:7-10`.

- **AlphaZero and RLHF both turn generated behavior into fresh supervision.** Search traces train the Go network; preference data trains the reward model and policy. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L14.md:21-23`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L08.md:9-10`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L16.md:8-10`.

## Under-Appreciated Ideas

- **State is a modeling choice, not a fact.** Atari needs four frames because one frame hides velocity; Bayesian bandits and POMDPs show the hidden state can be beliefs or parameters. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L01.md:16-17`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L12.md:21-24`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L13.md:16-18`.

- **Finite-horizon optimal policies are not automatically stationary.** The planning lecture explicitly allows time-indexed policies once the horizon is finite. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L02.md:21-29`.

- **Batch methods can converge to different answers on the same dataset.** Monte Carlo and TD are not just different speeds; on finite data they can land on different fixed points. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L03.md:24-29`.

- **Bayesian and frequentist regret are different questions.** The bandit lecture refuses to treat them as interchangeable metrics. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L12.md:8-10`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L12.md:26-31`.

- **The replay buffer may matter more than the deep net.** DQN's stabilization story is mostly replay plus target networks, not just representation power. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L04.md:24-28`.

- **Preference collection is the exploration budget in RLHF and DPO.** The hard part is choosing which comparisons to ask for, not just optimizing the objective afterward. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L08.md:24-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L09.md:21-30`.

- **Safety is objective design, not just filtering.** The alignment lectures treat autonomy, paternalism, trusted regions, and reward penalties as part of the objective itself. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L15.md:9-10`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L16.md:19-35`.

## Open Questions

- **What is the right deep exploration primitive at scale?** Counts work in tabular settings, but pseudo-counts, priors, and meta-learning all feel like partial patches for large state spaces. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L13.md:21-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L13.md:47-55`.

- **When does DPO replace RLHF, and when does missing explicit reward modeling become a liability?** The lecture says the implicit reward looks competitive, but reward hacking and verbosity drift still show up. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L09.md:25-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L09.md:40-53`.

- **How do you detect confounding and support failure in offline logs?** The support problem and hidden-state bias are easy to state and hard to diagnose in practice. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L10.md:24-30`.

- **Which alignment target should dominate when preferences, best interests, and morality diverge?** The course never resolves this cleanly, because the target itself is normative. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L15.md:6-10`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L15.md:24-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L16.md:7-10`.

- **Can human feedback scale without becoming the bottleneck?** DAGGER, preference labeling, and RLHF all need people in the loop; the course repeatedly exposes human effort as the limiting resource. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L07.md:27-29`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L08.md:9-10`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L16.md:24-26`.

- **Can robustness be had without paying the full pessimism tax?** Pessimism fixes extrapolation, but it can also suppress useful exploration and policy improvement. Sources: `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L10.md:21-30`, `/Users/otonashi/thinking/building/cs-courses-kb/CS234/lectures/L16.md:29-35`.
