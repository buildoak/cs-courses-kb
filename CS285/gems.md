# CS285 Gems

These are the recurring seams, not the lecture summaries. Citations point to the lecture markdown files under `/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/`.

## Top Insights

- The course is a stack of failure modes, not an algorithm zoo. It starts with labels vs reward, then keeps shifting toward variance, geometry, compute allocation, distribution shift, and assumption mismatch. Sources: [course.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/course.md), [L01.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L01.md), [L13.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L13.md), [L23.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L23.md)

- Most "new" methods are wrappers around the same evaluation-improvement loop. Policy iteration sits inside value learning, Q-learning, actor-critic, TRPO, and meta-learning. Sources: [L04.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L04.md), [L05.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L05.md), [L07.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L07.md), [L09.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L09.md), [L11.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L11.md), [L21.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L21.md)

- Model-based vs model-free is mostly a compute-allocation question. The boundary blurs once you backprop through a learned model or use synthetic rollouts to feed a model-free learner. Sources: [L10.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L10.md), [L12.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L12.md), [L13.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L13.md), [L14.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L14.md)

- RL becomes most useful when the signal is weak, subjective, or delayed. That is why the same formalism keeps reappearing in grasping, traffic, preference optimization, transfer, and open problems. Sources: [L01.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L01.md), [L02.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L02.md), [L22.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L22.md), [L23.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L23.md)

## Surprising Connections

- Baselines, target networks, replay buffers, and trust regions are all versions of the same move: keep the target from moving as fast as the estimator. Sources: [L04.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L04.md), [L06.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L06.md), [L18.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L18.md), [L20.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L20.md), [L21.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L21.md)

- Value iteration, fitted Q-learning, policy iteration, and TRPO are the same improvement loop with different estimation and safety layers. Sources: [L05.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L05.md), [L07.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L07.md), [L09.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L09.md), [L11.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L11.md), [L14.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L14.md)

- Model-based RL and meta-RL both use inference as a control primitive: infer a latent model or task, then act as if that latent were known. Sources: [L12.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L12.md), [L22.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L22.md), [L23.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L23.md)

- The "learning + search" thesis from L03 quietly unifies planning, policy gradients, and transfer: learning supplies structure, search spends compute on decisions. Sources: [L03.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L03.md), [L10.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L10.md), [L15.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L15.md), [L22.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L22.md)

- Generative modeling is not separate from RL; RL sits on top as a preference layer that turns broad behavioral priors into task-specific action selection. Sources: [L01.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L01.md), [L02.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L02.md), [L23.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L23.md)

## Under-Appreciated Ideas

- The policy-gradient derivation erases the transition model algebraically. The method is about trajectory sampling, not world modeling. Sources: [L15.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L15.md)

- Causality, not Markovity, is enough for reward-to-go and baselines. That is stronger than the usual folklore version of the argument. Sources: [L16.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L16.md), [L17.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L17.md)

- `V_pi` is compression of `Q_pi`, and Q-learning is not gradient descent on a fixed loss because the target moves. Value learning is compression plus bootstrapping, not just regression. Sources: [L05.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L05.md), [L07.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L07.md), [L11.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L11.md)

- The KL in TRPO and natural gradient measures behavior change, not parameter change. Euclidean step size is the wrong mental model. Sources: [L09.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L09.md), [L20.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L20.md)

- Replay buffers are distribution engineering, not just memory. They decorrelate data and keep old experience useful. Sources: [L06.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L06.md), [L08.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L08.md)

- Deterministic policies are bad explorers. Exploration can fail because the policy becomes too certain, not because the reward is vague. Sources: [L18.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L18.md), [L23.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L23.md)

- Meta-learning in RL is just a hidden-task POMDP in disguise. The whole game is inferring context from experience and then acting on it. Sources: [L22.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L22.md)

## Open Questions

- Can deep RL stay robust without leaning on tabular-style guarantees that disappear under function approximation? Sources: [L13.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L13.md), [L23.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L23.md)

- Where should the prior live: policy, Q-function, model, or feature space? Transfer says that answer is still unsettled. Sources: [L22.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L22.md)

- How do we balance sample efficiency against wall-clock compute when simulation is cheap and parallel? Sources: [L13.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L13.md), [L23.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L23.md)

- How do we keep exploration alive once the policy becomes too confident? Sources: [L18.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L18.md), [L23.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L23.md)

- Can one learning system really cover perception, control, transfer, and meta-learning, or is the general-learning thesis only a useful compression? Sources: [L03.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L03.md), [L22.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L22.md)

- How do we design reward when reward itself is the problem? Sources: [L02.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L02.md), [L23.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L23.md)

- Can model-free, model-based, and synthetic-data methods be unified cleanly, or is compute allocation the only abstraction that survives practice? Sources: [L10.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L10.md), [L12.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L12.md), [L13.md](/Users/otonashi/thinking/building/cs-courses-kb/CS285/lectures/L13.md)
