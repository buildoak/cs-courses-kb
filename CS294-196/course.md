# CS294-196: Large Language Model Agents

UC Berkeley, Fall 2024. The archived course page titles the class `CS294/194-196 Large Language Model Agents`, with separate CalCentral numbers for CS194-196 and CS294-196. The public archive is unusually complete: syllabus, readings, slides, recordings, edited videos, grading policy, and office-hour details are all on one page.

## Overview

This course treats LLM agents as a systems problem, not a prompt-engineering trick. The syllabus starts with reasoning and agent history, then moves through tool use, agent frameworks, retrieval, code generation, enterprise workflows, robotics, evaluation, safety, human-agent interaction, and multi-agent collaboration.

The course page makes the operating model explicit:

- Monday lecture, 3-5pm PT, Latimer 120
- Variable-unit enrollment
- Weekly reading summaries due Sunday 11:59pm before lecture
- 1-unit students write an article
- 2-unit students do a lab and written report
- 3/4-unit students add a substantial implementation project
- Prerequisite: prior ML and deep learning exposure

That is a serious workload. It is also the right shape for the topic.

## Syllabus

The archived page lists 12 Monday slots, including one no-class holiday.

### Lecture Arc

1. **Sept 9 - LLM Reasoning** - Denny Zhou, Google DeepMind
2. **Sept 16 - LLM agents: brief history and overview** - Shunyu Yao, OpenAI
3. **Sept 23 - Agentic AI Frameworks & AutoGen / Building a Multimodal Knowledge Assistant** - Chi Wang, AutoGen-AI; Jerry Liu, LlamaIndex
4. **Sept 30 - Enterprise trends for generative AI, and key components of building successful agents/applications** - Burak Gokturk, Google
5. **Oct 7 - Compound AI Systems & the DSPy Framework** - Omar Khattab, Databricks
6. **Oct 14 - Agents for Software Development** - Graham Neubig, Carnegie Mellon University
7. **Oct 21 - AI Agents for Enterprise Workflows** - Nicolas Chapados, ServiceNow
8. **Oct 28 - Towards a unified framework of Neural and Symbolic Decision Making** - Yuandong Tian, Meta AI (FAIR)
9. **Nov 4 - Project GR00T: A Blueprint for Generalist Robotics** - Jim Fan, NVIDIA
10. **Nov 11 - No Class** - Veterans Day
11. **Nov 18 - Open-Source and Science in the Era of Foundation Models** - Percy Liang, Stanford University
12. **Nov 25 - Measuring Agent capabilities and Anthropic’s RSP** - Ben Mann, Anthropic
13. **Dec 2 - Towards Building Safe & Trustworthy AI Agents and a Path for Science- and Evidence-based AI Policy** - Dawn Song, UC Berkeley

The reading list is a mix of papers, vendor docs, and product/blog artifacts. That matters: the course is not just theory. It is trying to connect the research stack to the actual systems stack.

### Arc Summary

- Reasoning comes first, because everything else depends on it.
- Frameworks and orchestration follow, because agents need a runtime.
- Software, enterprise, and multimodal applications show where the stack gets real.
- The last third of the course pushes into robotics, open-source science, capability measurement, and safety/policy.

## Instructor Bios

### Dawn Song

The course page labels Dawn Song as the guest instructor. Berkeley’s faculty page says she is a Professor in Computer Science at UC Berkeley and Co-Director of Berkeley Center for Responsible Decentralized Intelligence. Her research areas include AI safety and security, Agentic AI, deep learning, security and privacy, and decentralization technology.

That makes her a natural fit for a course on agents: she sits at the intersection of systems, security, and AI, which is exactly where agent failures become expensive.

### Xinyun Chen

The course page labels Xinyun Chen as co-instructor and identifies her as a Research Scientist at Google DeepMind. Her personal homepage says she is now an AI research scientist at Meta Superintelligence Labs, previously at Google DeepMind, with a UC Berkeley PhD and research focused on code generation, reasoning, and AI safety.

That background fits the class well. The course is about agent capability, but also about the methods that make capability measurable and the failure modes that appear once models start acting.

## Links

- [Course archive](https://rdi.berkeley.edu/llm-agents/f24)
- [Dawn Song faculty page](https://www2.eecs.berkeley.edu/Faculty/Homepages/song.html)
- [Xinyun Chen homepage](https://jungyhuk.github.io/)

The course archive also links the signup form, EdStem, slides, original recordings, edited videos, and readings for each lecture.

## Why This Course Matters

Inference from the syllabus:

- It is one of the cleanest public maps of what "LLM agents" actually means.
- It treats the problem as a stack: reasoning, planning, tools, infrastructure, evaluation, safety, and coordination.
- It covers the failure surfaces that matter in practice: reliability, measurement, privacy, governance, and multi-agent behavior.
- It is useful for anyone building agent products or agent infrastructure because it connects research vocabulary to implementation reality.
- It is a good bridge from "models that answer" to "systems that act."

## Sources

- UC Berkeley course archive: https://rdi.berkeley.edu/llm-agents/f24
- Dawn Song faculty page: https://www2.eecs.berkeley.edu/Faculty/Homepages/song.html
- Xinyun Chen homepage: https://jungyhuk.github.io/
