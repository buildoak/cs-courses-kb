# CS-Courses KB — Archived Ticket Backlog

This file is the archived ticket backlog from the **v3 experiment** (pratchett-os research ticket system). It documents all tickets that were generated and executed to build this knowledge base, exported on 2026-04-03 as part of closing out the v3 ticket system.

The tickets lived in `/Users/otonashi/thinking/pratchett-os/centerpiece/tickets/tickets.db` under the `research` domain.

---

## Summary

| Status | Count | Meaning |
|--------|-------|---------|
| done | 159 | Completed — results are in this repo and in `pratchett-os/centerpiece/research/` |
| failed | 1 | Failed during execution (research-100: CS230 L04-L06) |
| approved | 0 | None — all queued tickets ran |
| **Total** | **160** | |

**Done/failed tickets completed their work.** Their results (lecture articles, course overviews, assignments, gems syntheses) are in the corresponding course directories in this repo.

---

## All Research Tickets

### Infrastructure / Non-CS-Course (research-001 to 005)

These tickets were system and tooling work that bootstrapped the research pipeline. They ran in the pratchett-os context, not in this repo.

| ID | Status | Title |
|----|--------|-------|
| research-001 | done | Process new ChatGPT export and rebuild search index |
| research-002 | done | Research agency-agents repo — dissect architecture, patterns, applicability, file research artifact |
| research-003 | done | Research G0DM0D3 repo — prompt engineering patterns and relevance to Jenkins Junior persona on Codex |
| research-004 | done | agent-mux: broad search output jams harness — research root cause and create fix plan |
| research-005 | done | Implement dispatch-layer response budget (128K cap) with artifact spill — fix oversized scout output jamming harness |

---

### CS-Course KB Build (research-006 to 160)

The core v3 experiment. Each ticket built one artifact: a course overview, a batch of lecture articles, an assignments article, or a gems synthesis.

#### Pilot Phase (006-009): CS336

| ID | Status | Title |
|----|--------|-------|
| research-006 | done | PILOT-3: CS336 Lectures 1-3 — tokenization, neural LM basics, training from scratch |
| research-007 | done | PILOT-5: CS336 Lectures 4-8 — scaling, architecture, optimization |
| research-008 | done | CS336 assignments batch 1 (Basics + Systems) |
| research-009 | done | CS336 assignments batch 2 (Scaling + Data + Alignment) |

#### Assignment Articles (010-031)

| ID | Status | Title |
|----|--------|-------|
| research-010 | done | CS224N assignments batch 1 (word vectors + neural nets) |
| research-011 | done | CS224N assignments batch 2 (self-attention + LLM benchmarking) |
| research-012 | done | CS234 assignments (reinforcement learning 1-3) |
| research-013 | done | CS149 assignments batch 1 (asst1 + asst2) |
| research-014 | done | CS149 assignments batch 2 (asst3 + asst4 + biggraphs) |
| research-015 | done | CS330 assignments batch 1 (MAML + ProtoNets + multi-task learning) |
| research-016 | done | CS330 assignments batch 2 (meta-learning assignments 4-5) |
| research-017 | done | CS236 assignments (deep generative models 1-3) |
| research-018 | done | CS221 assignments batch 1 (Foundations + Search + MDPs) |
| research-019 | done | CS221 assignments batch 2 (Games + CSPs + Bayesian) |
| research-020 | done | CS221 assignments batch 3 (Learning + Logic) |
| research-021 | done | CS285 assignments batch 1 (imitation learning + policy gradients) |
| research-022 | done | CS285 assignments batch 2 (Q-learning + MBRL + exploration) |
| research-023 | done | 15-445 assignments batch 1 (HW1-3: SQL + Indexes + Join) |
| research-024 | done | 15-445 assignments batch 2 (HW4-6: Concurrency + Recovery + Distributed) |
| research-025 | done | 15-445 projects batch 1 (P1-3: Buffer Pool + B+Tree + Query Execution) |
| research-026 | done | 15-445 projects batch 2 (P4-5: Concurrency + Distributed Query) |
| research-027 | done | 10-414 assignments batch 1 (Needle hw1-3: autodiff + nn + sequence models) |
| research-028 | done | 10-414 assignments batch 2 (Needle hw4-5: training + transformers) |
| research-029 | done | 10-414 assignments batch 3 (Needle hw6-7: GPU backend + distributed) |
| research-030 | done | 6.S191 labs (intro to deep learning labs 1-3) |
| research-031 | done | CS224U assignments (NLU 1-3) |

