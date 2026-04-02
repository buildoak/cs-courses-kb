# CS224N Assignment 3: Neural Machine Translation with Attention

## Problem Statement
Assignment 3 is the public Spring 2024 CS224N machine translation build. You implement a full sequence-to-sequence NMT system that translates Mandarin Chinese into English with a bidirectional LSTM encoder, a unidirectional LSTM decoder, multiplicative attention, and beam-search decoding.

The assignment is split in two. The coding half is the model build itself: embeddings, convolution over source embeddings, encoder/decoder state plumbing, attention, training, and decoding. The written half asks you to analyze attention variants, BLEU, beam-search behavior, and translation errors, so the goal is not just "make it run" but "understand why it behaves that way."

## Key Techniques
- **Seq2Seq with global multiplicative attention.** Encoder states are projected once, then scored against the decoder hidden state at each timestep.
- **Bidirectional source encoding.** Source tokens are embedded, passed through a small convolution, and then encoded with a BiLSTM so each source position carries left and right context.
- **Input feeding in the decoder.** The previous combined output vector is concatenated with the current target embedding before each decoder step.
- **State projection between encoder and decoder.** The final forward/backward encoder states are concatenated and linearly projected into the decoder's initial hidden and cell states.
- **Mask-aware attention.** Padding positions in the source are masked before softmax so the decoder cannot attend to fake timesteps.
- **Beam search plus BLEU evaluation.** The assignment does not stop at greedy decoding; it explicitly asks you to inspect beam-search outputs and reason about BLEU as an imperfect automatic metric.

## Implementation Walkthrough
1. **Build language-specific embedding layers.** The reference solution starts with separate source and target embeddings keyed by the source and target vocabularies, both honoring the pad index so padded timesteps do not contribute useful signal.
2. **Encode the source sentence before any decoding happens.** In the public solution, source embeddings are passed through a `Conv1d` layer with kernel size 2 and then through a bidirectional LSTM. The packed-sequence path keeps the recurrent work aligned with true sentence lengths instead of padded length.
3. **Project encoder end states into decoder start states.** The final forward and backward hidden states are concatenated into a `2h` vector and then mapped down to `h` with separate linear projections for hidden state and cell state. That is the bridge from bidirectional encoder space into unidirectional decoder space.
4. **Run attention at every decoder step.** The decoder consumes the current target embedding plus the previous combined output vector. After the LSTMCell update, the decoder hidden state is scored against projected encoder states, masked, softmaxed, and used to form a weighted source context vector.
5. **Fuse context with decoder state into a combined output.** The context vector and decoder hidden state are concatenated, projected, passed through `tanh`, and dropped out. That combined output is what the model uses for target-vocabulary prediction.
6. **Train with sequence log-likelihood and decode with beam search.** The training script computes token log-probabilities, sums gold-word scores, clips gradients, tracks dev perplexity, and checkpoints the best model. At decode time, beam search keeps multiple partial translations alive and the script computes corpus BLEU with `sacrebleu`.

## What You Learn
- Attention is the practical fix for the encoder bottleneck. Instead of compressing the whole source into one vector, the decoder learns where to look at each step.
- Sequence models are mostly interface work: padding, packing, masking, shape transforms, and state projection are as important as the LSTM equations.
- Input feeding is not cosmetic. It gives the decoder direct access to its previous attention-informed output and stabilizes alignment behavior.
- BLEU and beam search are not afterthoughts. They determine how you inspect model quality and where automatic evaluation can mislead you.

## Sources
- Official assignment archive: `https://web.stanford.edu/class/cs224n/assignments/`
- Official handout: `https://web.stanford.edu/class/cs224n/assignments/a3_spr24_student_handout.pdf`
- Public GitHub solution repo: `https://github.com/Yousefbahr/Stanford-CS224N-NLP-2025/tree/main/a3`
- Embedding implementation: `https://raw.githubusercontent.com/Yousefbahr/Stanford-CS224N-NLP-2025/main/a3/model_embeddings.py`
- Encoder/decoder/attention implementation: `https://raw.githubusercontent.com/Yousefbahr/Stanford-CS224N-NLP-2025/main/a3/nmt_model.py`
- Training and decoding script: `https://raw.githubusercontent.com/Yousefbahr/Stanford-CS224N-NLP-2025/main/a3/run.py`
