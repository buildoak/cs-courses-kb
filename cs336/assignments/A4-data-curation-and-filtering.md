# CS336 Assignment 4: Data Curation and Filtering

## Problem Statement
Assignment 4 shifts the focus from model code to the data engine behind a language model. The goal is to turn raw Common Crawl webpages into a filtered, deduplicated, tokenized corpus that actually improves downstream language-model training.

This is not one isolated preprocessing task. The assignment spans the full pipeline: inspect WARC and WET records, extract text from HTML, identify language, mask personal data, filter harmful content, apply heuristic and learned quality filters, remove exact and fuzzy duplicates, tokenize the surviving documents, and finally train a GPT-2-small-shaped model on the result. The leaderboard objective is explicit: minimize validation perplexity on the C4 100 domains subset of Paloma.

The assignment matters because it makes a blunt point: “better pretraining data” is mostly a systems and data-engineering problem. You are forced to define what good web text looks like operationally, then prove that your pipeline helps a model.

## Key Techniques
- **HTML-to-text extraction with encoding detection.** The first implementation task is to decode raw HTML bytes robustly and run `resiliparse.extract.html2text.extract_plain_text`.
- **fastText-based filtering.** Language identification and harmful-content classification both rely on fastText models and confidence scores rather than hand-written heuristics alone.
- **Regex-based PII masking.** Email addresses, phone numbers, and IPv4 addresses are masked directly in text before training.
- **Gopher-style heuristic quality rules.** Documents are filtered by length, average word length, ellipsis-heavy line endings, and alphabetic-token ratios.
- **Learned quality classification.** In addition to heuristics, the assignment asks for a quality classifier trained on higher-quality positives such as Wikipedia-linked pages.
- **Two levels of deduplication.** Exact line deduplication removes repeated boilerplate; MinHash + LSH fuzzy deduplication catches near-duplicate documents like templated license files.
- **Parallel corpus processing.** The full Common Crawl subset is large enough that multiprocessing and Slurm array execution are part of the intended solution, not optional optimization.
- **Fixed-model evaluation.** The point is to optimize data, not model architecture, so the training command and GPT-2-small hyperparameters are locked down.

## Implementation Walkthrough
1. **Start by wiring the filter primitives into the provided adapters.** The repository’s tests define the concrete contract: extract text from bytes, return `"en"`/`"zh"`-style language IDs, emit labels like `"nsfw"` and `"non-toxic"`, and expose exact and MinHash dedup functions through `tests/adapters.py`.
2. **Inspect raw crawl examples before filtering anything.** The handout explicitly has you browse WARC and WET records so you see the kinds of failures that matter: menus and footers masquerading as content, low-value fragments, boilerplate, and mixed-quality multilingual text.
3. **Build the single-document pipeline first.** A practical per-document order is: decode HTML, extract text, identify language, optionally mask PII, score harmful content, run heuristic and classifier-based quality filters, then serialize the document if it survives.
4. **Add corpus-level dedup as a second stage.** Exact line dedup requires a first pass that counts hashed lines across the corpus and a second pass that rewrites each file keeping only unique lines. Fuzzy dedup then works at document level: normalize text, compute n-gram MinHash signatures, use LSH bands to get candidate duplicate sets, compute true Jaccard similarity on candidates, cluster matches, and keep one representative per cluster.
5. **Track filter attribution.** The assignment explicitly asks for counts of how many examples each filter removes, so the pipeline should emit per-stage statistics rather than just a final output directory.
6. **Parallelize at the WARC-file boundary.** The handout gives both `concurrent.futures` and `submitit` patterns. That is a strong hint to make `process_single_warc_file(input_path, output_path)` the unit of work and let the executor fan out across many files.
7. **Tokenize and train with a fixed recipe.** Once the filtered corpus is ready, encode it with the GPT-2 tokenizer, append `<|endoftext|>` after each document, serialize token IDs, and train the provided GPT-2-small setup for 200K steps. The only knob you are supposed to optimize is the data itself.

## What You Learn
- Most of the hard work in web-scale pretraining happens before the first optimizer step.
- “Quality” is not a single filter. It is a stack of imperfect signals: extraction quality, language, safety, heuristics, learned scoring, and deduplication.
- Deduplication is multi-scale. Exact duplicate lines and fuzzy near-duplicate documents are different failure modes and need different machinery.
- Data pipelines need measurement just like models do. If you do not know what each filter removes and how it affects validation loss, you are not really optimizing data.

## Sources
- Handout: `https://github.com/stanford-cs336/spring2024-assignment4-data/blob/main/cs336_spring2024_assignment4_data.pdf`
- Repo overview: `https://github.com/stanford-cs336/spring2024-assignment4-data`
- Local handout extraction: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a4/cs336_spring2024_assignment4_data.pdf` via `pdftotext`
- Assignment 4 README: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a4/README.md`
- Data package README: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a4/cs336-data/README.md`
- Adapter contract: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a4/cs336-data/tests/adapters.py`
- Extraction test: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a4/cs336-data/tests/test_extract.py`
- Language ID tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a4/cs336-data/tests/test_langid.py`
- PII tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a4/cs336-data/tests/test_pii.py`
- Harmful-content tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a4/cs336-data/tests/test_toxicity.py`
- Quality tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a4/cs336-data/tests/test_quality.py`
- Deduplication tests: `/Users/otonashi/thinking/pratchett-os/tmp/research-009-a4/cs336-data/tests/test_deduplication.py`
