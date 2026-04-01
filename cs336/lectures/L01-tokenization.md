# L01 Tokenization

Lecture 1 starts with the course thesis, then lands on the real problem: tokenization is a messy compression scheme we keep using because it is still the least-bad option for language models.

## Summary
- The course is explicit about its motive: build language models from scratch to understand the stack, not just prompt black boxes. Source: transcript `[2:09]` to `[5:09]` in `/Users/otonashi/thinking/pratchett-os/tmp/research-001-l01-transcript.txt`.
- Tokenization is a reversible map between strings and integer sequences. The lecture walks through character, byte, and word tokenization before landing on BPE. Source: `[1:00:35]` to `[1:18:30]`.
- Byte Pair Encoding wins because it adaptively allocates vocabulary to frequent sequences while still round-tripping back to text. Source: `[1:11:02]` to `[1:17:30]`.
- The practical goal is not elegance. It is a compression ratio that makes training and inference feasible without wrecking the vocabulary. Source: `[1:04:44]` to `[1:09:16]`.
- The class assignment asks for a fast BPE tokenizer, not just a correct one. Source: `[1:17:15]` to `[1:17:44]`.

## Key Concepts
- **Tokenization as a contract.** Strings go in, token IDs come out, and decoding must reconstruct the original text as faithfully as the tokenizer design allows. Source: `[1:17:49]` to `[1:18:13]`.
- **Vocabulary size matters.** Character tokenization wastes slots on tiny units; byte tokenization caps the vocabulary at 256 but inflates sequence length; word tokenization explodes the vocabulary and produces too many unknowns. Source: `[1:05:12]` to `[1:10:39]`.
- **BPE is frequency-driven compression.** Start from bytes, count adjacent pairs, merge the most common pair, repeat. Source: `[1:12:46]` to `[1:16:04]`.
- **Pre-tokenization is a performance hack.** GPT-2 first splits text into coarse segments, then runs BPE on each segment. Source: `[1:12:29]` to `[1:12:43]`.
- **Special tokens exist outside the ordinary compression story.** The lecture flags them as an implementation detail that matters in real tokenizers. Source: `[1:17:23]` to `[1:17:28]`.
- **Compression ratio is the practical lens.** The lecture uses bytes-per-token as a quick sanity check and notes that OpenAI’s GPT tokenizer averages about 1.6 bytes per token. Source: `[1:04:44]` to `[1:05:06]`.

## Non-Obvious Insights
- Tokenization is not just preprocessing. It is an information allocation policy. The model’s effective context budget depends on how the tokenizer spends vocabulary on frequent patterns versus rare patterns. Source: `[1:08:58]` to `[1:09:16]`, `[1:18:10]` to `[1:18:18]`.
- The lecture’s real complaint is not “BPE is old.” It is that we still do not have a clean byte-native architecture, so tokenization remains the compromise layer. Source: `[1:18:21]` to `[1:18:30]`.
- GPT-2’s byte-level BPE is presented as a bridge between old-school compression and modern LM training, not as a magic invention. Source: `[1:11:11]` to `[1:11:53]`.
- The expensive part is not the algorithmic idea. It is making encode fast enough that the tokenizer does not become the bottleneck. Source: `[1:17:15]` to `[1:17:44]`.
- The lecture quietly treats tokenization as a systems decision, not a linguistic one. That is why bytes, Unicode pain, and efficiency keep coming back. Source: `[1:06:06]` to `[1:07:35]`.

## Connections
- **To L02:** tokenization changes the length and shape of the data pipeline, which feeds directly into memory and compute accounting.
- **To L03:** tokenizer choice sets the vocabulary size hyperparameter and interacts with model capacity, especially when balancing efficiency against expressivity.
- **To real-world LMs:** the lecture frames GPT-2 as the point where BPE became the standard compromise for large-scale language modeling.

## Referenced Papers
- **Gage, 1994, Byte Pair Encoding.** The original compression algorithm the lecture cites as the root of BPE. Source: `[1:11:02]` to `[1:11:16]`.
- **Sennrich et al., 2016, Neural Machine Translation of Rare Words with Subword Units.** The lecture credits the NMT line of work with introducing BPE into NLP. Source: `[1:11:16]` to `[1:11:22]`.
- **GPT-2.** The lecture uses GPT-2 as the model that carried byte-level BPE into modern language modeling. Source: `[1:11:48]` to `[1:12:43]`.

## Timestamps
- **0:05 - 5:09**: course framing, why build from scratch, why frontier-model abstraction is leaky.
- **1:01:04 - 1:10:39**: character, byte, and word tokenization tradeoffs.
- **1:11:02 - 1:16:04**: BPE mechanics and merge loop.
- **1:16:10 - 1:17:44**: encode/decode behavior and implementation notes.
- **1:17:46 - 1:18:30**: summary and why tokenization still exists.

**Sources**
- Transcript: `/Users/otonashi/thinking/pratchett-os/tmp/research-001-l01-transcript.txt`
- Video: `https://www.youtube.com/watch?v=SQ3fZ1sAqXI`
