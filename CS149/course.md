# CS149: Parallel Computing

Stanford's parallel-computing course, using the public Fall 2023 archive as the source of truth here. The class is about the hidden cost model under modern software: multicore CPUs, SIMD, GPUs, Spark-style data parallelism, caches, synchronization, and hardware specialization.

## Quick Facts

- **Course:** CS149: Parallel Computing
- **School:** Stanford
- **Offering used for this KB page:** Fall 2023 archive
- **Instructors:** Kayvon Fatahalian and Kunle Olukotun
- **Public video source:** [Stanford CS149 2023 YouTube playlist](https://youtube.com/playlist?list=PLoROMvodv4rMp7MTFr4hQsDEcX7Bx6Odp)
- **Official course page:** [Stanford CS149 Fall 2023](https://gfxcourses.stanford.edu/cs149/fall23)
- **Assignment repo set:** [stanford-cs149](https://github.com/stanford-cs149)

## Course Mechanics

- **Schedule:** Tuesdays and Thursdays, 10:30-11:50am
- **Location:** NVIDIA Auditorium
- **Assessments:** five programming assignments, five written assignments, a midterm exam, and a final exam
- **Public archive note:** the Stanford page says public lecture videos are available from the 2023 run on Stanford's YouTube channel

## Syllabus

### Core Arc

The 2023 playlist runs as a 19-video arc:

1. Why parallelism exists, and why efficiency matters
2. Multicore CPU basics
3. Multicore architecture, latency vs. bandwidth, and ISPC abstractions
4. Parallel programming basics in shared-memory and data-parallel styles
5. Work distribution, scheduling, and work stealing
6. Locality, communication, pipelining, and contention
7. GPU architecture and CUDA programming
8. Data-parallel thinking: map, reduce, scan, prefix sum, groupByKey
9. Distributed data-parallel computing with Spark
10. Efficient DNN evaluation on GPUs, including transformers and layer fusion
11. Cache coherence and false sharing
12. Memory consistency and acquire/release semantics
13. Fine-grained synchronization and lock-free programming
14. Midterm review
15. Domain-specific programming languages
16. Transactional memory, part 1
17. Transactional memory, part 2
18. Hardware specialization
19. Memory-system wrap-up and course closeout

### Programming Assignments

- **A1:** Parallel performance analysis on a quad-core CPU
- **A2:** Task-graph scheduling on a multicore CPU
- **A3:** A CUDA renderer
- **A4:** A FlashAttention-style transformer DNN, labeled "Chat149"
- **A5 (optional):** Big graph processing

### Written Work

- Written assignments 1-5 are listed on the Stanford course page as separate PDFs.

## Instructor Bios

### Kayvon Fatahalian

Associate Professor of Computer Science at Stanford. His Stanford bio says his research centers on real-time graphics, high-efficiency simulation engines for entertainment and AI, and large-scale image/video analysis.

### Kunle Olukotun

Cadence Design Systems Professor and Professor of Electrical Engineering and Computer Science at Stanford. His Stanford bio describes him as a pioneer in multicore processor design, leader of the Hydra CMP project, co-founder of Afara Websystems and SambaNova Systems, and director of the Pervasive Parallel Lab and the DAWN Lab.

## Links

- [Stanford CS149 Fall 2023 course page](https://gfxcourses.stanford.edu/cs149/fall23)
- [Public CS149 2023 playlist](https://youtube.com/playlist?list=PLoROMvodv4rMp7MTFr4hQsDEcX7Bx6Odp)
- [Kayvon Fatahalian Stanford bio](https://cap.stanford.edu/profiles/frdActionServlet?choiceId=printerprofile&profileId=182700&profileversion=full)
- [Kunle Olukotun Stanford bio](https://engineering.stanford.edu/people/oyekunle-olukotun)
- [Assignment 1 repo](https://github.com/stanford-cs149/asst1)
- [Assignment 2 repo](https://github.com/stanford-cs149/asst2)
- [Assignment 3 repo](https://github.com/stanford-cs149/asst3)
- [Assignment 4 repo](https://github.com/stanford-cs149/cs149gpt)
- [Optional Assignment 5 repo](https://github.com/stanford-cs149/biggraphs-ec)

## Why It Matters

Inference from the syllabus arc:

- It makes the machine visible. The course turns "fast code" into a discussion of bandwidth, scheduling, coherence, and consistency.
- It connects the main parallel-computing substrates in one place: multicore CPUs, GPUs, distributed data parallelism, and specialized accelerators.
- It is relevant to AI work because the same bottlenecks show up in model inference, kernel design, and accelerator architecture.
- It is a clean bridge from programming models to actual systems trade-offs, not a toy CUDA-only survey.

## Sources

- Stanford CS149 Fall 2023 course page: https://gfxcourses.stanford.edu/cs149/fall23
- Stanford course page note that public lecture videos are available from the 2023 run on Stanford's YouTube channel: https://gfxcourses.stanford.edu/cs149/fall23
- YouTube playlist metadata verified locally on 2026-04-02 with:
  - `yt-dlp --flat-playlist --skip-download --print '%(playlist_index)s|%(title)s|%(webpage_url)s' 'https://youtube.com/playlist?list=PLoROMvodv4rMp7MTFr4hQsDEcX7Bx6Odp'`
- Kayvon Fatahalian Stanford bio: https://cap.stanford.edu/profiles/frdActionServlet?choiceId=printerprofile&profileId=182700&profileversion=full
- Kunle Olukotun Stanford bio: https://engineering.stanford.edu/people/oyekunle-olukotun
- Assignment repo URLs extracted from the Stanford course page HTML on 2026-04-02
