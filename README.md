# Automatic Plain-Language Simplification of Public-Facing Health Information Using Transformer-Based NLP

An project investigating whether a transformer-based language model can rewrite
technical biomedical text into plain language without losing the meaning that makes it medically
useful. The system is trained and evaluated on Cochrane-auto, a dataset that pairs sentences from
Cochrane systematic review abstracts with their official plain-language summaries, and is compared
against a rule-based heuristic baseline and a zero-shot (non-fine-tuned) transformer baseline.

## Repository structure

```
.
├── research/     # Python implementation: data pipeline, baselines, fine-tuning, evaluation
│                 # (Jupyter notebook — see research/README.md)
├── thesis/       # Dissertation write-up in LaTeX, Overleaf-ready
│                 # (see thesis/README.md)
├── Week 1 Project Proposal.md
└── Automatic Plain-Language Simplification of Public-Facing Health Information
    Using Transformer-Based NLP.md    # literature review and design notes
```

`research/` and `thesis/` are independent and can be opened separately. The two proposal and
literature-review documents at the repository root record the project's initial planning and are
kept for reference.

## Summary of approach

1. **Data.** Cochrane-auto (Bakker and Kamps, 2024) supplies sentence-level pairs of technical
   abstract text and its corresponding plain-language summary, already labelled by rewrite type
   (`rephrase`, `split`, `merge`, `delete`, `ignore`/`none`).
2. **Baselines.** A hand-written rule-based simplifier (lexical substitution plus sentence
   splitting) and a zero-shot T5-small model (no fine-tuning) are used as points of comparison.
3. **Model.** T5-small is fine-tuned on the cleaned Cochrane-auto training pairs.
4. **Evaluation.** Every system is scored on readability (Flesch Reading Ease, Flesch-Kincaid
   Grade Level, SMOG), text overlap (BLEU, ROUGE), a simplification-specific metric (SARI), and a
   semantic similarity metric (BERTScore), followed by a manual reading of individual outputs.

Full methodology, results, and discussion are in `thesis/`; the runnable implementation and raw
outputs are in `research/`.

## Results at a glance

Scores from a confirmation run (800 training pairs, 3 epochs) on a 150-sentence test sample. See
`thesis/chapters/07_results_and_discussion.tex` for the full discussion, including a manual
reading of individual outputs.

| System              | BLEU  | SARI  | BERTScore F1 |
|----------------------|------:|------:|-------------:|
| Source (unmodified)  | 29.59 | 51.94 | 0.881 |
| Heuristic baseline   | 28.39 | 48.77 | 0.879 |
| Zero-shot T5-small   | 19.31 | 36.29 | 0.787 |
| Fine-tuned T5-small  | 29.81 | 50.50 | **0.885** |

Fine-tuning clearly improves on the untrained model. Against the rule-based baseline, the result
is closer: the fine-tuned model edges ahead on SARI and BERTScore but does not yet outperform it
on every measure, which the thesis discusses in more detail rather than treating as a clean win.

## Reproducing the results

See `research/README.md` for full setup instructions. In short:

```bash
cd research
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook notebooks/research.ipynb
```

The notebook's `DEV_MODE` flag controls whether it runs a fast confirmation pass or the full
training run described in the thesis.

## Reference

Bakker, J. and Kamps, J. (2024). Cochrane-auto: An Aligned Dataset for the Simplification of
Biomedical Abstracts. In *Proceedings of the Third Workshop on Text Simplification, Accessibility
and Readability (TSAR 2024)*, pp. 41–51. https://doi.org/10.18653/v1/2024.tsar-1.5
