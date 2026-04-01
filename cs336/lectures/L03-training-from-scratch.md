# L03 Training from Scratch

Lecture 3 is the architecture audit. It is not “what is a transformer.” It is “what do the modern survivors actually do, and why did those choices converge.”

## Summary
- The lecture starts by contrasting the original transformer with the modern implementation expected in the course: pre-norm, RoPE, SwiGLU, and bias-free layers. Source: transcript `[0:47]` to `[2:57]` in `/Users/otonashi/thinking/pratchett-os/tmp/research-001-l03-transcript.txt`.
- The big story is convergence. Across many 2023-2025 dense models, architecture choices have collapsed toward a few defaults. Source: `[3:03]` to `[4:28]`, `[31:25]` to `[31:41]`.
- LayerNorm has mostly moved out of the residual stream, RMSNorm has mostly replaced it, and bias terms are disappearing. Source: `[9:26]` to `[18:05]`.
- Gated linear units, especially SwiGLU, have become the default MLP choice for modern LMs. Source: `[19:25]` to `[28:50]`.
- Hyperparameters are not sacred. The lecture treats them as constrained search variables, not folklore, and uses large-scale model tables to show where consensus exists and where it does not. Source: `[41:00]` to `[55:52]`.
- RoPE and attention variants show the same pattern: the details look chaotic, but the field has mostly converged on a small set of efficient choices. Source: `[32:34]` to `[40:10]`, `[1:23:13]` to `[1:26:52]`.

## Key Concepts
- **Pre-norm transformer.** Put normalization outside the residual stream so gradient flow stays sane in deep networks. Source: `[6:24]` to `[7:11]`, `[31:03]` to `[31:18]`.
- **RMSNorm.** Drop mean subtraction and bias terms; keep the scaling behavior. Source: `[12:04]` to `[12:59]`.
- **Bias-free blocks.** Modern models increasingly omit bias terms because they rarely buy enough quality to justify the extra memory movement and stability cost. Source: `[16:03]` to `[17:12]`.
- **GLU-family MLPs.** SwiGLU and GeGLU gate one projection with another, and the lecture treats this as a broad empirical win. Source: `[19:25]` to `[28:46]`.
- **Parallel layers.** Some models compute attention and MLP side by side and add both into the residual stream, trading architectural seriality for systems efficiency. Source: `[29:02]` to `[30:36]`.
- **RoPE.** Rotary position embeddings push positional information into the attention computation itself, and the lecture frames them as the de facto modern standard. Source: `[32:34]` to `[40:10]`.
- **Attention variants.** Multi-query, grouped-query, and sparse/sliding-window patterns are all attempts to reduce the cost of long-context attention without giving up too much quality. Source: `[1:15:29]` to `[1:26:52]`.

## Non-Obvious Insights
- The lecture’s core thesis is not “this new thing is better.” It is “architecture is becoming an evolutionary process.” The surviving choices are the ones that balance quality, stability, and hardware behavior. Source: `[3:10]` to `[4:28]`, `[18:28]` to `[19:17]`.
- Memory movement matters as much as FLOPs. RMSNorm and bias removal are framed as wins because they reduce data motion, not because the arithmetic itself is hard. Source: `[13:19]` to `[14:59]`.
- Convergence is real, but not absolute. Most modern models look LLaMA-like, yet the lecture explicitly calls out exceptions such as Cohere Command A/R+, Grok, Gemma 2, and OLMo 2. Source: `[8:57]` to `[10:03]`, `[17:55]` to `[18:05]`.
- Gated activations are not mandatory for good models. GPT-3, Nemotron 340B, and Falcon 2 11B are cited as counterexamples, which keeps the lecture honest. Source: `[28:21]` to `[28:46]`.
- The most useful architecture question is not “what is the mathematically pure version?” It is “what survives at scale without becoming a systems tax?” Source: `[19:11]` to `[19:17]`, `[29:28]` to `[30:29]`.
- Hyperparameters become architecture once scale gets large enough. The lecture treats hidden size, head dimensions, and MLP expansion as coupled design decisions, not independent knobs. Source: `[41:00]` to `[55:52]`, `[1:00:52]` to `[1:01:27]`.

## Connections
- **To L01:** tokenizer choice affects vocabulary size, which is one of the hyperparameters the lecture wants you to size intentionally.
- **To L02:** the architecture choices here are all justified through the same resource-accounting lens introduced in the previous lecture.
- **To modern model design:** the lecture reads like a field guide to why recent dense LMs look so similar.

## Referenced Papers
- **Vaswani et al., 2017, Attention Is All You Need.** The lecture uses the original transformer as the baseline comparison. Source: `[1:51]` to `[2:05]`.
- **Xiong et al., 2020.** Cited in the LayerNorm discussion as part of the pre-norm story. Source: `[7:32]` to `[7:53]`.
- **Narang et al., 2020.** Used for the RMSNorm and GLU ablation evidence. Source: `[15:24]` to `[15:55]`, `[27:31]` to `[27:53]`.
- **Ivanov et al., 2023, Memory Movement Is All You Need.** Used to support the claim that normalization and memory movement matter even when FLOPs are tiny. Source: `[13:23]` to `[14:59]`.
- **Shazeer et al., GLU variants paper.** The lecture calls this “Noam Shazeer’s original paper” and uses it as the primary GLU evidence source. Source: `[26:47]` to `[27:26]`.

## Timestamps
- **0:47 - 2:57**: lecture framing and the modern transformer variant used in the class.
- **3:03 - 4:28**: model-scaling survey and the “19 dense model releases” point.
- **6:24 - 18:05**: pre-norm, LayerNorm, RMSNorm, and bias terms.
- **19:25 - 28:50**: activations and GLU-family MLPs.
- **29:02 - 32:31**: serial vs parallel layers.
- **32:34 - 40:10**: RoPE and positional encoding.
- **41:00 - 55:52**: hyperparameter selection and scaling heuristics.
- **1:15:29 - 1:26:52**: attention variants, long context, and sparse/sliding-window strategies.

**Sources**
- Transcript: `/Users/otonashi/thinking/pratchett-os/tmp/research-001-l03-transcript.txt`
- Video: `https://www.youtube.com/watch?v=ptFiH_bHnJw`
