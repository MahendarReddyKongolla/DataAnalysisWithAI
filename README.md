# San Marino IMF Article IV — LLM Text-to-Dashboard Pipeline

Coursework for **7PAM2032 Data Analysis with AI, Assignment 1**.
Source document: *Republic of San Marino: 2024 Article IV Consultation — Press Release and Staff Report*,
IMF Country Report No. 24/330 (November 2024). The PDF is not included; download it from the IMF eLibrary.

## What the notebook does
1. Reads the PDF with pypdf, removes headers and footers, and splits it into chapters and annexes using the contents page.
2. Chunks each section with the DistilBERT tokenizer (450-token windows, 50-token overlap, trimmed to sentence ends).
3. Writes an executive summary and section summaries with Gemini, then scores them with a second Gemini model (LLM-as-a-Judge).
4. Counts seven financial-stability themes, extracts strengths and vulnerabilities with verified quotes, extracts six core indicators as JSON with fallbacks (judge-audited), and extracts annual time series from the statistical tables.
5. Builds a FAISS IndexFlatL2 over DistilBERT embeddings and compares grounded RAG, multi-query RAG (extension) and closed-book answers with RAG-triad scores.
6. Reshapes the results with `pd.melt` and draws a 2×2 dashboard at 300 dpi.

## Run it
Open the notebook in Google Colab, add the secret `GEMINI_API_KEY`, upload the report as `imf_report.pdf`, then Runtime → Run all.
All LLM answers are cached in `llm_cache.json`; with that file present, a re-run makes no API calls.

## Folders
* `outputs/` — every result as JSON, plus `call_log.csv`
* `figures/` — `dashboard.png` and `chunk_lengths.png` (300 dpi)

## Licence
MIT. See `LICENSE`.
