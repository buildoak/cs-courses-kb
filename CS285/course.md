# CS285: Deep Reinforcement Learning, Decision Making, and Control

UC Berkeley's public deep-RL archive. This KB page uses the Fall 2023 course site as the source of truth and the supplied Berkeley playlist as the video spine. The class is about decision making under uncertainty, not RL as a toy game-playing trick.

## Quick Facts

- **Course:** CS285: Deep Reinforcement Learning, Decision Making, and Control
- **School:** UC Berkeley
- **Offering used for this KB page:** Fall 2023 archive
- **Instructor:** Sergey Levine
- **Lectures:** Mon/Wed 5:00-6:30 p.m., Wheeler 212
- **Prerequisite:** CS189/289A or equivalent
- **Public video source:** [CS285 playlist](https://www.youtube.com/playlist?list=PL_iWQOsE6TfVYGEGiAOMaOzzv41Jfm_Ps)
- **Official course page:** [CS285 course catalog entry](https://www2.eecs.berkeley.edu/Courses/CS285/)
- **Official archive:** [CS285 Fall 2023](https://rail.eecs.berkeley.edu/deeprlcourse-fa23/)
- **Public archive note:** the playlist breaks the 23 lectures into part videos

## Overview

CS285 is the cleanest public Berkeley map of modern reinforcement learning. It starts with imitation learning, then moves through policy gradients, actor-critic methods, value functions, Q-learning, model-based RL, exploration, offline RL, RL theory, variational inference, control as inference, inverse RL, sequence models, transfer learning, and open problems.

The important thing is the shape of the arc. The course does not treat deep RL as one algorithm family. It treats it as a stack of connected failure modes: distribution shift, exploration, sample efficiency, planning, offline data, and reward design.

## Syllabus

### Core Arc

- Lectures 1-4: course overview, imitation learning, PyTorch, and RL basics.
- Lectures 5-8: policy gradients, actor-critic, value functions, and Q-learning.
- Lectures 9-12: advanced policy gradients, optimal control and planning, model-based RL, and model-based policy learning.
- Lectures 13-16: exploration and offline reinforcement learning.
- Lectures 17-20: RL theory, variational inference, control as inference, and inverse RL.
- Lectures 21-23: sequence models, transfer/meta-learning, and open problems.

### Lecture Map

The public playlist uses many part videos, but the Berkeley calendar gives the canonical lecture labels:

| # | Lecture |
|---|---|
| 1 | Introduction and Course Overview |
| 2 | Supervised Learning of Behaviors |
| 3 | PyTorch Tutorial |
| 4 | Introduction to Reinforcement Learning |
| 5 | Policy Gradients |
| 6 | Actor-Critic Algorithms |
| 7 | Value Function Methods |
| 8 | Deep RL with Q-Functions |
| 9 | Advanced Policy Gradients |
| 10 | Optimal Control and Planning |
| 11 | Model-Based Reinforcement Learning |
| 12 | Model-Based Policy Learning |
| 13 | Exploration (Part 1) |
| 14 | Exploration (Part 2) |
| 15 | Offline Reinforcement Learning (Part 1) |
| 16 | Offline Reinforcement Learning (Part 2) |
| 17 | Reinforcement Learning Theory Basics |
| 18 | Variational Inference and Generative Models |
| 19 | Connection between Inference and Control |
| 20 | Inverse Reinforcement Learning |
| 21 | RL with Sequence Models |
| 22 | Meta-Learning and Transfer Learning |
| 23 | Challenges and Open Problems |

### Assignments

- **Homework 1:** imitation learning, split in this KB into [behavioral cloning](./assignments/A1-behavioral-cloning.md) and [DAgger](./assignments/A1-dagger.md).
- **Homework 2:** [policy gradients and REINFORCE](./assignments/A2-policy-gradients-reinforce.md).
- **Homework 3:** [deep Q-learning](./assignments/A3-q-learning-dqn.md).
- **Homework 4:** [model-based RL](./assignments/A4-model-based-rl.md).
- **Homework 5:** [exploration with RND](./assignments/A5-exploration-rnd.md).

The official site also frames the course as a research-style sequence with a final project, not just homework.

## Instructor Bio

### Sergey Levine

Professor at UC Berkeley EECS. His Berkeley bio says he received his BS/MS and PhD in Computer Science from Stanford, joined Berkeley in 2016, and focuses on machine learning for decision making and control, especially deep learning and reinforcement learning algorithms. His applications include autonomous robots and vehicles, computer vision, and graphics.

## Course Staff

- Kyle Stachowicz, head GSI
- Vivek Myers, GSI
- Joey Hong, GSI
- Kevin Black, GSI

## Links

- [Berkeley course catalog entry](https://www2.eecs.berkeley.edu/Courses/CS285/)
- [Fall 2023 archive](https://rail.eecs.berkeley.edu/deeprlcourse-fa23/)
- [Calendar](https://rail.eecs.berkeley.edu/deeprlcourse-fa23/calendar/)
- [Syllabus](https://rail.eecs.berkeley.edu/deeprlcourse-fa23/syllabus/)
- [Staff](https://rail.eecs.berkeley.edu/deeprlcourse-fa23/staff/)
- [Resources](https://rail.eecs.berkeley.edu/deeprlcourse-fa23/resources/)
- [Public playlist](https://www.youtube.com/playlist?list=PL_iWQOsE6TfVYGEGiAOMaOzzv41Jfm_Ps)
- [HW1: Behavioral Cloning](./assignments/A1-behavioral-cloning.md)
- [HW1: DAgger](./assignments/A1-dagger.md)
- [HW2: Policy Gradients and REINFORCE](./assignments/A2-policy-gradients-reinforce.md)
- [HW3: Deep Q-Learning](./assignments/A3-q-learning-dqn.md)
- [HW4: Model-Based RL](./assignments/A4-model-based-rl.md)
- [HW5: Exploration with RND](./assignments/A5-exploration-rnd.md)

## Why It Matters

Inference from the syllabus arc:

- It is the practical bridge from imitation to RL.
- It covers the failure modes that matter in real systems: distribution shift, exploration, sample efficiency, and offline data.
- It connects the main RL families in one course: policy gradients, actor-critic, Q-learning, model-based control, and transfer.
- It reaches the research edge instead of stopping at introductory algorithms.
- The final project structure makes it useful as a research launchpad, not just a homework sequence.

## Sources

- Berkeley CS285 course catalog entry: https://www2.eecs.berkeley.edu/Courses/CS285/
- Berkeley CS285 Fall 2023 archive: https://rail.eecs.berkeley.edu/deeprlcourse-fa23/
- Berkeley CS285 calendar page: https://rail.eecs.berkeley.edu/deeprlcourse-fa23/calendar/
- Berkeley CS285 staff page: https://rail.eecs.berkeley.edu/deeprlcourse-fa23/staff/
- Sergey Levine Berkeley bio: https://www2.eecs.berkeley.edu/Faculty/Homepages/svlevine.html
- Public playlist metadata verified locally on 2026-04-02 with:
  - `yt-dlp --flat-playlist --skip-download --print '%(playlist_index)s|%(title)s|%(webpage_url)s' 'https://www.youtube.com/playlist?list=PL_iWQOsE6TfVYGEGiAOMaOzzv41Jfm_Ps'`
- Local KB assignment notes:
  - `/Users/otonashi/thinking/building/cs-courses-kb/CS285/assignments/A1-behavioral-cloning.md`
  - `/Users/otonashi/thinking/building/cs-courses-kb/CS285/assignments/A1-dagger.md`
  - `/Users/otonashi/thinking/building/cs-courses-kb/CS285/assignments/A2-policy-gradients-reinforce.md`
  - `/Users/otonashi/thinking/building/cs-courses-kb/CS285/assignments/A3-q-learning-dqn.md`
  - `/Users/otonashi/thinking/building/cs-courses-kb/CS285/assignments/A4-model-based-rl.md`
  - `/Users/otonashi/thinking/building/cs-courses-kb/CS285/assignments/A5-exploration-rnd.md`
