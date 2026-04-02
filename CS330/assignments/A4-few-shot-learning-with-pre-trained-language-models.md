# CS330 Assignment 4: Few-Shot Learning with Pre-trained Language Models

## Problem Statement
This assignment is the practical bridge between pretraining and adaptation. The core question is simple: when you have a pre-trained language model, do you fine-tune it, prompt it, or do something cheaper in between?

The handout pushes that question across three different language tasks:

- **Amazon Reviews** for direct fine-tuning on a small classification problem.
- **XSum** for few-shot generation and summarization.
- **bAbI** for few-shot question answering with context-sensitive prompting.

The deliverable is not a single trick. It is a small adaptation stack:

- `ft.py` handles fine-tuning, parameter selection, loss, accuracy, and LoRA.
- `icl.py` handles prompt construction and greedy generation for in-context learning.
- The evaluation compares full fine-tuning, parameter-efficient fine-tuning, and prompt-only inference under the same pre-trained model family.

The assignment matters because it makes the tradeoff visible. Full fine-tuning is straightforward but expensive. In-context learning is cheap to deploy but fragile. LoRA and partial fine-tuning sit in the middle.

## Key Techniques
- **Full-model fine-tuning.** Start with the simplest k-shot baseline: update all BERT parameters on Amazon Reviews.
- **Parameter-efficient fine-tuning.** Restrict updates to the last, first, or middle transformer blocks instead of copying the whole model per task.
- **In-context learning.** Build prompts from support examples instead of changing weights.
- **Prompt formatting.** The handout compares `qa`, `none`, `tldr`, and `custom` prompt styles, so formatting is part of the algorithm.
- **Greedy decoding with caching.** The generation code uses GPT-2 autoregressive sampling and cached past key values to keep inference tractable.
- **LoRA.** Fine-tune a low-rank residual on top of frozen weights instead of learning a full replacement matrix.
- **Task-specific metrics.** Classification uses accuracy; summarization uses ROUGE; QA uses exact-answer style accuracy.

## Implementation Walkthrough
1. **Start in `ft.py`.**
   - Implement `parameters_to_fine_tune()` so the script can switch between full fine-tuning and subset fine-tuning.
   - Finish `get_loss()` and `get_acc()` for the 2D classification case used in the Amazon warmup.

2. **Run the Amazon Reviews warmup.**
   - Fine-tune `bert-tiny` and `bert-med` for `k = 1, 8, 128`.
   - Compare how scale changes the low-shot curve.
   - Use the warmup to verify the loss/accuracy plumbing before touching the generation code.

3. **Implement the in-context learning path in `icl.py`.**
   - Build prompts for XSum and bAbI in `get_icl_prompts()`.
   - Preserve support-example pairings while shuffling the order of examples.
   - Handle the prompt modes the handout names explicitly: `qa`, `none`, `tldr`, and `custom`.

4. **Finish greedy sampling.**
   - Implement `do_sample()` using the GPT-2 causal language model interface.
   - Use caching so generation does not reprocess the entire prefix on every step.
   - Keep the prompt free of the trailing-space bug the handout warns about.

5. **Add LoRA support.**
   - Wrap linear layers with a low-rank adapter module.
   - Keep the base weights frozen.
   - Route the LoRA parameters through `parameters_to_fine_tune()` so the rest of the script can treat them like a normal fine-tuning mode.

6. **Run the evaluation scripts.**
   - Plot Amazon, XSum, and bAbI results.
   - Compare full fine-tuning against parameter-efficient variants and prompt-only inference.
   - Use the provided grader before packaging the submission zip.

## What You Learn
- Fine-tuning is the blunt instrument. It works, but every new task can mean a new full copy of the model.
- In-context learning is the opposite extreme. It avoids updates, but context windows and prompt formatting become hard constraints.
- LoRA shows the middle path: adapt in a low-rank subspace instead of rewriting the whole model.
- Model scale helps, but it does not erase the importance of prompt shape, task framing, and evaluation metric choice.
- Few-shot LMs are mostly a systems problem disguised as a modeling problem.

## Sources
- Official homework PDF: https://cs330.stanford.edu/materials/CS330_Homework_3.pdf
- Public course mirror: https://github.com/Andrew-Ng-s-number-one-fan/CS330-Deep-Multi-Task-and-Meta-Learning
