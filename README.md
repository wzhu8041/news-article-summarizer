# News Article Summarizer (LLM)

A small, end-to-end project that **takes news article links (URLs)**, extracts the article text, and generates a **clean summary** using a transformer summarization model.

This repo is organized around two workflows:

1. **Train / fine-tune a summarization model** (notebook)
2. **Run inference from links** (notebook)

---

## Why this matters

The modern news cycle is *high-volume* and often *high-noise*. For students, analysts, and decision-makers, the bottleneck isn’t access to information—it’s turning lots of long-form articles into a quick, accurate understanding of **what happened**, **why it matters**, and **what to read next**. A link-to-summary workflow helps you:

- **Triage faster:** skim key points before deciding what’s worth a full read  
- **Reduce cognitive load:** convert dense articles into digestible summaries  
- **Stay informed with less fatigue:** keep up with major developments without reading everything end-to-end  

As a portfolio project, this demonstrates an end-to-end applied NLP pipeline: **data ingestion → text extraction → model inference → user-facing output**, which is a common pattern in real analytics and product work.

---

## Repo structure

```
.
├── assets/
│   ├── Summarize_UI.png
│   └── Summarize_output.png
├── notebooks/
│   ├── NewsSummarizationModel.ipynb
│   └── runwithlinks.ipynb
├── models/
│   └── .gitkeep
├── requirements.txt
└── LICENSE
```

> Notes:
> - `models/` is intentionally empty by default. Keep large model artifacts out of git and store them in releases, cloud storage, Hugging Face Hub, or Git LFS.

---

## Demo

### Paste up to 3 article links

<img src="assets/Summarize_UI.png" alt="UI for pasting up to three URLs" width="800" />

### Example summarization output

<img src="assets/Summarize_output.png" alt="Example summary output" width="800" />

---

## Quickstart (Locally)

1. Open `notebooks/runwithlinks.ipynb` and follow the steps in order.
2. Provide the fine-tuned model artifact (e.g., a zipped checkpoint produced by the training notebook).
3. Paste one or more URLs and run inference to generate summaries.

---

## How it works (high level)

### 1) Article ingestion
- Download + parse article text from a URL (commonly via `newspaper3k`).
- Light cleaning and truncation to keep inputs within model limits.

### 2) Summarization
- Tokenize the article text with a BART tokenizer.
- Run a fine-tuned sequence-to-sequence model to generate a short summary.

---

## Model & training (high level)

- **Base model:** `facebook/bart-large-cnn`
- **Training dataset:** Multi-News
- **Training approach:** fine-tune BART for abstractive summarization; save the resulting checkpoint for downstream inference.

---

## Running locally

Create a virtual environment and install dependencies:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

> If you want GPU support locally, install the correct PyTorch build for your CUDA version (see PyTorch install instructions).

---

## How this could work in real-world deployment

A production version of this project could be implemented as a small service with guardrails:

- **Ingestion layer:** accept URLs, validate domains, respect `robots.txt`, and apply rate limiting/caching to avoid hammering publishers.
- **Extraction reliability:** use a robust parsing pipeline (fallback extractors, readability cleaning, length checks).
- **Summarization service:** package the model behind an API (FastAPI) or background job queue for longer articles; return results with latency targets.
- **Trust & accuracy controls:** mitigate hallucinations by adding entity/number checks, highlighting uncertainty, or attaching “key sentences” pulled from the source.
- **Publisher-friendly UX:** always preserve attribution and links; position summaries as a *reading aid* that drives users to original reporting rather than replacing it.
- **Monitoring:** track summary length/latency, error rates, and periodic quality checks (factual consistency) as data sources and writing styles change over time.

---

## Limitations

- Some publishers block scraping, require JavaScript rendering, or are behind paywalls.
- Generated summaries can be incomplete or occasionally incorrect—treat them as a reading aid, not ground truth.
- Scraping using newspaper3k doesn't always grab the correct text from the article.
- If running in Colab, Colab’s filesystem (`/content`) is temporary. When the runtime disconnects/restarts, your files can disappear.

---

## License

MIT (see `LICENSE`).
