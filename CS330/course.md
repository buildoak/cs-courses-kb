# CS330: Deep Multi-Task and Meta Learning

Stanford's Fall 2022 CS330 archive, pinned here because the supplied YouTube playlist matches the 2022 lecture run. The course is about how to use shared structure across tasks to learn faster, generalize better, and adapt with less data.

## Quick Facts

- **Course:** CS330: Deep Multi-Task and Meta Learning
- **School:** Stanford
- **Offering used for this KB page:** Fall 2022 archive
- **Instructor:** Chelsea Finn
- **Public video source:** [Stanford CS330 2022 YouTube playlist](https://www.youtube.com/playlist?list=PLoROMvodv4rNjRoawgt72BBNwL2V7doGI)
- **Official course archive:** [Stanford CS330 Fall 2022](https://cs330.stanford.edu/fall2022/index.html)
- **Lecture time:** Mon / Wed, 3:00pm - 4:20pm
- **Location:** Skilling Auditorium
- **Prerequisite:** CS229 or equivalent introductory machine learning
- **Format:** in-person lectures, livestreamed and recorded, four graded homework assignments, one optional homework assignment, and a research-style project

## Course Mechanics

- **Grading:** Homework 0 is a warmup worth 5%; Homeworks 1-3 are each worth 15%; Homework 4 is optional and can replace one of Homeworks 1-3 or part of the project grade; the project is 50%.
- **Project:** research-level, group size 1-3, with a poster session and final report.
- **Public archive note:** the official course page says recorded lecture videos were posted to Canvas for the current offering, and public recordings of previous offerings are linked separately.
- **Schedule note:** the Fall 2022 archive also lists a Hanie Sedghi guest lecture and a final poster session that are not part of the public YouTube playlist.

## Syllabus

### Core Arc

The course moves in a straight line from task sharing to task adaptation:

- Multi-task learning basics and transfer learning
- Fine-tuning and the fragility of pretrained representations
- Black-box meta-learning and in-context learning
- Optimization-based meta-learning
- Few-shot learning via metric learning
- Self-supervised pre-training for few-shot learning, both contrastive and generative
- Advanced meta-learning topics: task construction and large-scale meta-optimization
- Variational inference and Bayesian meta-learning
- Domain adaptation and domain generalization
- Lifelong learning
- Frontiers and open challenges

### Public Playlist

The public playlist has 17 videos:

| # | Public lecture video |
|---|---|
| 1 | What is multi-task learning? |
| 2 | Multi-task learning basics |
| 3 | Transfer learning, meta learning |
| 4 | Black box meta learning |
| 5 | Optimization-based meta-learning |
| 6 | Non-parametric few-shot learning |
| 7 | Unsupervised pre-training: contrastive learning |
| 8 | Unsupervised pre-training for few-shot learning |
| 9 | Advanced meta-learning topics: task construction |
| 10 | Advanced meta-learning 2: large-scale meta-optimization |
| 11 | Variational inference and generative models |
| 12 | Bayesian meta-learning |
| 13 | Domain adaptation |
| 14 | Domain generalization |
| 15 | Lifelong learning |
| 16 | Frontiers and open challenges |
| 17 | Percy Liang guest lecture |

### Homework Arc

- **Homework 0:** warmup
- **A1:** multitask training for recommender systems
- **A2:** prototypical networks
- **A3:** model-agnostic meta-learning
- **A4:** few-shot learning with pre-trained language models
- **A5:** advanced meta-learning topics

## Instructor Bios

### Chelsea Finn

Assistant Professor of Computer Science and Electrical Engineering at Stanford, and the William George and Ida Mary Hoover Faculty Fellow. Her Stanford bio says her work is about enabling robots and other agents to develop broadly intelligent behavior through learning and interaction. Her research sits at the intersection of machine learning and robotic control, including visual perception, robotic manipulation, deep reinforcement learning, and meta-learning algorithms for fast learning.

## Links

- [Official CS330 Fall 2022 archive](https://cs330.stanford.edu/fall2022/index.html)
- [Public CS330 2022 playlist](https://www.youtube.com/playlist?list=PLoROMvodv4rNjRoawgt72BBNwL2V7doGI)
- [Chelsea Finn Stanford profile](https://profiles.stanford.edu/chelsea-finn)
- [Chelsea Finn academic website](http://ai.stanford.edu/~cbfinn)
- [A1: Multitask Training for Recommender Systems](./assignments/A1-multitask-training-for-recommender-systems.md)
- [A2: Prototypical Networks](./assignments/A2-prototypical-networks.md)
- [A3: Model-Agnostic Meta-Learning](./assignments/A3-model-agnostic-meta-learning.md)
- [A4: Few-Shot Learning with Pre-trained Language Models](./assignments/A4-few-shot-learning-with-pre-trained-language-models.md)
- [A5: Advanced Meta-Learning Topics](./assignments/A5-advanced-meta-learning-topics.md)

## Why It Matters

Inference from the syllabus arc:

- It shows the whole adaptation stack in one course: shared structure, transfer, meta-learning, pretraining, and continual learning.
- It makes the failure modes explicit. Task construction, memorization, domain shift, and long-horizon adaptation are the real problems, not side quests.
- It is a clean bridge from classical few-shot learning to the modern pretraining mindset used around foundation models.
- It is research-shaped, not just implementation-shaped. The project is framed as a research project, not a homework bucket.
- It teaches the difference between learning a good representation and learning a fast optimizer. That split matters everywhere in modern ML.

## Sources

- Official course archive: https://cs330.stanford.edu/fall2022/index.html
- Stanford Profiles bio for Chelsea Finn: https://profiles.stanford.edu/chelsea-finn
- Chelsea Finn academic website: http://ai.stanford.edu/~cbfinn
- YouTube playlist metadata verified locally on 2026-04-02 with:
  - `yt-dlp --flat-playlist --skip-download --print '%(playlist_index)s|%(title)s|%(webpage_url)s' 'https://www.youtube.com/playlist?list=PLoROMvodv4rNjRoawgt72BBNwL2V7doGI'`
- Local CS330 assignment docs used for the homework links:
  - `/Users/otonashi/thinking/building/cs-courses-kb/CS330/assignments/A1-multitask-training-for-recommender-systems.md`
  - `/Users/otonashi/thinking/building/cs-courses-kb/CS330/assignments/A2-prototypical-networks.md`
  - `/Users/otonashi/thinking/building/cs-courses-kb/CS330/assignments/A3-model-agnostic-meta-learning.md`
  - `/Users/otonashi/thinking/building/cs-courses-kb/CS330/assignments/A4-few-shot-learning-with-pre-trained-language-models.md`
  - `/Users/otonashi/thinking/building/cs-courses-kb/CS330/assignments/A5-advanced-meta-learning-topics.md`