#### Course Overviews (032-046)

| ID | Status | Title |
|----|--------|-------|
| research-032 | done | CS-KB: CS336 course overview |
| research-033 | done | CS-KB: CS224N course overview |
| research-034 | done | CS-KB: CS234 course overview |
| research-035 | done | CS-KB: CS149 course overview |
| research-036 | done | CS-KB: CS25 course overview |
| research-037 | done | CS-KB: CS330 course overview |
| research-038 | done | CS-KB: CS236 course overview |
| research-039 | done | CS-KB: CS230 course overview |
| research-040 | done | CS-KB: CS221 course overview |
| research-041 | done | CS-KB: CS224U course overview |
| research-042 | done | CS-KB: CS285 course overview |
| research-043 | done | CS-KB: CS294-196 course overview |
| research-044 | done | CS-KB: 15-445 course overview |
| research-045 | done | CS-KB: 10-414 course overview |
| research-046 | done | CS-KB: 6.S191 course overview |

#### Lecture Articles: CS336 (047-050)

| ID | Status | Title |
|----|--------|-------|
| research-047 | done | CS336 L09-L11 — scaling, training dynamics, compute-optimal regimes |
| research-048 | done | CS336 L12-L14 — mixture of experts, efficient architectures, inference optimization |
| research-049 | done | CS336 L15-L17 — alignment, RLHF, post-training techniques |
| research-050 | done | CS336 L18-L19 — evaluation, benchmarks, frontier LLM analysis |

#### Lecture Articles: CS224N (051-057)

| ID | Status | Title |
|----|--------|-------|
| research-051 | done | CS224N L01-L03 — word vectors, word2vec, neural classifiers |
| research-052 | done | CS224N L04-L06 — backprop, neural networks, dependency parsing |
| research-053 | done | CS224N L07-L09 — RNNs, LSTMs, machine translation |
| research-054 | done | CS224N L10-L12 — transformers, pretraining, BERT |
| research-055 | done | CS224N L13-L15 — question answering, generation, coreference |
| research-056 | done | CS224N L16-L18 — knowledge graphs, multi-task learning, future of NLP |
| research-057 | done | CS224N L19 — model analysis and interpretability |

#### Lecture Articles: CS234 (058-063)

| ID | Status | Title |
|----|--------|-------|
| research-058 | done | CS234 L01-L03 — RL introduction, MDPs, dynamic programming |
| research-059 | done | CS234 L04-L06 — model-free prediction, Monte Carlo, temporal difference |
| research-060 | done | CS234 L07-L09 — value function approximation, DQN, policy gradients |
| research-061 | done | CS234 L10-L12 — actor-critic, model-based RL, exploration |
| research-062 | done | CS234 L13-L15 — offline RL, multi-agent RL, reward shaping |
| research-063 | done | CS234 L16 — RL safety, open problems |

#### Lecture Articles: CS149 (064-069)

| ID | Status | Title |
|----|--------|-------|
| research-064 | done | CS149 L01-L03 — parallel computing intro, ISPC, performance analysis |
| research-065 | done | CS149 L04-L06 — GPU architecture, CUDA programming, memory hierarchy |
| research-066 | done | CS149 L07-L09 — work distribution, synchronization, optimization |
| research-067 | done | CS149 L10-L12 — cache coherence, memory consistency, interconnects |
| research-068 | done | CS149 L13-L15 — domain-specific hardware, accelerators, scheduling |
| research-069 | done | CS149 L16-L18 — deep learning hardware mapping, sparsity, algorithm design |

#### Lecture Articles: CS25 (070-085)

