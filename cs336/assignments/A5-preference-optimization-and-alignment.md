# CS336 Assignment 5: Preference Optimization and Alignment

## Problem Statement
Assignment 5 is the post-training stack in one course project. You start from a strong base model, Llama 3 8B, then build the machinery that turns it into an assistant: zero-shot evaluations, supervised fine-tuning on instruction-response pairs, safety and preference evaluation, and finally direct preference optimization (DPO) on human preference data.

The assignment is deliberately broader than “implement DPO.” First you need to establish baselines on MMLU, GSM8K, AlpacaEval, and SimpleSafetyTests. Then you pack instruction-tuning data into language-model sequences, fine-tune the model, and reevaluate it. Only after that do you move into preference learning, where the task becomes: take single-turn preference triples from Anthropic HH, compute the DPO loss against a frozen reference model, train for one epoch, and measure both capability gains and the alignment tax.

This assignment is valuable because it forces you to treat alignment as an end-to-end engineering loop: evaluation, data formatting, training, preference optimization, and regression testing all have to line up.

## Key Techniques
- **Structured zero-shot evaluation.** The baseline stage uses prompt templates plus parser functions to turn free-form generations into measurable outputs for MMLU and GSM8K.
- **vLLM-based offline inference.** The handout recommends vLLM for high-throughput batched generation over evaluation datasets.
- **Packed SFT data.** Instruction-response examples are rendered with the Alpaca prompt template, concatenated with an end-of-sequence token, and packed into fixed-length language-model sequences.
- **Gradient accumulation for large models.** The fine-tuning stage assumes Llama 3 8B in `bfloat16` with FlashAttention-2, then scales effective batch size through gradient accumulation.
- **Benchmark-specific external evaluation.** AlpacaEval uses Llama 3 70B Instruct as annotator against GPT-4 Turbo references; SimpleSafetyTests uses a separate safety-evaluation script to label outputs safe or unsafe.
- **Preference-data preprocessing.** The HH dataset is simplified into single-turn `(instruction, chosen, rejected)` triples before DPO training.
- **Reference-model regularization through DPO.** DPO compares how the trainable model and the frozen reference model score chosen versus rejected completions, avoiding a separate explicit reward model.

## Implementation Walkthrough
1. **Establish the baseline evaluation stack.** Implement the MMLU parser, the GSM8K numeric parser, and scripts that serialize generations and metrics for MMLU, GSM8K, AlpacaEval, and SimpleSafetyTests. The tests enforce concrete behavior here, including exact parsing expectations.
2. **Build the SFT dataset around the Alpaca template.** The assignment provides the prompt template in `cs336_alignment/prompts/alpaca_sft.prompt`. Each `(prompt, response)` example is rendered into that format, tokenized, concatenated, and packed into fixed-length sequences with aligned `input_ids` and `labels`.
3. **Train Llama 3 8B with memory-aware fine-tuning.** Load the model with Hugging Face Transformers in `bfloat16`, turn on FlashAttention-2, use a small per-device batch size, and recover a larger effective batch size with gradient accumulation. Save both model and tokenizer because later stages depend on reloading the exact SFT checkpoint.
4. **Re-evaluate after SFT.** Run the same benchmark suite again so you can measure what instruction tuning changed: general task performance, assistant-style output quality, and safety behavior.
5. **Preprocess HH for DPO.** Load the four HH training files, discard multi-turn conversations, split each example into the first human message plus chosen/rejected assistant responses, and preserve source provenance for later analysis.
6. **Implement per-instance DPO loss.** For each prompt, compute the chosen-vs-rejected log-probability gap under the trainable model and subtract the same gap under the frozen reference model, scaled by `beta`. The tests include a numerical fixture with tiny GPT-2 checkpoints, so the loss implementation has to be exact, not just conceptually correct.
7. **Train with two models in memory.** The handout recommends putting the reference model and trainable model on separate GPUs, using RMSprop, gradient accumulation, and validation accuracy of the implicit reward comparison as the selection metric.
8. **Measure post-DPO trade-offs.** After DPO, rerun AlpacaEval, SimpleSafetyTests, MMLU, and GSM8K. That final comparison is the real lesson: alignment can improve helpfulness and harmlessness while also moving other capabilities.

## What You Learn
- Alignment work begins with measurement. If you cannot parse outputs and benchmark them reliably, you cannot tell whether fine-tuning helped.
- Instruction tuning and preference optimization solve different problems: SFT teaches the response format and style, while DPO reshapes behavior using pairwise judgments.
- Large-model post-training is constrained as much by memory and evaluation infrastructure as by optimization theory.
- “Alignment tax” stops being a slogan once you have to re-run MMLU and GSM8K after DPO and see what moved.

## Sources
- Handout: `https://github.com/stanford-cs336/spring2024-assignment5-alignment/blob/main/cs336_spring2024_assignment5_alignment.pdf`
- Repo overview: `https://github.com/stanford-cs336/spring2024-assignment5-alignment`
- Local handout extraction: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a5/cs336_spring2024_assignment5_alignment.pdf` via `pdftotext`
- README: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a5/README.md`
- Adapter contract: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a5/tests/adapters.py`
- Dataset and batching tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a5/tests/test_data.py`
- Metric parser tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a5/tests/test_metrics.py`
- DPO loss test: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a5/tests/test_dpo.py`
- Alpaca SFT prompt: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a5/cs336_alignment/prompts/alpaca_sft.prompt`
- Zero-shot system prompt: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a5/cs336_alignment/prompts/zero_shot_system_prompt.prompt`
- Safety evaluation script: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a5/scripts/evaluate_safety.py`
