# CS224N Assignment 4: Self-Attention, Transformers, and Pretraining

## Problem Statement
Assignment 4 is the public Spring 2024 CS224N Transformer assignment. It moves from RNN-based sequence modeling into self-attention, positional encoding, and pretraining. The written section studies what single-head and multi-head attention can or cannot represent, why position embeddings matter, and how RoPE changes positional behavior. The coding section then turns those ideas into a small GPT-style model.

The practical task is deliberately concrete: pretrain a character-level Transformer on Wikipedia span corruption, then fine-tune it to answer questions of the form `Where was [person] born?`. The assignment is using a tiny model and a narrow factual QA task to teach a broader lesson: pretraining can inject world knowledge that is absent from supervised labels, and evaluation becomes a comparison between no-pretraining, pretraining, and RoPE variants rather than a single accuracy number.

## Key Techniques
- **Causal multi-head self-attention.** Queries, keys, and values are projected per head, masked causally, and recombined through an output projection.
- **Transformer blocks with residual paths.** Each block is LayerNorm -> attention -> residual, then LayerNorm -> MLP -> residual.
- **Character-level pretraining via span corruption.** Instead of next-token prediction on raw text alone, the dataset removes a span, inserts a mask token, appends the removed span later in the sequence, and trains the model to predict that reordered string.
- **Knowledge-access fine-tuning.** Fine-tuning converts `name <mask>` style sequences into birthplace prediction examples while masking out the question prefix in the target labels.
- **Absolute vs rotary position handling.** The vanilla model uses learned position embeddings; the RoPE variant rotates query/key feature pairs so attention depends on relative position.
- **Compare systems, not just models.** The handout explicitly frames the work as no-pretraining vs pretrained vs RoPE-pretrained evaluation on held-out birthplace prediction.

## Implementation Walkthrough
1. **Instantiate a compact GPT stack.** The public solution wires a `GPTConfig` with block size 128, 4 layers, 8 heads, and embedding size 256. `run.py` creates either a vanilla model or a RoPE-enabled model from the same base config.
2. **Build the Transformer core.** In `models.py`, token embeddings are optionally combined with learned positional embeddings, passed through stacked Transformer blocks, normalized once more, and projected to vocabulary logits. Each block is pre-LayerNorm attention followed by a GELU MLP.
3. **Implement causal self-attention directly.** `attention.py` does not call a black-box attention layer. It constructs `q`, `k`, and `v`, reshapes them into heads, applies the lower-triangular causal mask, softmaxes the attention matrix, and projects the merged head outputs back to model width.
4. **Add RoPE as a variant, not a separate model family.** The same attention module precomputes rotary sine/cosine caches, applies them to the query and key tensors, and leaves the rest of the attention pipeline unchanged. That is the key design lesson: relative positional structure can be injected with a localized change in the attention path.
5. **Use two different datasets for the two phases.** `CharCorruptionDataset` creates pretraining examples by truncating a Wikipedia line, masking a random span, and forcing the model to reconstruct it autoregressively. `NameDataset` then repackages TSV rows into `Where was X born?` prompts where only the birthplace portion contributes to loss.
6. **Train with a real optimizer schedule.** `trainer.py` groups parameters for AdamW weight decay, clips gradients, and applies linear warmup followed by cosine decay. `run.py` uses long pretraining runs, shorter fine-tuning runs, checkpoint writes, and a separate evaluation path that samples completions and scores predicted birthplaces.
7. **Treat evaluation as the point of the assignment.** The handout asks you to compare no-pretraining, pretrained vanilla, a London baseline, and RoPE-pretrained models. It even gives target bars: at least 15% dev accuracy after pretraining and at least 30% test accuracy for the RoPE variant.

## What You Learn
- Self-attention is easiest to understand when you look at what it can copy, average, and fail to isolate under noisy keys.
- Pretraining changes what the model knows, not just how fast it learns. The birthplace task is simple precisely so the effect of pretraining is obvious.
- Dataset design matters as much as model design. Span corruption and masked fine-tuning targets are what make the small GPT learn something transferable.
- Positional encoding is not a side detail. Switching from learned absolute positions to RoPE changes how the model extrapolates and how relative offsets are represented inside attention.

## Sources
- Official assignment archive: `https://web.stanford.edu/class/cs224n/assignments/`
- Official handout: `https://web.stanford.edu/class/cs224n/assignments/a4_spr24_student_handout.pdf`
- Public GitHub solution repo: `https://github.com/Yousefbahr/Stanford-CS224N-NLP-2025/tree/main/a4`
- Attention implementation: `https://raw.githubusercontent.com/Yousefbahr/Stanford-CS224N-NLP-2025/main/a4/src/attention.py`
- Transformer model definition: `https://raw.githubusercontent.com/Yousefbahr/Stanford-CS224N-NLP-2025/main/a4/src/models.py`
- Dataset definitions: `https://raw.githubusercontent.com/Yousefbahr/Stanford-CS224N-NLP-2025/main/a4/src/dataset.py`
- Trainer loop: `https://raw.githubusercontent.com/Yousefbahr/Stanford-CS224N-NLP-2025/main/a4/src/trainer.py`
- Pretrain/finetune/evaluate entrypoint: `https://raw.githubusercontent.com/Yousefbahr/Stanford-CS224N-NLP-2025/main/a4/src/run.py`
