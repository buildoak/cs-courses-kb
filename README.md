# CS Courses Knowledge Base

Structured knowledge base built from the best publicly available CS/AI university courses.
Not transcripts — enriched articles: key concepts, non-obvious insights, implementation notes, connections.

## Courses

### Tier 1 — Core (build first)

| # | Course | University | Instructor | Year | Lectures | Topic |
|---|--------|-----------|-----------|------|----------|-------|
| 1 | **CS336** | Stanford | Hashimoto, Liang | 2025 | 19 | Language Modeling from Scratch |
| 2 | **CS224N** | Stanford | Manning, Murty | 2024 | 22 | NLP with Deep Learning |
| 3 | **CS234** | Stanford | Brunskill | 2024 | 16 | Reinforcement Learning |
| 4 | **CS149** | Stanford | Fatahalian, Olukotun | 2023 | 18 | Parallel Computing |
| 5 | **CS161** | Stanford | Anari, Charikar | 2024 | ~20 | Design and Analysis of Algorithms |
| 6 | **CS166** | Stanford | Schwarz | 2024 | ~20 | Advanced Data Structures |

### Tier 2 — Systems & Applied

| # | Course | University | Instructor | Year | Lectures | Topic |
|---|--------|-----------|-----------|------|----------|-------|
| 7 | **CS285** | UC Berkeley | Levine | 2023 | 23 | Deep Reinforcement Learning |
| 8 | **CS294-196** | UC Berkeley | Song, Chen | 2024 | 13 | LLM Agents |
| 9 | **15-445** | CMU | Pavlo | 2024 | 26 | Database Systems |
| 10 | **10-414** | CMU | Kolter, Chen | 2022 | 23 | Deep Learning Systems |

### Tier 3 — Depth

| # | Course | University | Instructor | Year | Lectures | Topic |
|---|--------|-----------|-----------|------|----------|-------|
| 11 | **CS25** | Stanford | Various speakers | 2024-26 | 9+ | Transformers United |
| 12 | **CS330** | Stanford | Finn | 2023 | 20 | Meta-Learning |
| 13 | **CS236** | Stanford | Ermon, Grover | 2023 | 18 | Deep Generative Models |
| 14 | **CS224W** | Stanford | Leskovec | 2025 | 19 | ML with Graphs |
| 15 | **6.S191** | MIT | Amini, Soleimany | 2025 | 12 | Introduction to Deep Learning |

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

## Lecture Article Format

Each lecture article contains:
- **Summary** — what was covered (3-5 bullets)
- **Key Concepts** — definitions, algorithms, equations
- **Non-Obvious Insights** — things the speaker said that aren't in textbooks
- **Connections** — to other lectures, courses, papers, real-world systems
- **Referenced Papers** — with one-line descriptions
- **Timestamps** — key moments in the video

## Assignment Article Format

Each assignment article contains:
- **Problem Statement** — what you're building
- **Key Techniques** — which lecture concepts you apply
- **Implementation Walkthrough** — from GitHub solutions, architecture and approach
- **What You Learn** — the pedagogical punchline

## GitHub Assignment Sources

| Course | Repo | Notes |
|--------|------|-------|
| CS336 | [stanford-cs336](https://github.com/stanford-cs336) | Official org, 5 assignments, executable lectures |
| CS149 | [stanford-cs149](https://github.com/stanford-cs149) | Official org, 4 assignments incl. Trainium |
| CS224N | [CS224N-Spring2024-DFP](https://github.com/amahankali10/CS224N-Spring2024-DFP-Student-Handout) | BERT implementation starter |
| CS231n | [cs231n.github.io](https://cs231n.github.io/) | Official assignment specs |
| 10-414 | [dlsyscourse.org](https://dlsyscourse.org/lectures/) | Needle framework (build PyTorch) |
| 6.S191 | [MITDeepLearning](https://github.com/MITDeepLearning/introtodeeplearning) | Official lab repo |
| 15-445 | [cmu-db/bustub](https://github.com/cmu-db/bustub) | Public codebase |
| CS236 | [deepgenerativemodels](https://github.com/deepgenerativemodels/notes) | Official notes repo |

## Progress

| Course | Lectures | Assignments | Gems | Status |
|--------|----------|-------------|------|--------|
| CS336 | 0/19 | 0/5 | - | not started |
| ... | | | | |

---

Built by R. Jenkins dispatch workers. Each lecture is a ticket.
