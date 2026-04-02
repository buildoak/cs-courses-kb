# CS25: Transformers United

Stanford's public transformer seminar. This KB page covers the V1-V5 archive, where the course grows from the original transformer story into agents, alignment, multimodality, interpretability, and domain applications.

## Quick Facts

- **Course:** CS25: Transformers United
- **School:** Stanford
- **Coverage in this KB:** V1 (Fall 2021) through V5 (Spring 2025)
- **Format:** public seminar; audit and livestream are open to everyone, enrollment is only for Stanford credit
- **Series size:** 47+ lecture slots across the five public versions
- **Public playlist:** [CS25 Transformers United playlist](https://www.youtube.com/playlist?list=PLoROMvodv4rNiJRchCzutFw5ItR_Z27CM)
- **Official archive pages:** [V1](https://web.stanford.edu/class/cs25/past/cs25-v1/), [V2](https://web.stanford.edu/class/cs25/past/cs25-v2/), [V3](https://web.stanford.edu/class/cs25/past/cs25-v3/), [V4](https://web.stanford.edu/class/cs25/past/cs25-v4/), [V5](https://web.stanford.edu/class/cs25/past/cs25-v5/)

## Overview

CS25 is not a normal lecture course. It is a rolling public seminar: one week you get architecture, the next week alignment, then agents, interpretability, or biology. The course works because the field itself moved that way. Each version is a timestamped snapshot of what transformers could do at the time.

The useful part is not just the speaker list. It is the chronology. V1 reads like transformers becoming a general substrate. V2 turns into scaling, alignment, and cross-domain transfer. V3 leans into post-ChatGPT open-model work and agentic systems. V4 makes the seminar more public and more polished. V5 pushes into reasoning, multimodality, drug discovery, diffusion, and video generation.

## Syllabus

### Version Arc

| Version | Term | Focus | Representative talks |
|---|---|---|---|
| V1 | Fall 2021 | Core transformer mechanics across NLP, vision, RL, MoE, Perceiver, interpretability, and audio | Transformers in Language; Applications in Vision; Decision Transformer; Scaling Transformers; GLOM |
| V2 | Winter 2023 | Scaling, alignment, strategic reasoning, robotics, commonsense, and biology | Introduction to Transformers; Language and Human Alignment; Emergent Abilities and Scaling in LLMs; Strategic Games; Biomedical Transformers |
| V3 | Autumn 2023 | Llama 2, embodied intelligence, helpful chatbots, NLLB, agents, and retrieval augmentation | Llama 2; Low-level Embodied Intelligence; Generalist Agents; Recipe for Training Helpful Chatbots; Retrieval Augmented Language Models |
| V4 | Spring 2024 | Open-model alignment, MoE, multimodal models, pretraining, and new training objectives | Overview of Transformers; Aligning Open Language Models; Demystifying Mixtral; From Large Language Models to Large Multimodal Models; StarCoder Use Case |
| V5 | Spring 2025 | Agents, reasoning, mechanistic interpretability, multimodal science, and generative media | RL as a Co-Design of Product and Research; The Advent of AGI; Large Language Model Reasoning; On the Biology of a Large Language Model; Transformers for Video Generation |

### Lecture Themes

- Transformers start as a language model architecture, then expand into vision, RL, MoE, and Perceiver-style arbitrary I/O.
- The middle versions put scaling, alignment, and human feedback front and center.
- Later versions treat transformers as the backbone for agents, tool use, multimodal perception, interpretability, and scientific applications.
- The course never stays in one lane. That is the point.

## Instructor Bios

### V5 teaching team

- **Steven Feng** - fourth-year Stanford CS PhD student in the Stanford AI Lab and Stanford NLP Group. His research focuses on aligning foundation models and human capabilities, especially reasoning, generalization, efficiency, and multimodal generation.
- **Karan Singh** - third-year Stanford electrical engineering PhD student and NSF Graduate Research Fellow in the Stanford Translational AI Lab. He works on applied ML, including foundation models for fMRI and brain-network analysis, and previously used ML for transcranial ultrasound neuromodulation.
- **Chelsea Zou** - Stanford MS Symbolic Systems student advised by Noah Goodman. She works in the CoCo Lab / Stanford AI Lab on hallucination guardrails and frameworks for better LLM reasoning.
- **Jenny Duan** - Stanford graduate who started Clair Health with Abhinav Agarwal. Stanford Daily identifies Clair as a Stanford-founded startup focused on privacy-first, continuous hormone monitoring.
- **Div Garg** - on leave from a Stanford CS PhD, founder of MultiOn, and the original creator of CS25. His work centers on general-purpose AI agents, reinforcement learning, robotics, and real-world deployment.
- **Christopher Manning** - Thomas M. Siebel Professor in Machine Learning, Professor of Linguistics and Computer Science, and Senior Fellow at Stanford HAI. He founded the Stanford NLP Group and is one of the central figures in Stanford NLP and transformer-era teaching.

## Links

- [Current public home (V6)](https://web.stanford.edu/class/cs25/)
- [CS25 FAQ](https://web.stanford.edu/class/cs25/faq/)
- [CS25 recordings](https://web.stanford.edu/class/cs25/recordings/)
- [V1 archive](https://web.stanford.edu/class/cs25/past/cs25-v1/)
- [V2 archive](https://web.stanford.edu/class/cs25/past/cs25-v2/)
- [V3 archive](https://web.stanford.edu/class/cs25/past/cs25-v3/)
- [V4 archive](https://web.stanford.edu/class/cs25/past/cs25-v4/)
- [V5 archive](https://web.stanford.edu/class/cs25/past/cs25-v5/)
- [Public YouTube playlist](https://www.youtube.com/playlist?list=PLoROMvodv4rNiJRchCzutFw5ItR_Z27CM)
- [Steven Feng bio](https://styfeng.github.io/)
- [Karan Singh bio](https://karanps.com/)
- [Chelsea Zou bio](https://bosonphoton.github.io/)
- [Jenny Duan source](https://stanforddaily.com/2026/02/05/stanford-founded-startup-develops-wearable-for-continuous-hormone-monitoring/)
- [Div Garg bio](https://divyanshgarg.com/)
- [Christopher Manning bio](https://nlp.stanford.edu/~manning/)

## Why It Matters

Inference from the official archive:

- It is one of the cleanest public timelines of how transformers escaped NLP and became a general-purpose substrate.
- It captures the field's shift from architecture to post-training, agents, multimodality, interpretability, and scientific use cases.
- The speaker list is a useful map of the current transformer ecosystem across Stanford, OpenAI, DeepMind, Anthropic, Meta, Google, Hugging Face, Mistral, and AI2.
- The public format makes the course useful as both a syllabus and a discovery index for adjacent papers, talks, and teams.

## Source Notes

- Official Stanford archive pages: V1-V5, FAQ, and recordings.
- Public playlist fetch: `yt-dlp --flat-playlist --print '%(playlist_index)02d %(title)s | %(webpage_url)s' 'https://www.youtube.com/playlist?list=PLoROMvodv4rNiJRchCzutFw5ItR_Z27CM'`
- Playlist artifact: `/tmp/agent-mux-501/01KN7SE9R5RFQ33FQB7D36XR77/cs25-playlist.txt`
- Local KB catalog: `/Users/otonashi/thinking/building/cs-courses-kb/README.md` lists CS25 as `2021-25`, `47+`, `Transformers United (seminar)`.
- Bio sources: Steven Feng, Karan Singh, Chelsea Zou, Jenny Duan, Div Garg, and Christopher Manning pages linked above.
