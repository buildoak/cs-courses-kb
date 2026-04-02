# CS236: Deep Generative Models

Stanford's Fall 2023 generative-models course. The public archive includes 18 lecture videos on YouTube, self-contained course notes, three homework assignments, and a project track that ends in a poster session and final report.

## Overview

CS236 treats deep generative modeling as a set of mathematical tradeoffs, not a grab bag of model families. The course starts from probabilistic foundations and learning algorithms, then moves through autoregressive models, maximum likelihood, VAEs, normalizing flows, GANs, energy-based models, score-based models, evaluation, and diffusion.

The course site frames the applications broadly: computer vision, speech, natural language processing, graph mining, reinforcement learning, reliable machine learning, and inverse problems. That is the right scope. Generative modeling is not only about images. It is about building probability models that still work when the data is high-dimensional, structured, and ugly.

## Syllabus

### Public lecture list

| # | Lecture |
|---|---|
| 1 | Introduction |
| 2 | Background |
| 3 | Autoregressive Models |
| 4 | Maximum Likelihood Learning |
| 5 | VAEs |
| 6 | VAEs |
| 7 | Normalizing Flows |
| 8 | Normalizing Flows |
| 9 | GANs |
| 10 | GANs |
| 11 | Energy Based Models |
| 12 | Energy Based Models |
| 13 | Score Based Models |
| 14 | Energy Based Models |
| 15 | Evaluation of Generative Models |
| 16 | Score Based Diffusion Models |
| 17 | Discrete Latent Variable Models |
| 18 | Diffusion Models for Discrete Data |

### Course arc

- Lectures 1-4: introduction, background, autoregressive models, and maximum likelihood.
- Lectures 5-6: variational autoencoders.
- Lectures 7-8: normalizing flows.
- Lectures 9-10: GANs.
- Lectures 11-14: energy-based models and score-based models.
- Lecture 15: evaluation of generative models.
- Lectures 16-18: diffusion, discrete latent variables, and diffusion for discrete data.

### Assessment

- **Three homeworks:** 15% each.
- **Midterm:** 15%.
- **Course project:** 40% total.
  - Proposal: 5%
  - Progress report: 10%
  - Poster presentation: 10%
  - Final report: 15%

### Assignments

- [Homework 1: Variational Autoencoders](assignments/A1-variational-autoencoders.md)
- [Homework 2: Normalizing Flows](assignments/A2-normalizing-flows.md)
- [Homework 3: GANs and Diffusion](assignments/A3-gan-diffusion.md)

The project page says groups can work on novel applications, algorithmic improvements, or theoretical analysis. That is the useful norm: the project is about substance, not just a final demo.

## Instructor Bio

- **Stefano Ermon** - Stanford Profiles lists him as Associate Professor of Computer Science and Senior Fellow at the Woods Institute for the Environment. His bio says his research centers on scalable and accurate inference in graphical models, statistical modeling of data, large-scale combinatorial optimization, and robust decision making under uncertainty, with applications including computational sustainability.

## Why This Course Matters

- It covers the main modern generative families in one coherent arc instead of treating them as disconnected tricks.
- It makes the core tension explicit: exact likelihood, approximate inference, and sample quality do not all optimize the same way.
- It moves from VAEs and flows into GANs, energy-based models, and diffusion, which is the actual historical shape of the field.
- It has a proper project spine, so the course can produce research-like work instead of just homework completion.
- The public notes and playlist make it a usable self-study resource, not just a classroom artifact.

## Links

- [Official course site](http://cs236.stanford.edu/)
- [Course homepage mirror](https://deepgenerativemodels.github.io/)
- [Syllabus](https://deepgenerativemodels.github.io/syllabus.html)
- [Assignments](https://deepgenerativemodels.github.io/assignments.html)
- [Project](https://deepgenerativemodels.github.io/project.html)
- [Course notes](https://deepgenerativemodels.github.io/notes/index.html)
- [Stanford Online CS236 playlist](https://www.youtube.com/playlist?list=PLoROMvodv4rPOWA-omMM6STXaWW4FvJT8)
- [Stefano Ermon Stanford profile](https://profiles.stanford.edu/stefano-ermon)
- [Homework 1 note](assignments/A1-variational-autoencoders.md)
- [Homework 2 note](assignments/A2-normalizing-flows.md)
- [Homework 3 note](assignments/A3-gan-diffusion.md)

## Sources

- Official course site: http://cs236.stanford.edu/
- Course homepage mirror: https://deepgenerativemodels.github.io/
- Syllabus page: https://deepgenerativemodels.github.io/syllabus.html
- Assignments page: https://deepgenerativemodels.github.io/assignments.html
- Project page: https://deepgenerativemodels.github.io/project.html
- Course notes index: https://deepgenerativemodels.github.io/notes/index.html
- Stefano Ermon profile: https://profiles.stanford.edu/stefano-ermon
- Stanford Online playlists page used to resolve the CS236 playlist: https://www.youtube.com/@StanfordOnline/playlists
- Public CS236 playlist: https://www.youtube.com/playlist?list=PLoROMvodv4rPOWA-omMM6STXaWW4FvJT8
- Local assignment note: /Users/otonashi/thinking/building/cs-courses-kb/CS236/assignments/A1-variational-autoencoders.md
- Local assignment note: /Users/otonashi/thinking/building/cs-courses-kb/CS236/assignments/A2-normalizing-flows.md
- Local assignment note: /Users/otonashi/thinking/building/cs-courses-kb/CS236/assignments/A3-gan-diffusion.md
