# CS221: Artificial Intelligence: Principles and Techniques

Stanford's AI survey course, using the Spring 2025 archive as the source of truth here. The class keeps the classic AI spine intact: search, heuristics, planning, probabilistic inference, and logic, but wraps it in short modules, lectures, problem sessions, and weekly homeworks that force implementation.

## Quick Facts

- **Course:** CS221: Artificial Intelligence: Principles and Techniques
- **School:** Stanford
- **Offering used for this KB page:** Spring 2025 archive
- **Instructors:** Moses Charikar and Zachary Robertson
- **Public video source:** [Stanford CS221 public playlist](https://www.youtube.com/playlist?list=PLoROMvodv4rOca_Ovz1DvdtWuz8BfSWL2)
- **Official course page:** [Stanford CS221 Spring 2025](https://stanford-cs221.github.io/spring2025/)
- **Modules index:** [CS221 modules](https://stanford-cs221.github.io/spring2025/modules/index.html)
- **Coursework page:** [CS221 coursework](https://stanford-cs221.github.io/spring2025/#coursework)

## Course Mechanics

- **Lectures:** Mon/Wed, 10:30am-12pm, NVIDIA Auditorium
- **Problem sessions:** Fri, 10:30am-12pm, NVIDIA Auditorium
- **Format:** short modules with slides, recorded videos, and notes
- **Homework style:** weekly assignments with written and programming parts
- **Grading spine:** prerequisites quiz (0.5%), weekly homeworks (40%), two in-person exams (59.5%), optional project extra credit (up to 1.5%)
- **Project option:** milestone-based extra-credit project

## Syllabus

### Core Arc

The Spring 2025 schedule runs through these themes:

1. Introduction and prerequisites
2. Machine learning I, II, and III
3. Search
4. Search and heuristics
5. MDPs I and II
6. Games I and II
7. Factor graphs and beam search
8. Markov nets, Bayesian nets, and logic
9. Conclusion

### Weekly Sequence

- **Week 1:** Introduction, prerequisites, Machine Learning I
- **Week 2:** Machine Learning II, Machine Learning III
- **Week 3:** Search, Search & Heuristics
- **Week 4:** MDPs I, MDPs II
- **Week 5:** Games I, Games II
- **Week 6:** Factor Graphs
- **Week 7:** Markov Nets, Bayesian Nets I-II
- **Week 8:** Bayesian Nets III, Logic I
- **Week 9:** Logic II
- **Week 10:** Conclusion

### Homework Arc

- **Foundations:** Python utilities, sparse vectors, and basic algorithmic muscle
- **Sentiment:** linear classification and feature engineering
- **Route:** graph search and heuristics
- **Mountaincar:** MDPs and reinforcement learning
- **Pacman:** adversarial search
- **Scheduling:** constraint satisfaction
- **Car:** Bayesian tracking and inference
- **Logic:** theorem proving and knowledge representation

## Instructor Bios

### Moses Charikar

Donald E. Knuth Professor of Computer Science at Stanford. His Stanford profile says his research focuses on efficient algorithms for massive high-dimensional data, optimization problems in machine learning, approximation algorithms, convex optimization, and low-distortion embeddings. He earned his PhD at Stanford and spent time at Google and Princeton before returning to Stanford.

### Zachary Robertson

Stanford CS PhD working on scalable oversight. His Stanford bio says he builds information-theoretic mechanisms for evaluating AI systems without ground truth, and works end-to-end from proofs to benchmarks to pilots.

## Links

- [Stanford CS221 Spring 2025 course page](https://stanford-cs221.github.io/spring2025/)
- [CS221 modules index](https://stanford-cs221.github.io/spring2025/modules/index.html)
- [CS221 coursework](https://stanford-cs221.github.io/spring2025/#coursework)
- [CS221 schedule](https://stanford-cs221.github.io/spring2025/#schedule)
- [CS221 public playlist](https://www.youtube.com/playlist?list=PLoROMvodv4rOca_Ovz1DvdtWuz8BfSWL2)
- [Moses Charikar Stanford profile](https://profiles.stanford.edu/moses-charikar)
- [Zachary Robertson Stanford bio](https://stairlab.stanford.edu/members/zachary_robertson.html)

## Why It Matters

Inference from the syllabus arc:

- It gives one clean pass through the major AI representations: search trees, state machines, factor graphs, Bayesian nets, and logic.
- It does not treat AI as a single model family. It shows where the problem lives before the model does.
- The homeworks force the implementation layer to match the theory layer, which is the only reason the course sticks.
- It is still relevant to modern agent work because search, heuristics, planning, uncertainty, and symbolic reasoning are the primitives under the stack.
- It is a useful bridge from textbook AI to systems that need to decide, explain, and recover under uncertainty.

## Sources

- Stanford CS221 Spring 2025 course page: https://stanford-cs221.github.io/spring2025/
- Stanford CS221 modules index: https://stanford-cs221.github.io/spring2025/modules/index.html
- Stanford CS221 coursework section: https://stanford-cs221.github.io/spring2025/#coursework
- Stanford CS221 schedule section: https://stanford-cs221.github.io/spring2025/#schedule
- Moses Charikar Stanford profile: https://profiles.stanford.edu/moses-charikar
- Zachary Robertson Stanford bio: https://stairlab.stanford.edu/members/zachary_robertson.html
- Public playlist metadata verified locally on 2026-04-02 with:
  - `yt-dlp --flat-playlist --skip-download --print '%(playlist_index)s|%(title)s|%(webpage_url)s' 'https://www.youtube.com/playlist?list=PLoROMvodv4rOca_Ovz1DvdtWuz8BfSWL2'`
