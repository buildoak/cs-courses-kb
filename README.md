# CS Courses Knowledge Base

Structured knowledge base built from the best publicly available CS/AI university courses.
Not transcripts — enriched articles: key concepts, non-obvious insights, implementation notes, connections.

---

## Course Catalog

### Full Video + Assignments (KB-ready)

Courses with public lecture recordings and publicly available assignments. Primary build targets.

| Course | University | Year | Lectures | Assignments | Topic |
|--------|-----------|------|----------|-------------|-------|
| **CS336** | Stanford | 2025 | 19 | 5 | Language Modeling from Scratch |
| **CS224N** | Stanford | 2024 | 19 | 4 | NLP with Deep Learning |
| **CS234** | Stanford | 2024 | 16 | 3 | Reinforcement Learning |
| **CS149** | Stanford | 2023 | 18 | 5 | Parallel Computing |
| **CS25** | Stanford | 2021–25 | 47+ | 0 | Transformers United (seminar) |
| **CS330** | Stanford | 2022 | 20 | 5 | Meta-Learning |
| **CS236** | Stanford | 2023 | 16 | 3 | Deep Generative Models |
| **CS230** | Stanford | 2025 | 9 | 22 (Coursera) | Deep Learning |
| **CS221** | Stanford | 2021 | ~20 | 8 | AI: Principles & Techniques |
| **CS224U** | Stanford | 2023 | ~14 | 3 | Natural Language Understanding |
| **CS285** | UC Berkeley | 2023 | 23 | 5 | Deep Reinforcement Learning |
| **CS294-196** | UC Berkeley | 2024 | 12 | 1 | LLM Agents (seminar) |
| **15-445** | CMU | 2024 | 26 | 11 | Database Systems |
| **10-414** | CMU | 2022 | 23 | 7 | Deep Learning Systems |
| **6.S191** | MIT | 2026 | 9 | 3 | Introduction to Deep Learning |

### Slides Only (no public video)

Courses worth tracking for their assignments and notes, but without accessible recordings.

| Course | University | Slides Year | Lectures | Assignments | Notes | Topic |
|--------|-----------|-------------|----------|-------------|-------|-------|
| **CS161** | Stanford | 2026 | 18 | 8 | Video Canvas-only | Design & Analysis of Algorithms |
| **CS166** | Stanford | 2026 | 19 | 6 | Never had public video — Keith Schwarz slides are legendary | Advanced Data Structures |
| **CS229** | Stanford | 2023 notes | 20 | 5 | Last public video: 2018 (Andrew Ng) | Machine Learning |
| **CS231n** | Stanford | 2026 slides | 16 | 3 | Last public video: 2017 | Deep Learning for Computer Vision |
| **CS224W** | Stanford | 2025 slides | 19 | 8 | Last public video: 2021 | ML with Graphs |

### Special: CS153 — Frontier Systems (Stanford)

Guest lecture seminar: Jensen Huang, Sam Altman, Andrej Karpathy, Satya Nadella, Ben Mann (Anthropic).
Videos are patchwork — availability depends on individual speaker consent. Cherry-pick individual talks rather than treating as a full course.

---

## Structure

```
cs-courses-kb/
├── README.md
├── {course}/
│   ├── course.md              # Overview, syllabus, links, why it matters
│   ├── lectures/
│   │   ├── L01-{slug}.md      # Enriched lecture article
│   │   └── ...
│   ├── assignments/
│   │   ├── A1-{slug}.md       # Assignment walkthrough + GitHub solutions
│   │   └── ...
│   └── gems.md                # Non-obvious insights across all lectures
```

---

## Lecture Article Format

Each lecture article contains:
- **Summary** — what was covered (3-5 bullets)
- **Key Concepts** — definitions, algorithms, equations
- **Non-Obvious Insights** — things the speaker said that aren't in textbooks
- **Connections** — to other lectures, courses, papers, real-world systems
- **Referenced Papers** — with one-line descriptions
- **Timestamps** — key moments in the video

---

## Assignment Article Format

