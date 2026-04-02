# 10-414/714: Deep Learning Systems

CMU's systems-first deep learning course. The 2025 public site turns the model stack into a build exercise: start with softmax regression and backprop, then keep moving downward into autodiff, library abstractions, kernels, GPUs, sequence models, transformers, large-model training, deployment, and a final project.

## Overview

The public lectures page says the schedule is tentative, that some future slides are from a previous version of the course, and that updated slides and videos are posted before each class. That makes the site the right source of truth for the current offering.

10-414/714 is not a theory-only course. The course home, lectures page, and assignments page all point at the same thing: students build Needle, a from-scratch deep learning library, while learning the systems problems that make modern deep learning expensive and fragile.

## Why This Course Matters

Inference from the lecture and assignment arc:

- It forces the full-stack view. Math, software architecture, GPU performance, and distributed training all show up in one class.
- It turns deep learning internals into an implementation problem, which is the only way the details stick.
- It covers the places where real systems fail: gradient plumbing, state management, memory traffic, kernel efficiency, and training at scale.
- It is relevant well beyond coursework. Modern model work is mostly systems work wearing model-shaped clothes.
- The final project makes students extend the framework instead of only replaying it, which is where the useful learning compounds.

## Instructor Bios

- **Tianqi Chen** - Assistant Professor in CMU's Machine Learning and Computer Science Departments. His official CMU profile says his research sits at the intersection of machine learning and systems, with interests in deep learning, knowledge transfer, and lifelong learning. The same profile credits him with TVM, XGBoost, and Apache MXNet.
- **Tim Dettmers** - Assistant Professor at CMU and Research Scientist at Ai2. His about page says he is focused on making AI accessible, that he created and maintains bitsandbytes, and that his research centers on efficient deep learning methods, open-source agents, hierarchical LLM architectures, and software that makes those methods easy to use.

## Syllabus

### Lecture Arc

- Lectures 1-3: introduction, ML refresher, softmax regression, manual neural networks, and backprop.
- Lectures 4-5: automatic differentiation and its implementation.
- Lectures 6-9: optimization, neural network abstractions, normalization/dropout, and library implementation.
- Lectures 10-14: convolutional networks, hardware acceleration, GPUs, and implementation work.
- Lectures 15-18: sequence modeling, RNNs, transformers, and autoregressive models.
- Lectures 19-23: large-model training, generative models, pretrained model customization, and deployment.
- Project week: future directions / Q&A, student project presentations, and overflow presentations.

### Assignments

- The official assignments page currently lists Homework 0 through Homework 4, plus a Homework 4 Extra for 714-only students.
- All homeworks are released as Jupyter notebooks, and the course uses mugrade for submission and autograding.
- Students also submit the final version of the code they used to solve the homework after finishing the individual problems.
- The course site frames the final project as a separate deliverable, not just a bigger homework.

### Related Local KB Notes

- [HW1: Automatic Differentiation](./assignments/HW1-automatic-differentiation.md)
- [HW2: Neural Network Library](./assignments/HW2-neural-network-library.md)
- [HW3: Sequence Models / RNN-LSTM](./assignments/HW3-sequence-models-rnn-lstm.md)
- [HW6: GPU CUDA Backend for Needle](./assignments/HW6-gpu-cuda-backend-for-needle.md)
- [HW7: Distributed Training](./assignments/HW7-distributed-training.md)

## Links

- [Official course home](https://dlsyscourse.org/)
- [Lectures page](https://dlsyscourse.org/lectures/)
- [Assignments page](https://dlsyscourse.org/assignments/)
- [Staff page](https://dlsyscourse.org/staff/)
- [Tianqi Chen CMU profile](https://www.csd.cmu.edu/people/faculty/tianqi-chen)
- [Tim Dettmers about page](https://timdettmers.com/about/)

## Sources

- Official course home: https://dlsyscourse.org/
- Official lectures page: https://dlsyscourse.org/lectures/
- Official assignments page: https://dlsyscourse.org/assignments/
- Official staff page: https://dlsyscourse.org/staff/
- Tianqi Chen CMU profile: https://www.csd.cmu.edu/people/faculty/tianqi-chen
- Tim Dettmers about page: https://timdettmers.com/about/
- Local KB note: /Users/otonashi/thinking/building/cs-courses-kb/10-414/assignments/HW1-automatic-differentiation.md
- Local KB note: /Users/otonashi/thinking/building/cs-courses-kb/10-414/assignments/HW2-neural-network-library.md
- Local KB note: /Users/otonashi/thinking/building/cs-courses-kb/10-414/assignments/HW3-sequence-models-rnn-lstm.md
- Local KB note: /Users/otonashi/thinking/building/cs-courses-kb/10-414/assignments/HW6-gpu-cuda-backend-for-needle.md
- Local KB note: /Users/otonashi/thinking/building/cs-courses-kb/10-414/assignments/HW7-distributed-training.md
