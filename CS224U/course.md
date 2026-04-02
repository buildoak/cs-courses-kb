# CS224U

Stanford's natural language understanding course, archived here from Spring 2023. The public course page says it can be taken entirely online and asynchronously, with recorded meetings and core content delivered via slides, videos, and Python notebooks. The YouTube archive runs to 50 entries and mirrors the course arc.

## Quick Facts

- **Course:** CS224U: Natural Language Understanding
- **Stanford online label:** XCS224U
- **Term used for this KB page:** Spring 2023
- **Instructor:** Christopher Potts
- **Teaching team:** Christopher Potts, Kawin Ethayarajh, Sidd Karamcheti, Mina Lee, Siyan Li, Xiang (Lisa) Li, Tolúlope Ògúnremí, Tianyi Zhang
- **Format:** online/asynchronous, recorded lectures, slides/videos/Python notebooks
- **Meeting slot:** MW 3:00-4:20 pm, Gates B1
- **Official course page:** [Stanford CS224U Spring 2023](https://web.stanford.edu/class/cs224u/)
- **Public playlist:** [Stanford XCS224U Spring 2023](https://www.youtube.com/playlist?list=PLoROMvodv4rOwvldxftJTmoR3kRcWkJBp)
- **Playlist size:** 50 entries
- **Supporting materials:** [Projects](https://web.stanford.edu/class/cs224u/projects.html), [Background materials](https://web.stanford.edu/class/cs224u/background.html)

## Syllabus

### Foundations

- Course introduction and overview
- Course set-up notebook
- Optional background materials on understanding and foundation models

### Supervised sentiment and adaptation

- Domain adaptation for supervised sentiment
- Bake-off-style homework framing
- NLU as a measurement problem, not just a modeling problem

### Contextual word representations

- Transformers
- Positional encoding
- GPT
- BERT
- RoBERTa
- ELECTRA
- Seq2seq architectures
- Distillation
- Diffusion objectives for text
- Fantastic language models and how to build them

### Retrieval and in-context learning

- Retrieval augmented in-context learning
- Classical IR ideas carried into modern NLU
- Neural IR as the learned retrieval counterpart

### Behavioral evaluation

- Behavioral evaluation of NLU models
- Analytical considerations
- Compositionality
- COGS and ReCOGS
- Adversarial testing
- Adversarial NLI
- DynaSent

### Analysis methods

- Probing
- Feature attribution
- Causal abstraction
- Interchange Intervention Training
- Distributed Alignment Search
- Circuits

### Metrics and experimental discipline

- Experiment protocol overview
- NLP methods and metrics
- Classifier metrics
- Generation metrics
- Datasets
- Data organization
- Model evaluation
- Real-world NLP assessments

### Research communication

- Presenting your research
- Choosing papers
- Writing NLP papers
- Conference submission
- Giving talks

### Assessment snapshot

- Bake-offs and quizzes for the early and middle course sections
- Lit review
- Experimental protocol
- Final paper

## Instructor Bios

### Christopher Potts

Christopher Potts is Professor of Linguistics at Stanford, chair of the Linguistics Department, a faculty affiliate of HAI, and a Bio-X member. His work spans semantics, pragmatics, language understanding, and machine learning. CS224U is the public teaching version of that bridge: linguistics first, but with the engineering and evaluation discipline visible.

### Kawin Ethayarajh

Kawin Ethayarajh appears on the 2023 teaching team and is the named staff voice on the real-world NLP assessment session. He later completed a Stanford PhD in Computer Science and moved into applied AI research, with work centered on behavior-bound machine learning.

## Links

- [Official course page](https://web.stanford.edu/class/cs224u/)
- [Schedule](https://web.stanford.edu/class/cs224u/index.html#schedule)
- [Projects page](https://web.stanford.edu/class/cs224u/projects.html)
- [Background materials](https://web.stanford.edu/class/cs224u/background.html)
- [Public playlist](https://www.youtube.com/playlist?list=PLoROMvodv4rOwvldxftJTmoR3kRcWkJBp)
- [Christopher Potts profile](https://profiles.stanford.edu/christopher-potts)
- [Kawin Ethayarajh website](https://kawine.github.io/)

## Why It Matters

Inference from the schedule and playlist:

- It gives a compact map from classical NLU to the transformer-and-retrieval era.
- It treats understanding as something you test, not something you assert.
- It covers the practical work around LLM-era systems: contextual representations, retrieval, in-context learning, metrics, and failure analysis.
- It makes research communication part of the course, not a separate afterthought.
- The public archive is unusually complete: lectures, homework overviews, and research-writing sessions are all there.

## Source Notes

- Stanford CS224U course page and schedule: https://web.stanford.edu/class/cs224u/
- Stanford Profiles bio for Christopher Potts: https://profiles.stanford.edu/christopher-potts
- Kawin Ethayarajh bio and Stanford NLP people references: https://www.web.stanford.edu/~jurafsky/people.html and https://kawine.github.io/
- Stanford XCS224U Spring 2023 playlist metadata from `yt-dlp --flat-playlist --dump-single-json "https://www.youtube.com/playlist?list=PLoROMvodv4rOwvldxftJTmoR3kRcWkJBp" | jq -r '.entries[].title'` on 2026-04-02
