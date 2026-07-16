# Health Text Simplification — Research Notebook

Implementation and evaluation of a transformer-based pipeline for simplifying technical
biomedical sentences into plain-language text. The pipeline is built and evaluated on
Cochrane-auto, a dataset that pairs sentences from Cochrane systematic review abstracts with
their official plain-language summaries, and compares a fine-tuned T5-small model against a
rule-based heuristic baseline and a zero-shot (non-fine-tuned) transformer baseline.

## Project layout

```
research/
├── data/
│   ├── raw/cochrane-auto/     # Cochrane-auto dataset (Bakker & Kamps, 2024),
│   │                           # fetched automatically by the notebook on first run
│   └── processed/             # cleaned train/val/test CSVs and a preprocessing log, generated
│                               # by the notebook's cleaning step
├── notebooks/
│   └── research.ipynb         # the full pipeline: EDA -> baselines -> fine-tuning -> evaluation
├── outputs/
│   ├── figures/                # chart images (PNG, 150 dpi), regenerated on each run
│   ├── tables/                 # result tables (CSV), regenerated on each run
│   └── models/                 # fine-tuned model checkpoints — not tracked in git (exceeds
│                                # GitHub's file size limit); recreate by running the notebook
├── requirements.txt
└── README.md
```

## Dataset

**Primary dataset:** [Cochrane-auto](https://github.com/JanB100/cochrane-auto) (Bakker and Kamps,
2024, [TSAR 2024](https://aclanthology.org/2024.tsar-1.5/)). The notebook clones it automatically into `data/raw/cochrane-auto` the first time it runs.

Other datasets referenced in the accompanying thesis (ASSET, WikiAuto, GEM Cochrane
Simplification) are discussed there as comparison points but are not used for training in this
implementation.

## Environment setup

No virtual environment is included in this repository — `.venv` is intentionally excluded from
git, since a project-specific environment is large, platform-dependent, and trivially
reproducible from `requirements.txt`.

```bash
cd research
python3 -m venv .venv
source .venv/bin/activate      # on Windows: .venv\Scripts\activate
pip install -r requirements.txt
python -m ipykernel install --user --name research --display-name "Python 3 (research)"
jupyter notebook notebooks/research.ipynb
```

Any Python 3.10+ environment with the packages in `requirements.txt` installed will run the
notebook. It has no dependency on a particular operating system or chip architecture. The
results currently saved under `outputs/` were produced on a Mac with Apple Silicon, using
PyTorch's MPS backend for training acceleration; on other machines the notebook automatically
selects CUDA if a GPU is available, and falls back to CPU otherwise. A CUDA GPU (for example, a
free Google Colab runtime) will fine-tune noticeably faster than a CPU-only environment.

## Running the notebook

The notebook has a `DEV_MODE` switch near the top (Section 1 — Configuration):

- `DEV_MODE = True` (default): fine-tunes on a subset of 800 training pairs for 3 epochs.
  Completes in a few minutes on a laptop and is intended to confirm that the full pipeline runs
  correctly end to end.
- `DEV_MODE = False`: fine-tunes on the full sentence-level training set (~7,000 pairs) for up to
  8 epochs, with early stopping. This produces the results intended for reporting and takes
  considerably longer, particularly without a GPU.

Run all cells top to bottom (`Kernel -> Restart & Run All`). Each section prints result tables and
renders charts inline. Figures are also saved to `outputs/figures/*.png` and tables to
`outputs/tables/*.csv`.

## Pipeline overview

1. **Environment and configuration** — random seeds, device detection (MPS/CUDA/CPU), and all
   hyperparameters defined in one place.
2. **Dataset acquisition and inspection** — loads the Cochrane-auto sentence-level splits and
   builds (complex, simple) pairs from the `rephrase`/`split`/`merge`/`delete`/`ignore` alignment
   labels.
3. **Exploratory data analysis** — sentence length distributions, compression ratios, label
   counts, and manual inspection of sample pairs.
4. **Data cleaning** — deduplication, whitespace normalisation, and removal of empty or
   too-short pairs, with a logged, traceable record of every row dropped or flagged.
5. **Baseline 1 — heuristic rule-based simplifier** — lexical substitution combined with
   length-based sentence splitting.
6. **Baseline 2 — zero-shot T5-small** — the off-the-shelf pretrained model, used without
   fine-tuning.
7. **Main model — fine-tuned T5-small** — fine-tuned on the cleaned Cochrane-auto pairs, with
   training settings and loss curves recorded.
8. **Evaluation** — readability (Flesch Reading Ease, Flesch-Kincaid Grade Level, SMOG), overlap
   (BLEU, ROUGE), a simplification-specific metric (SARI), and a semantic similarity metric
   (BERTScore), reported for every system.
9. **Qualitative analysis** — individual outputs selected by SARI and BERTScore, read manually
   for meaning preservation.
10. **Discussion and possible extensions** — including scaling up training and comparing
    alternative model architectures.

## Extending this work

- Set `CONFIG["model_name"]` to `"facebook/bart-base"` to fine-tune BART instead of T5-small; the
  training code is architecture-agnostic.
- Set `CONFIG["bertscore_model_type"]` to `"roberta-large"` to match the embedding model used in
  the original BERTScore paper. `distilbert-base-uncased` is used by default as a lighter,
  faster stand-in.
- Set `DEV_MODE = False` and evaluate on the full test split rather than a sample for final
  reported results.