Each assignment article contains:
- **Problem Statement** — what you're building
- **Key Techniques** — which lecture concepts you apply
- **Implementation Walkthrough** — from GitHub solutions, architecture and approach
- **What You Learn** — the pedagogical punchline

---

## GitHub Assignment Sources

| Course | Repo | Notes |
|--------|------|-------|
| CS336 | [stanford-cs336](https://github.com/stanford-cs336) | Official org, 5 assignments, executable lectures |
| CS149 | [stanford-cs149](https://github.com/stanford-cs149) | Official org, 5 assignments incl. Trainium |
| CS224N | [CS224N-Spring2024](https://github.com/amahankali10/CS224N-Spring2024-DFP-Student-Handout) | BERT implementation starter |
| CS234 | [cs234-2024](https://github.com/search?q=cs234+stanford+2024) | Assignment starters on GitHub |
| CS330 | [cs330-2022](https://cs330.stanford.edu/) | Official course site, 5 assignments |
| CS230 | [cs230-code-examples](https://github.com/cs230-stanford/cs230-code-examples) | Official, supplemented by Coursera |
| CS221 | [cs221-stanford](https://cs221.stanford.edu/) | Official site, 8 assignments |
| CS224U | [cs224u](https://github.com/cgpotts/cs224u) | Official repo (Christopher Potts) |
| CS285 | [rail-berkeley/deeprl-course](https://github.com/rail-berkeley/deeprl_course) | Official, 5 homework assignments |
| CS294-196 | [llm-agents-mooc](https://llmagents-learning.org/) | Course site, 1 project assignment |
| 15-445 | [cmu-db/bustub](https://github.com/cmu-db/bustub) | BusTub public codebase, 11 projects |
| 10-414 | [dlsyscourse.org](https://dlsyscourse.org/) | Needle framework (build PyTorch), 7 hw |
| 6.S191 | [MITDeepLearning](https://github.com/MITDeepLearning/introtodeeplearning) | Official lab repo, 3 labs |
| CS236 | [deepgenerativemodels](https://github.com/deepgenerativemodels/notes) | Official notes repo |
| CS231n | [cs231n.github.io](https://cs231n.github.io/) | Official assignment specs (slides-only era) |
| CS224W | [snap-stanford/cs224w](https://web.stanford.edu/class/cs224w/) | Official site, 8 colab assignments |

---

## Progress

| Course | Availability | Lectures | Assignments | Gems | Status |
|--------|-------------|----------|-------------|------|--------|
| CS336 | video + hw | 0/19 | 0/5 | — | not started |
| CS224N | video + hw | 0/19 | 0/4 | — | not started |
| CS234 | video + hw | 0/16 | 0/3 | — | not started |
| CS149 | video + hw | 0/18 | 0/5 | — | not started |
| CS25 | video | 0/47+ | — | — | not started |
| CS330 | video + hw | 0/20 | 0/5 | — | not started |
| CS236 | video + hw | 0/16 | 0/3 | — | not started |
| CS230 | video + hw | 0/9 | 0/22 | — | not started |
| CS221 | video + hw | 0/20 | 0/8 | — | not started |
| CS224U | video + hw | 0/14 | 0/3 | — | not started |
| CS285 | video + hw | 0/23 | 0/5 | — | not started |
| CS294-196 | video + hw | 0/12 | 0/1 | — | not started |
| 15-445 | video + hw | 0/26 | 0/11 | — | not started |
| 10-414 | video + hw | 0/23 | 0/7 | — | not started |
| 6.S191 | video + hw | 0/9 | 0/3 | — | not started |
| CS161 | slides only | 0/18 | 0/8 | — | not started |
| CS166 | slides only | 0/19 | 0/6 | — | not started |
| CS229 | slides only | 0/20 | 0/5 | — | not started |
| CS231n | slides only | 0/16 | 0/3 | — | not started |
| CS224W | slides only | 0/19 | 0/8 | — | not started |
| CS153 | patchwork | — | — | — | cherry-pick only |

---

Built by R. Jenkins dispatch workers. Each lecture is a ticket.