| ID | Status | Title |
|----|--------|-------|
| research-070 | done | CS25 L01-L03 — transformers intro, attention, self-supervised learning |
| research-071 | done | CS25 L04-L06 — vision transformers, NLP and CV applications |
| research-072 | done | CS25 L07-L09 — transformers for biology, protein structure |
| research-073 | done | CS25 L10-L12 — large language models, GPT pretraining, emergent capabilities |
| research-074 | done | CS25 L13-L15 — multimodal transformers, cross-modal attention, embodied AI |
| research-075 | done | CS25 L16-L18 — efficient transformers, sparse attention, linear attention |
| research-076 | done | CS25 L19-L21 — instruction tuning, RLHF, alignment |
| research-077 | done | CS25 L22-L24 — code generation, reasoning, chain-of-thought |
| research-078 | done | CS25 L25-L27 — agents, tool use, retrieval-augmented generation |
| research-079 | done | CS25 L28-L30 — diffusion models and transformers, image/video generation |
| research-080 | done | CS25 L31-L33 — transformers for robotics, decision transformers, world models |
| research-081 | done | CS25 L34-L36 — long-context modeling, state space models, memory architectures |
| research-082 | done | CS25 L37-L39 — scaling laws, compute frontiers, mixture-of-experts at scale |
| research-083 | done | CS25 L40-L42 — neuroscience-inspired transformers, cognitive architectures, AI safety |
| research-084 | done | CS25 L43-L45 — healthcare AI, scientific discovery, real-world impact |
| research-085 | done | CS25 L46-L47 — transformers retrospective and open frontiers |

#### Lecture Articles: CS330 (086-092)

| ID | Status | Title |
|----|--------|-------|
| research-086 | done | CS330 L01-L03 — meta-learning intro, few-shot learning |
| research-087 | done | CS330 L04-L06 — black-box meta-learning, optimization-based, MAML |
| research-088 | done | CS330 L07-L09 — metric-based meta-learning, prototypical networks |
| research-089 | done | CS330 L10-L12 — Bayesian meta-learning, generative approaches |
| research-090 | done | CS330 L13-L15 — multi-task learning, task structure, curriculum learning |
| research-091 | done | CS330 L16-L18 — lifelong learning, continual learning, catastrophic forgetting |
| research-092 | done | CS330 L19-L20 — meta-RL, reward-conditioned meta-learning, open problems |

#### Lecture Articles: CS236 (093-098)

| ID | Status | Title |
|----|--------|-------|
| research-093 | done | CS236 L01-L03 — deep generative models intro, latent variable, autoregressive |
| research-094 | done | CS236 L04-L06 — VAEs, ELBO derivation, amortized inference |
| research-095 | done | CS236 L07-L09 — normalizing flows, exact likelihood, invertible architectures |
| research-096 | done | CS236 L10-L12 — GANs, adversarial training, variants and stability |
| research-097 | done | CS236 L13-L15 — diffusion models, score matching, DDPM |
| research-098 | done | CS236 L16 — evaluation of generative models, open problems |

#### Lecture Articles: CS230 (099-101)

| ID | Status | Title |
|----|--------|-------|
| research-099 | done | CS230 L01-L03 — deep learning overview, neural network foundations, optimization |
| research-100 | **failed** | CS230 L04-L06 — CNNs, sequence models, transformer architectures |
| research-101 | done | CS230 L07-L09 — AI strategy, full-cycle deep learning projects, career in AI |

#### Lecture Articles: CS221 (102-108)

| ID | Status | Title |
|----|--------|-------|
| research-102 | done | CS221 L01-L03 — AI overview, search algorithms, CSPs |
| research-103 | done | CS221 L04-L06 — MDPs, game playing, adversarial search |
| research-104 | done | CS221 L07-L09 — ML basics, linear classification, neural networks |
| research-105 | done | CS221 L10-L12 — backpropagation, optimization, SGD |
| research-106 | done | CS221 L13-L15 — probabilistic graphical models, Bayesian networks, HMMs |
| research-107 | done | CS221 L16-L18 — logic, knowledge representation, planning |
| research-108 | done | CS221 L19-L20 — fairness, ethics in AI, societal implications |

#### Lecture Articles: CS224U (109-113)

| ID | Status | Title |
|----|--------|-------|
| research-109 | done | CS224U L01-L03 — NLU overview, distributed word representations, sentiment |
| research-110 | done | CS224U L04-L06 — contextual representations, grounded language, NLI |
| research-111 | done | CS224U L07-L09 — semantic parsing, information extraction, QA |
| research-112 | done | CS224U L10-L12 — dialogue systems, conversational AI, pragmatics |
| research-113 | done | CS224U L13-L14 — evaluation methods, future of NLU, responsible AI |

#### Lecture Articles: CS285 (114-121)

