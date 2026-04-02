# CS336 Assignment 1: Byte-Pair Tokenizer

## Problem Statement
Assignment 1 starts at the bottom of the stack: turn raw text into token IDs without hiding behind an existing tokenizer library. The handout makes the scope explicit: understand Unicode and UTF-8, train a byte-level BPE tokenizer with GPT-2 style pre-tokenization, implement encode/decode with special-token handling, and make the whole thing fast enough to be practical on real corpora.

The important constraint is that this is not "just write a parser." The tokenizer has to satisfy three contracts at once: correctness on arbitrary Unicode, compatibility with a reference GPT-2 style tokenizer behavior, and enough efficiency that BPE training does not become the bottleneck.

## Key Techniques
- **Byte-level vocabulary.** The tokenizer starts from the 256 possible byte values, not Unicode code points or words. This guarantees coverage for arbitrary input text and avoids out-of-vocabulary failures.
- **Regex pre-tokenization.** Before running BPE merges, the assignment uses the GPT-2 regex pre-tokenizer so pair counts happen inside coarse text fragments instead of across arbitrary boundaries.
- **Frequency-driven merging.** BPE repeatedly counts adjacent token pairs inside pre-tokens, merges the most frequent pair, and appends that merged token to the vocabulary. Ties are broken deterministically by preferring the lexicographically greater pair.
- **Special-token preservation.** Strings like `<|endoftext|>` must stay atomic during encoding even if their byte contents would otherwise be splittable.
- **Reference-level validation.** The tests do not just check round-tripping. They compare behavior against GPT-2 merges and vocab artifacts, cover ASCII and Unicode cases, and enforce a speed budget for `train_bpe`.

## Implementation Walkthrough
1. **Normalize the mental model around bytes.** The handout spends time on `chr(0)`, UTF-8 byte sequences, and why code-point-level tokenization is the wrong abstraction for scalable LMs. That is groundwork for the actual implementation: every later step operates on byte sequences, not Python strings.
2. **Train BPE on pre-token counts, not raw-text rescans.** The efficient implementation aggregates repeated pre-tokens, counts adjacent byte pairs within those sequences, and updates merges iteratively. The provided `test_train_bpe_speed` exists specifically to push students away from the naive "rescan the whole corpus every merge" implementation.
3. **Build vocab plus merge table together.** The output of training is two coupled artifacts: a vocabulary mapping token IDs to byte strings and an ordered merge list that defines merge priority. Those artifacts are then reused by the runtime tokenizer.
4. **Encode by applying merge rules greedily inside pre-tokens.** At inference time, the tokenizer splits around special tokens, regex-pre-tokenizes the remaining text, converts each fragment to bytes, and repeatedly applies the highest-priority valid merges until no merge remains.
5. **Decode by concatenating token bytes and UTF-8 decoding the result.** The round-trip tests cover empty strings, ASCII, accented text, and emoji. If decode is wrong, the implementation usually broke either special-token handling or byte reconstruction.
6. **Treat compatibility and memory use as first-class concerns.** The tokenizer tests compare outputs with `tiktoken` for GPT-2 assets and apply memory limits during some encode paths, which means the implementation has to be clean in both behavior and data structures.

## What You Learn
- Tokenization is compression engineering, not a cosmetic preprocessing step.
- Byte-level BPE is a compromise: fixed base vocabulary, no OOVs, shorter sequences than pure bytes, and still reversible.
- Most tokenizer bugs are not about merge math. They come from Unicode boundaries, special tokens, or inconsistent encode/decode state.
- "Fast enough" is part of correctness when the tokenizer sits on the training hot path.

## Sources
- Handout: `https://github.com/stanford-cs336/spring2024-assignment1-basics/blob/main/cs336_spring2024_assignment1_basics.pdf` (Assignment 1, sections 2.1-2.7)
- Repo overview: `https://github.com/stanford-cs336/spring2024-assignment1-basics`
- Adapter contract: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a1/tests/adapters.py`
- BPE training tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a1/tests/test_train_bpe.py`
- Tokenizer behavior tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-008-a1/tests/test_tokenizer.py`
