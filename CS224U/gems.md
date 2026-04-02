# CS224U Gems

Synthesis across `L01.md` through `L14.md`.

## Top Insights

- The course is not really "from RNNs to transformers." It is a shift of leverage: hand-built features give way to learned objectives, then to evaluation and deployment constraints. The architecture changes matter, but the evaluation frame keeps becoming the real bottleneck. Sources: [L01](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L01.md#L15), [L02](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L02.md#L7), [L09](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L09.md#L23), [L13](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L13.md#L25), [L14](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L14.md#L25).

- "Contextual representation" is a bundle, not a single trick. Tokenization, position, and objective all become part of the representation surface once the model stops pretending words are static lookup entries. Sources: [L02](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L02.md#L17), [L04](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L04.md#L14), [L06](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L06.md#L23), [L08](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L08.md#L13).

- Dense learning signal beats sparse signal more often than the older BERT-era story suggests. BERT spends compute on a small masked minority, ELECTRA reassigns signal to every token, RoBERTa widens the effective training distribution, and BART/T5/distillation all repackage supervision so more of it survives the training loop. Sources: [L08](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L08.md#L23), [L09](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L09.md#L24), [L10](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L10.md#L23), [L11](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L11.md#L27), [L12](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L12.md#L25).

- Evaluation is part of the model. Baselines, dev curves, seed variance, and deployment cost all change what a score means; a leaderboard number by itself is mostly a social artifact. Sources: [L03](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L03.md#L28), [L13](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L13.md#L24), [L14](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L14.md#L24).

- Smaller, inspectable, and compressed models are not merely cheaper versions of the same thing. They are epistemic tools: easier to analyze, easier to deploy, and often easier to trust because their behavior can actually be examined. Sources: [L01](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L01.md#L31), [L02](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L02.md#L32), [L12](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L12.md#L28), [L14](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L14.md#L29).

## Surprising Connections

- BERT, ELECTRA, and BART are all corruption-repair systems in different clothes. BERT hides tokens and reconstructs them, ELECTRA replaces tokens and detects the replacement, and BART corrupts sequences and learns to restore them. The common move is to turn damage into supervision. Sources: [L08](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L08.md#L25), [L10](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L10.md#L23), [L11](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L11.md#L27).

- T5 task prefixes and GPT-style prompting are the same interface idea at different maturity levels: put the instruction in the text stream and let the model condition on it. Sources: [L11](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L11.md#L25), [L01](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L01.md#L19), [L07](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L07.md#L22).

- RoBERTa and the evaluation lectures are about the same hidden problem: recipe changes only matter if the measurement protocol can actually see them. One lecture says "change the recipe harder"; the other says "do not trust one score." Sources: [L09](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L09.md#L23), [L13](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L13.md#L25), [L13](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L13.md#L27).

- The CLS token in BERT and the teacher in distillation both act as compression bottlenecks. One turns a sequence into a cheap downstream summary; the other turns a large model's behavior into something a smaller model can absorb. Sources: [L08](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L08.md#L24), [L12](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L12.md#L25).

- GPT decoding, beam search, and teacher forcing show that generation is partly a policy layer on top of scores, not a property of the network alone. The model produces distributions; the decoding rule decides how those distributions become tokens. Sources: [L07](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L07.md#L22), [L07](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L07.md#L23).

## Under-Appreciated Ideas

- Tokenization is a modeling decision, not preprocessing trivia. WordPiece, BPE, and subword splitting change what the model can even express, especially once context is involved. Sources: [L02](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L02.md#L29), [L08](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L08.md#L18), [L09](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L09.md#L18).

- Relative positional encoding is an inductive-bias question, not a bookkeeping trick. It generalizes better because it reuses position information across contexts instead of baking every absolute slot into the input. Sources: [L06](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L06.md#L23), [L06](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L06.md#L27).

- Fine-tuning is not a binary switch. It is a stack of choices: which layers freeze, which outputs you pool, how much masking mismatch you accept, and how aggressively you optimize. Sources: [L02](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L02.md#L22), [L07](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L07.md#L18), [L08](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L08.md#L17).

- Distillation is behavior transfer plus regularization, not only compression. The lecture treats hard labels, soft labels, hidden states, embeddings, co-distillation, and counterfactual variants as real options. Sources: [L12](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L12.md#L25), [L12](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L12.md#L26), [L12](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L12.md#L27).

- Seed variance is not an annoyance. It can erase apparent model differences or reveal failures that a single run would hide. Sources: [L13](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L13.md#L27), [L13](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L13.md#L28).

- Cost-aware evaluation should be the default, not a special-case add-on. Latency, RAM, and index size are part of the system, so they should be part of the score. Sources: [L14](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L14.md#L25), [L14](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L14.md#L26), [L14](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L14.md#L29).

- The bakeoff contract matters because it turns scientific discipline into a visible constraint. Public test boundaries and one-shot submission are not admin noise; they are part of the lesson. Sources: [L03](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L03.md#L28), [L03](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L03.md#L30), [L03](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L03.md#L32).

## Open Questions

- Where should structure live: in architecture, in objective, in data, or in prompting? The lectures keep moving that boundary and never settle it.

- Can we get BERT-level contextual power without MLM's sparse signal and train/fine-tune mismatch? ELECTRA and RoBERTa both attack pieces of the problem, but neither fully closes it. Sources: [L08](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L08.md#L25), [L10](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L10.md#L25).

- How should we compare models when cost is multi-objective and seed variance is real? A single mean score is too thin to carry that load. Sources: [L13](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L13.md#L28), [L14](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L14.md#L26).

- How far can prompting and task prefixes go before task-specific heads or specialized architectures become necessary again? Sources: [L01](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L01.md#L19), [L11](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L11.md#L25).

- Can distillation preserve reasoning behavior, or only output behavior? The lecture is comfortable talking about labels and representations, but the reasoning itself is still an open target. Sources: [L12](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L12.md#L27), [L12](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L12.md#L28).

- Which positional scheme truly generalizes: absolute, frequency-based, relative, or something retrieval-like and more dynamic? Sources: [L06](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L06.md#L23), [L06](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L06.md#L25).

- Is the next breakthrough going to be a better architecture, or just a better recipe and measurement stack? The lectures keep making the recipe look more important than the logo on the paper. Sources: [L09](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L09.md#L23), [L10](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L10.md#L24), [L13](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L13.md#L29), [L14](/Users/otonashi/thinking/building/cs-courses-kb/CS224U/lectures/L14.md#L27).