| ID | Status | Title |
|----|--------|-------|
| research-114 | done | CS285 L01-L03 — deep RL intro, imitation learning, policy gradients |
| research-115 | done | CS285 L04-L06 — actor-critic, value function methods, Q-learning |
| research-116 | done | CS285 L07-L09 — advanced policy gradients, NPG, trust region |
| research-117 | done | CS285 L10-L12 — model-based RL, Dyna architecture, model learning |
| research-118 | done | CS285 L13-L15 — exploration, curiosity-driven learning, offline RL |
| research-119 | done | CS285 L16-L18 — inverse RL, reward learning from humans, transfer RL |
| research-120 | done | CS285 L19-L21 — meta-RL, hierarchical RL, language-conditioned RL |
| research-121 | done | CS285 L22-L23 — RL with large models, open problems, frontiers |

#### Lecture Articles: CS294-196 (122-125)

| ID | Status | Title |
|----|--------|-------|
| research-122 | done | CS294-196 L01-L03 — LLM agents overview, reasoning and planning, tool use |
| research-123 | done | CS294-196 L04-L06 — RAG, memory architectures, multi-agent systems |
| research-124 | done | CS294-196 L07-L09 — code generation agents, web browsing, evaluation |
| research-125 | done | CS294-196 L10-L12 — safety for LLM agents, enterprise agents, future |

#### Lecture Articles: 15-445 (126-134)

| ID | Status | Title |
|----|--------|-------|
| research-126 | done | 15-445 L01-L03 — relational model, advanced SQL, database storage |
| research-127 | done | 15-445 L04-L06 — buffer pools, hash tables, tree indexes |
| research-128 | done | 15-445 L07-L09 — index concurrency, sorting, join algorithms |
| research-129 | done | 15-445 L10-L12 — query execution, planning, optimization |
| research-130 | done | 15-445 L13-L15 — cost estimation, concurrency control theory, 2PL |
| research-131 | done | 15-445 L16-L18 — timestamp ordering, MVCC, logging and recovery |
| research-132 | done | 15-445 L19-L21 — ARIES recovery, distributed databases, distributed OLTP |
| research-133 | done | 15-445 L22-L24 — NewSQL systems, HTAP databases, column-store internals |
| research-134 | done | 15-445 L25-L26 — AI in databases, learned indexes, future of databases |

#### Lecture Articles: 10-414 (135-142)

| ID | Status | Title |
|----|--------|-------|
| research-135 | done | 10-414 L01-L03 — DL systems intro, ML refresher, manual neural networks |
| research-136 | done | 10-414 L04-L06 — automatic differentiation, FC networks, softmax |
| research-137 | done | 10-414 L07-L09 — NN library abstractions, backprop, weight initialization |
| research-138 | done | 10-414 L10-L12 — optimization algorithms, convolutions, hardware acceleration |
| research-139 | done | 10-414 L13-L15 — GPU programming, CUDA matmul, kernel optimization |
| research-140 | done | 10-414 L16-L18 — recurrent networks, attention, transformers implementation |
| research-141 | done | 10-414 L19-L21 — model deployment, distributed training, data parallelism |
| research-142 | done | 10-414 L22-L23 — generative models, normalizing flows, future of DL systems |

#### Lecture Articles: 6.S191 (143-145)

| ID | Status | Title |
|----|--------|-------|
| research-143 | done | 6.S191 L01-L03 — deep learning intro, RNNs, CNNs |
| research-144 | done | 6.S191 L04-L06 — deep generative models, RL, limitations |
| research-145 | done | 6.S191 L07-L09 — evidential deep learning, neurosymbolic AI, responsible AI |

#### Gems Syntheses (146-160)

Cross-lecture insight synthesis for each course.

| ID | Status | Title |
|----|--------|-------|
| research-146 | done | CS336 gems synthesis |
| research-147 | done | CS224N gems synthesis |
| research-148 | done | CS234 gems synthesis |
| research-149 | done | CS149 gems synthesis |
| research-150 | done | CS25 gems synthesis |
| research-151 | done | CS330 gems synthesis |
| research-152 | done | CS236 gems synthesis |
| research-153 | done | CS230 gems synthesis |
| research-154 | done | CS221 gems synthesis |
| research-155 | done | CS224U gems synthesis |
| research-156 | done | CS285 gems synthesis |
| research-157 | done | CS294-196 gems synthesis |
| research-158 | done | 15-445 gems synthesis |
| research-159 | done | 10-414 gems synthesis |
| research-160 | done | 6.S191 gems synthesis |

---

*Exported 2026-04-03. Ticket DB source: pratchett-os/centerpiece/tickets/tickets.db (domain=research). All 160 research tickets are now deleted from the source DB.*
