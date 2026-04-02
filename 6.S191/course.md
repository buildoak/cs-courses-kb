# 6.S191: Introduction to Deep Learning

MIT’s bootcamp-style intro to deep learning. The 2026 edition runs weekly on Mondays at 10am ET from Mar. 30 through May 25, with open-source lectures, three software labs, and a final project proposal competition. The public YouTube playlist is a rolling archive, not a single semester snapshot: the current 2026 run sits at the top, then older editions follow.

## Quick Facts

- **Course:** 6.S191: Introduction to Deep Learning
- **School:** MIT
- **Edition used here:** 2026 course site plus the provided public playlist archive
- **Lead instructors:** Alexander Amini, Ava Amini
- **Faculty sponsor:** Daniela Rus
- **Format:** weekly lecture-and-lab bootcamp
- **Dates:** Mon. Mar. 30 - Mon. May 25, 2026
- **Time:** Every Monday at 10am ET
- **Grading:** P/D/F based on completion of the project proposal assignment
- **Prereqs:** elementary calculus and linear algebra; Python helps but is not required
- **Materials:** open-sourced under the MIT license

## Course Mechanics

- The course calls itself an efficient, high-intensity bootcamp.
- It is framed around applications in vision, robotics, medicine, language, game play, art, and more.
- Students get practical experience building neural networks, not just reading about them.
- The site explicitly says the class covers large language models and generative AI.
- Listeners are welcome.
- The program ends with a project proposal competition with feedback from staff and industry sponsors.

## Syllabus

### Lecture Arc

1. **Intro to Deep Learning** - Mar. 30, 2026
2. **Deep Sequence Modeling** - Apr. 6, 2026
3. **Deep Computer Vision** - Apr. 13, 2026
4. **Deep Generative Modeling** - Apr. 20, 2026
5. **Deep Reinforcement Learning** - Apr. 27, 2026
6. **New Frontiers** - May 4, 2026
7. **AI for Science** - May 11, 2026
8. **Secrets to Massively Parallel Training** - May 18, 2026
9. **The Three Laws of AI** - May 25, 2026

### Labs

- **Lab 1:** Deep Learning in Python; Music Generation
- **Lab 2:** Facial Detection Systems
- **Lab 3:** Fine-Tune an LLM, You Must!

### Capstone

- Final project work
- Project presentations and pitch session

### Related KB Notes

These are older, conceptually adjacent notes in this repo. They are not one-for-one mirrors of the 2026 lab titles.

- [Lab 1 note: Music Generation with RNNs](./assignments/lab1-music-generation-rnns.md)
- [Lab 2 note: Facial Detection, CNNs, and Debiasing](./assignments/lab2-facial-detection-cnns.md)
- [Lab 3 note: Reinforcement Learning in Cartpole and VISTA](./assignments/lab3-reinforcement-learning.md)

## Instructor Bios

### Alexander Amini

Lead instructor and organizer. His MIT CSAIL bio says he is a postdoctoral researcher working with Daniela Rus. He completed his PhD, MS, and BS in Computer Science at MIT with a minor in Mathematics. His research focuses on safe decision making for autonomous agents, end-to-end control, confidence of neural networks, and uncertainty-aware systems. He is also the lead organizer and lecturer for MIT 6.S191.

### Ava Amini

Lead instructor and organizer. Her Microsoft Research bio says she is a Principal Researcher in Cambridge. She completed her PhD in Biophysics at Harvard and her BS in Computer Science and Molecular Biology at MIT. Her research focuses on AI methods for biology and precision medicine, with work on diagnostics, therapeutics, and Project Ex Vivo.

## Links

- [Official course page](https://introtodeeplearning.com/)
- [Public YouTube playlist](https://www.youtube.com/playlist?list=PLtBw6njQRU-rwp5__7C0oIVt26ZgjG9NI)
- [MITDeepLearning / introtodeeplearning](https://github.com/MITDeepLearning/introtodeeplearning)
- [Alexander Amini bio](https://www.csail.mit.edu/person/alexander-amini)
- [Ava Amini bio](https://www.microsoft.com/en-us/research/people/avasoleimany/)
- [Legacy note: Lab 1 - Music Generation with RNNs](./assignments/lab1-music-generation-rnns.md)
- [Legacy note: Lab 2 - Facial Detection, CNNs, and Debiasing](./assignments/lab2-facial-detection-cnns.md)
- [Legacy note: Lab 3 - Reinforcement Learning in Cartpole and VISTA](./assignments/lab3-reinforcement-learning.md)

## Why It Matters

- It is the cleanest public bootcamp map of modern deep learning: sequence models, CNNs, generative modeling, RL, LLMs, AI for science, and massive parallel training.
- The course turns the lectures into code immediately through three software labs. The repo also keeps older companion notes for music generation, face detection, and RL.
- It is beginner-friendly without being soft. The course asks for calculus and linear algebra, then moves fast.
- It is useful as a reusable curriculum backbone because the materials are open-sourced.
- The course reflects where deep learning is actually going, not where the textbook stopped.

## Sources

- Official course page: https://introtodeeplearning.com/
- Course schedule and team section: https://introtodeeplearning.com/
- Alexander Amini bio: https://www.csail.mit.edu/person/alexander-amini
- Ava Amini bio: https://www.microsoft.com/en-us/research/people/avasoleimany/
- Public playlist: https://www.youtube.com/playlist?list=PLtBw6njQRU-rwp5__7C0oIVt26ZgjG9NI
- MITDeepLearning repository: https://github.com/MITDeepLearning/introtodeeplearning
- Playlist metadata verified locally on 2026-04-02 with:
  - `yt-dlp --flat-playlist --skip-download --print '%(playlist_index)s|%(title)s|%(webpage_url)s' 'https://www.youtube.com/playlist?list=PLtBw6njQRU-rwp5__7C0oIVt26ZgjG9NI'`
