# CS234: Reinforcement Learning

Stanford's public RL spine. This file tracks the Spring 2024 offering: 16 public lectures, 3 assignments, and a project track that ends in a poster session and final report.

## Overview

CS234 treats reinforcement learning as decision-making under uncertainty, not as a game-playing curiosity. The course starts with tabular MDPs and Bellman backups, moves through Q-learning and function approximation, then pushes into policy gradients, imitation learning, offline RL, exploration, multi-agent game playing, and value alignment.

The official course page frames RL as relevant to robotics, game playing, consumer modeling, and healthcare. The project page is explicit that careful negative results are acceptable if the reasoning and evidence are solid.

## Syllabus

### Course Arc

- Lectures 1-4: foundations, planning, policy evaluation, and Q-learning.
- Lectures 5-7: policy search and the basic policy-gradient stack.
- Lectures 8-10: imitation learning, human feedback, offline RL, and DPO.
- Lectures 11-13: exploration, bandits, and data-efficient RL.
- Lectures 14-16: multi-agent game playing, rewards, and value alignment.

### Public Lecture List

| # | Lecture |
|---|---|
| 1 | Introduction to Reinforcement Learning |
| 2 | Tabular MDP Planning |
| 3 | Policy Evaluation |
| 4 | Q-learning and Function Approximation |
| 5 | Policy Search 1 |
| 6 | Policy Search 2 |
| 7 | Policy Search 3 |
| 8 | Offline RL 1 |
| 9 | Guest Lecture on DPO |
| 10 | Offline RL 3 |
| 11 | Exploration 1 |
| 12 | Exploration 2 |
| 13 | Exploration 3 |
| 14 | Multi-Agent Game Playing |
| 15 | Emma Brunskill & Dan Webber |
| 16 | Value Alignment |

### Assignments and Project

- **Assignment 1:** `Effect of Effective Horizon`, `Reward Hacking`, and `Bellman Residuals and performance bounds`. Due April 12, 2024 at 6:00 pm PST.
- **Assignment 2:** `Deep Q-Networks` and `Policy Gradient Methods`. Due April 26, 2024 at 6:00 pm PST.
- **Assignment 3:** `An introduction to reinforcement learning from human preferences`. Due May 17, 2024 at 6:00 pm PST.
- **Project:** proposal due May 5, milestone due May 22, poster session June 5, final report June 11.

The project page is worth reading on its own. It says projects do not have to succeed to be strong; a careful explanation of why an idea failed is valid work if the analysis is rigorous.

## Instructor Bios

- **Emma Brunskill** - associate tenured professor in Stanford's Computer Science Department. Her work aims to build AI systems that learn from few samples and still make good decisions, with applications in healthcare and education.
- **Dan Webber** - COLLEGE lecturer at Stanford. He previously held a fellowship at Stanford's McCoy Family Center for Ethics in Society, earned a PhD in philosophy from the University of Pittsburgh in 2023, and has a BA in computer science from Amherst College.

## Course Staff

- Dilip Arumugam, Head TA
- Jonathan Lee
- Garrett Thomas
- Saurabh Kumar
- Joao Araujo
- Chethan Bheteja

## Why This Course Matters

- RL is the control layer for systems that have to act, not just classify.
- The course covers the real failure modes: exploration, sample efficiency, off-policy instability, reward design, and human feedback.
- It shows the full ladder from tabular planning to alignment, which is the right way to understand the field instead of treating deep RL as one isolated technique.
- The project policy encourages research behavior, not homework behavior. Negative results are allowed if they are argued properly. That is the useful norm.

## Links

- Official Spring 2024 course page: https://web.stanford.edu/class/cs234/CS234Spr2024/index.html
- Lecture materials: https://web.stanford.edu/class/cs234/CS234Spr2024/modules.html
- Course project: https://web.stanford.edu/class/cs234/CS234Spr2024/project.html
- YouTube playlist: https://youtube.com/playlist?list=PLoROMvodv4rN4wG6Nk6sNpTEbuOSosZdX
- Assignment 1 PDF: https://web.stanford.edu/class/cs234/CS234Spr2024/assignments/a1/CS234_A1.pdf
- Assignment 2 PDF: https://web.stanford.edu/class/cs234/CS234Spr2024/assignments/a2/CS234_A2.pdf
- Assignment 3 PDF: https://web.stanford.edu/class/cs234/CS234Spr2024/assignments/a3/CS234_A3.pdf
- Emma Brunskill bio: https://cs.stanford.edu/people/ebrun/
- Dan Webber bio: https://college.stanford.edu/people/daniel-webber
- Sutton and Barto textbook: https://incompleteideas.net/book/the-book-2nd.html

## Sources

- Official course page: https://web.stanford.edu/class/cs234/CS234Spr2024/index.html
- Lecture materials page: https://web.stanford.edu/class/cs234/CS234Spr2024/modules.html
- Project page: https://web.stanford.edu/class/cs234/CS234Spr2024/project.html
- Assignment 1 PDF: https://web.stanford.edu/class/cs234/CS234Spr2024/assignments/a1/CS234_A1.pdf
- Assignment 2 PDF: https://web.stanford.edu/class/cs234/CS234Spr2024/assignments/a2/CS234_A2.pdf
- Assignment 3 PDF: https://web.stanford.edu/class/cs234/CS234Spr2024/assignments/a3/CS234_A3.pdf
- YouTube playlist listing command: `yt-dlp --flat-playlist --print '%(playlist_index)02d %(title)s | %(webpage_url)s' 'https://youtube.com/playlist?list=PLoROMvodv4rN4wG6Nk6sNpTEbuOSosZdX'`
- Emma Brunskill bio: https://cs.stanford.edu/people/ebrun/
- Dan Webber bio: https://college.stanford.edu/people/daniel-webber
