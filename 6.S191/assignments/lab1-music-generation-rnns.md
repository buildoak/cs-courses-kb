# 6.S191 Lab 1: Music Generation with RNNs

## Problem Statement
Lab 1 turns sheet music into a sequence modeling problem. The model sees raw Irish folk songs in ABC notation and learns to predict the next character in the stream, then uses those predictions to generate new music that was not in the training set.

The useful constraint is that this is not "classify the song." It is "model the sequence." The notebook starts with a PyTorch warm-up, then pushes into a character-level recurrent model that has to hold enough state to keep musical structure alive across many timesteps.

## Key Techniques
- **Character-level tokenization.** The dataset is turned into integer indices with lookup tables in both directions.
- **Shifted input-target pairs.** Each input sequence is paired with the same text shifted one character to the right so the model learns next-character prediction.
- **Embedding + LSTM + linear head.** The model maps character IDs into embeddings, processes them with an LSTM, then projects to vocabulary-sized logits.
- **Stateful sequence modeling.** The hidden state carries context forward so the model can remember earlier notes, bars, and motifs.
- **Cross-entropy training.** The task is framed as categorical prediction with `CrossEntropyLoss`.
- **Multinomial sampling.** Generation samples from the softmax distribution instead of taking argmax, which avoids repetitive loops.

## Implementation Walkthrough
1. Load the Irish folk song corpus in ABC notation and inspect the raw text.
2. Build `char2idx` and `idx2char` maps so every character becomes a model-friendly integer.
3. Split the corpus into fixed-length sequences and create one-step-shifted targets.
4. Define the RNN as `nn.Embedding -> nn.LSTM -> nn.Linear`.
5. Run the untrained model once. The output is nonsense, which is the point.
6. Train with cross-entropy, `loss.backward()`, and `optimizer.step()`.
7. Seed generation with a short start string, then iterate character by character using multinomial sampling from the softmax output.
8. Decode the sampled characters back into ABC notation and play the generated song.

## What You Learn
- Sequence models are pattern compressors with memory.
- The encode/decode layer matters as much as the network.
- Sampling strategy changes the output quality more than people expect.
- Music generation is not magic. It is next-token prediction with enough state to make the next token feel like a melody.

## Sources
- `https://github.com/MITDeepLearning/introtodeeplearning/blob/master/README.md`
- `https://github.com/MITDeepLearning/introtodeeplearning/blob/master/lab1/README.md`
- `https://github.com/MITDeepLearning/introtodeeplearning/blob/master/lab1/PT_Part1_Intro.ipynb`
- `https://github.com/MITDeepLearning/introtodeeplearning/blob/master/lab1/PT_Part2_Music_Generation.ipynb`
- `/tmp/introtodeeplearning/README.md`
- `/tmp/introtodeeplearning/lab1/README.md`
- `/tmp/introtodeeplearning/lab1/PT_Part1_Intro.ipynb`
- `/tmp/introtodeeplearning/lab1/PT_Part2_Music_Generation.ipynb`
