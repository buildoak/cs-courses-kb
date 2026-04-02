# CS336: Language Modeling from Scratch

Stanford / Spring 2025. The public playlist has 17 videos. The archived course site lists 19 scheduled sessions, with two guest lectures that do not appear in the playlist.

## Overview
CS336 is the build-it-yourself language modeling course. The official description says it walks students through the whole stack: data collection and cleaning, transformer construction, model training, evaluation, and deployment.

It is also a 5-unit, implementation-heavy class. The course site explicitly warns that students will write far more code than in a typical AI course and need to be comfortable with Python, deep learning, and systems concepts.

## Why This Course Matters
- It treats language models as systems to build, not APIs to consume.
- It forces contact with the expensive parts of modern LMs: tokenization, compute, memory, kernels, distributed parallelism, data curation, and alignment.
- The assignments mirror the lecture arc, so the course becomes a map of how a language model actually gets made.

Source: the archived Spring 2025 course site and the assignment list on that site.

## Instructors
- **Tatsunori Hashimoto** - Assistant Professor at Stanford. His Stanford HAI bio says his research uses statistics to make ML systems more robust and trustworthy, especially large language models.
- **Percy Liang** - Professor of Computer Science at Stanford and director of the Center for Research on Foundation Models (CRFM). His Stanford Profiles bio says he focuses on making foundation models, especially language models, more accessible through open-source and more understandable through rigorous benchmarking.

Course staff on the archived site also list Neil Band, Marcel Rød, and Rohith Kuditipudi as CAs.

## Syllabus
### Lecture sequence
1. **Overview and Tokenization** - [video](https://www.youtube.com/watch?v=SQ3fZ1sAqXI) - [KB note](../cs336/lectures/L01-tokenization.md)
2. **PyTorch, Resource Accounting** - [video](https://www.youtube.com/watch?v=msHyYioAyNE) - [KB note](../cs336/lectures/L02-neural-lm-basics.md)
3. **Architectures, Hyperparameters** - [video](https://www.youtube.com/watch?v=ptFiH_bHnJw) - [KB note](../cs336/lectures/L03-training-from-scratch.md)
4. **Mixture of Experts** - [video](https://www.youtube.com/watch?v=LPv1KfUXLCo) - [KB note](../cs336/lectures/L04-mixture-of-experts.md)
5. **GPUs** - [video](https://www.youtube.com/watch?v=6OBtO9niT00) - [KB note](../cs336/lectures/L05-gpus.md)
6. **Kernels, Triton** - [video](https://www.youtube.com/watch?v=E8Mju53VB00) - [KB note](../cs336/lectures/L06-kernels-triton.md)
7. **Parallelism 1** - [video](https://www.youtube.com/watch?v=l1RJcDjzK8M) - [KB note](../cs336/lectures/L07-parallelism-1.md)
8. **Parallelism 2** - [video](https://www.youtube.com/watch?v=LHpr5ytssLo) - [KB note](../cs336/lectures/L08-parallelism-2.md)
9. **Scaling laws 1** - [video](https://www.youtube.com/watch?v=6Q-ESEmDf4Q)
10. **Inference** - [video](https://www.youtube.com/watch?v=fcgPYo3OtV0)
11. **Scaling laws 2** - [video](https://www.youtube.com/watch?v=OSYuUqGBQxw)
12. **Evaluation** - [video](https://www.youtube.com/watch?v=x-R5l2HsXqM)
13. **Data 1** - [video](https://www.youtube.com/watch?v=WePxmeXU1xg)
14. **Data 2** - [video](https://www.youtube.com/watch?v=9Cd0THLS1t0)
15. **Alignment - SFT/RLHF** - [video](https://www.youtube.com/watch?v=Dfu7vC9jo4w)
16. **Alignment - RL 1** - [video](https://www.youtube.com/watch?v=46f2QTDB08Q)
17. **Alignment - RL 2** - [video](https://www.youtube.com/watch?v=JdGFdViaOJk)
18. **Guest lecture: Junyang Lin** - listed on the archived course site only.
19. **Guest lecture: Mike Lewis** - listed on the archived course site only.

The archived course site is the source of the 19-session schedule. The public playlist stops at lecture 17.

### Assignments
- **A1 Basics** - [Byte-Pair Tokenizer](../cs336/assignments/A1-bpe-tokenizer.md), [Transformer LM Training](../cs336/assignments/A1-transformer-lm-training.md)
- **A2 Systems** - [Systems, Parallelism, and Optimizer State Sharding](../cs336/assignments/A2-systems-parallelism.md)
- **A3 Scaling** - [Scaling Laws](../cs336/assignments/A3-scaling-laws.md)
- **A4 Data** - [Data Curation and Filtering](../cs336/assignments/A4-data-curation-and-filtering.md)
- **A5 Alignment** - [Preference Optimization and Alignment](../cs336/assignments/A5-preference-optimization-and-alignment.md)

The course site frames the assignments as the practical spine of the class: build a tokenizer and Transformer LM, optimize systems pieces, study scaling, curate data, and train alignment methods.

## Links
- [Archived Spring 2025 course site](https://cs336.stanford.edu/spring2025/)
- [Stanford Online course page](https://online.stanford.edu/courses/cs336-language-modeling-scratch)
- [YouTube playlist: Stanford CS336 Language Modeling from Scratch I 2025](https://www.youtube.com/playlist?list=PLoROMvodv4rOY23Y0BoGoBGgQ1zmU_MT_)
- [Official CS336 GitHub org](https://github.com/stanford-cs336)
- [Local lecture notes](../cs336/lectures/)
- [Local assignment notes](../cs336/assignments/)

## Sources
- Official course site: https://cs336.stanford.edu/spring2025/
- YouTube playlist metadata: https://www.youtube.com/playlist?list=PLoROMvodv4rOY23Y0BoGoBGgQ1zmU_MT_
- Tatsunori Hashimoto bio: https://hai.stanford.edu/people/tatsunori-hashimoto
- Percy Liang bio: https://profiles.stanford.edu/percy-liang
- KB layout: /Users/otonashi/thinking/building/cs-courses-kb/README.md
- Local KB lecture notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/lectures/L01-tokenization.md
- Local KB lecture notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/lectures/L02-neural-lm-basics.md
- Local KB lecture notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/lectures/L03-training-from-scratch.md
- Local KB lecture notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/lectures/L04-mixture-of-experts.md
- Local KB lecture notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/lectures/L05-gpus.md
- Local KB lecture notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/lectures/L06-kernels-triton.md
- Local KB lecture notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/lectures/L07-parallelism-1.md
- Local KB lecture notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/lectures/L08-parallelism-2.md
- Local KB assignment notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/assignments/A1-bpe-tokenizer.md
- Local KB assignment notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/assignments/A1-transformer-lm-training.md
- Local KB assignment notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/assignments/A2-systems-parallelism.md
- Local KB assignment notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/assignments/A3-scaling-laws.md
- Local KB assignment notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/assignments/A4-data-curation-and-filtering.md
- Local KB assignment notes: /Users/otonashi/thinking/building/cs-courses-kb/cs336/assignments/A5-preference-optimization-and-alignment.md
