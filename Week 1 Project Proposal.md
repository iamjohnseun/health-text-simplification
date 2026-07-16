| Student ID | B01037764  |
|---|---|
| Name | Nnamdi Salau-John  |
| Email | [Salau_john-ns@ulster.ac.uk](mailto:Salau_john-ns@ulster.ac.uk) |
| Project Title | Automatic Plain-Language Simplification of Public-Facing Health Information Using Transformer-Based NLP |

## Project Summary

This project investigates whether transformer-based natural language processing can simplify public-facing health information into clearer plain-language text without losing important meaning. The study is motivated by a practical problem: health and public service materials are often written for specialist or highly literate readers, even though the people who rely on them most may need simpler, more direct explanations to understand what actions to take or what the information means for them.

The project will use Cochrane-auto as the main dataset, which contains aligned biomedical abstracts and lay summaries and was created specifically to support simplification research in the health domain. This provides a suitable basis for comparing complex source text with simpler target text and for training a transformer-based model such as T5-small or BART-base on a realistic health communication task rather than on general-domain simplification alone.

Methodologically, the work will begin with dataset preparation, inspection of source and simplified text pairs, and the creation of train, validation, and test splits. A simple baseline method, such as rule-based sentence shortening or extractive simplification, will be implemented first, after which a transformer-based model will be fine-tuned and evaluated using readability metrics, simplification metrics, semantic similarity measures, and qualitative analysis of strong and weak outputs.

The expected contribution of the project is a practical simplification pipeline and a clearer understanding of whether transformer-based approaches can genuinely improve readability in health communication while preserving factual meaning. The study will also examine the limits of automation in this setting, especially where oversimplification, omission, or distortion could make health information easier to read but less safe or less accurate to use.

## Problem Statement

Important health and public-facing service information is often written in language that assumes a medically literate or professionally trained reader. In practice, that creates a gap between availability of information and actual understanding. A person may receive a health leaflet, a treatment explanation, or a public health summary, yet still struggle to understand what the text is really saying because the sentences are dense, the terminology is specialised, and the phrasing is more formal than necessary.[^1]

This project addresses that problem by investigating whether transformer-based text simplification can make health-related information easier to read while preserving the original meaning. The problem matters because poor understanding of health information may affect decision-making, patient confidence, adherence to advice, and general access to care. While simplification systems exist, recent work suggests that domain-specific data remains especially important, particularly in biomedical and health contexts where simplification must reduce reading difficulty without distorting medically relevant content.[^2][^1]

## Background Literature

Text (Plain Language) simplification is concerned with rewriting content so that it is easier to read while keeping the core message intact. Earlier work in the field often focused on sentence-level rewriting, lexical substitution, deletion, and splitting. Over time, the literature has shifted toward richer datasets and stronger evaluation practices, partly because simplification is not just shortening text. A good simplification may split a sentence, paraphrase technical phrases, reorder ideas, and remove non-essential detail, all at once.[^3][^2]

ASSET is one of the better-known resources in this area. It was introduced as a multi-reference dataset for sentence simplification and was designed to capture multiple rewriting transformations rather than a single type of edit. The authors argue that standard evaluation settings were too narrow and that more realistic simplification requires a broader view of the rewriting process.[^2]

WikiAuto also matters here, though in a slightly different way. Jiang et al. proposed a neural CRF approach for sentence alignment in text simplification, producing aligned data that supported stronger simplification modelling. Their work is relevant because aligned source-target pairs are central to supervised simplification, and the paper reports that a Transformer-based sequence-to-sequence model trained on aligned datasets established strong performance in both automatic and human evaluation.[^4]

For this project, the most important recent development is Cochrane-auto. Bakker and Kamps introduced Cochrane-auto as a large aligned corpus linking biomedical abstracts with lay summaries at sentence, paragraph, and abstract level. They report that a plan-guided simplification system trained on Cochrane-auto outperformed a strong baseline trained on unaligned abstracts and lay summaries. That finding is especially useful here because it suggests that domain-specific alignment may improve simplification quality for biomedical content, which is closer to the proposed project domain than general simplification datasets alone.[^1]

Evaluation literature is also relevant. BLEU was originally proposed for machine translation and remains widely used as an overlap-based metric, though it is often criticised for limited sensitivity to readability and simplification quality. ROUGE is similarly useful for overlap-oriented comparison, especially where content retention matters, but it does not directly measure readability or accessibility. Xu et al. introduced SARI specifically for text simplification and argued that it better captures the quality of words that are added, kept, or deleted during simplification. That makes it particularly suitable for this project, where the aim is not simply to compress text but to rewrite it more accessibly.[^5][^6][^3]

Taken together, the literature suggests a few things. First, domain-specific health simplification is still worth studying because the cost of meaning loss is higher than in generic news or encyclopaedic text. Second, aligned biomedical datasets now make a focused project practical. Third, automatic evaluation alone is unlikely to be enough, so qualitative inspection is needed alongside readability and semantic-similarity measures.[^3][^1][^2]

## Dataset

The main dataset for the project will be **Cochrane-auto: An Aligned Dataset for the Simplification of Biomedical Abstracts** by Bakker and Kamps. It contains aligned biomedical abstracts and lay summaries and was created to support simplification research beyond sentence-level lexical editing.[^1]

- Dataset/paper link: [https://aclanthology.org/2024.tsar-1.5/](https://aclanthology.org/2024.tsar-1.5/)[^1]
- DOI: 10.18653/v1/2024.tsar-1.5[^1]

Supporting or backup datasets may include:

- **GEM Cochrane Simplification** dataset, based on the same broader medical simplification setting and useful if a benchmark-friendly format is needed for implementation.
- **ASSET**, a multi-reference English sentence simplification dataset that can be used for comparison experiments or metric sanity checks.[^2]
- **WikiAuto**, an aligned text simplification resource that may be used as optional comparison data where general-domain transfer is worth testing.[^4]

These additional datasets would not replace the main focus on health information. Their role would be supportive, mainly for comparison, fallback, or limited cross-domain analysis.

## Research Question

Can transformer-based text simplification improve the readability of public-facing health information while preserving meaning?

## Aim and Objectives

### Aim

To design and evaluate a transformer-based plain-language simplification pipeline for health-related public-facing text, with a focus on improving readability while preserving the original meaning.

### Objectives

- To prepare a clean paired dataset of complex biomedical or health-related text and simplified lay summaries using Cochrane-auto as the primary source.[^1]
- To implement a simple baseline simplification method, such as rule-based sentence shortening or extractive simplification, for comparison.
- To fine-tune one practical transformer model, such as T5-small or BART-base, on the selected dataset.
- To evaluate outputs using readability metrics, overlap-based metrics, simplification-specific metrics, and semantic similarity measures.[^6][^5][^3]
- To carry out qualitative error analysis on successful and unsuccessful simplifications.
- To discuss the risks of oversimplification, omission, and distortion in health communication.

## Methodology

### 1. Dataset Preparation

The first stage will involve downloading and inspecting Cochrane-auto, with possible access to GEM Cochrane Simplification if a benchmark-ready data format is more convenient for experimentation. Source and simplified pairs will be examined to understand alignment quality, text length, domain characteristics, and variation in rewriting style.[^1]

The text will be cleaned where necessary. Likely preprocessing steps include removing malformed examples, normalising whitespace, checking for duplicated pairs, and ensuring that source-target records are usable for modelling. After this, the dataset will be split into training, validation, and test sets. If the dataset already includes a standard split, that split will be respected where possible. ASSET or WikiAuto may be used only as optional comparison or backup data, rather than as the main training resource.[^4][^2]

### 2. Baseline Method

A simple baseline will be implemented before the transformer fine-tuning. This matters because a learned model should be shown to outperform something more basic than chance or trivial rewriting. The baseline may take one of three forms:

- extractive summarisation that selects the most central content;
- rule-based simplification using sentence splitting and heuristic shortening;
- sentence shortening guided by readability rules, such as limiting clause depth or removing low-value modifiers.

This baseline will provide a reference point for readability gains and meaning preservation.

### 3. Transformer-Based Method

The main modelling stage will use one manageable transformer architecture. The project should remain practical and reproducible, so the selected model will be intentionally modest in size. A likely starting point is **T5-small**, with **BART-base** as an alternative if training remains feasible on the available hardware. PEGASUS-small may be considered only if it proves computationally reasonable.

The chosen model will be fine-tuned on paired source and simplified texts. Training settings will be documented clearly, including:

- Tokenisation
- Maximum sequence length
- Batch size
- Learning rate
- Number of epochs
- Early stopping strategy

Since biomedical text can be longer and denser than ordinary simplification corpora, some experimentation may be needed around truncation and sentence segmentation. That said, the project is not aiming to chase benchmark scores at any cost. A cleaner and more interpretable setup is likely to be more appropriate for a Week 1 proposal and the work that follows.

### 4. Evaluation

Evaluation will combine automatic metrics with qualitative analysis.

#### Readability metrics

- Flesch Reading Ease
- Flesch-Kincaid Grade Level
- SMOG index

These metrics will be used to test whether the simplified outputs are easier to read than the original texts.

#### Text generation and simplification metrics

- SARI, where implementation is feasible, because it is specifically designed for simplification.[^3]
- BLEU, as a reference overlap metric.[^5]
- ROUGE, where useful for content-retention comparison.[^6]

#### Meaning-preservation metrics

- BERTScore or a similar semantic similarity measure, to test whether simplified outputs remain semantically close to their references and, where appropriate, to the source.

#### Qualitative analysis

Automatic metrics will not be treated as sufficient on their own. A qualitative review will compare good and poor model outputs, focusing on:

- preserved meaning;
- loss of clinically important detail;
- over-simplification;
- awkward or unnatural rewrites;
- cases where the output becomes shorter but not genuinely clearer.

This mixed evaluation approach appears necessary because health information carries factual and safety implications that may not be captured by overlap or readability scores alone.[^3][^1]

## Expected Outcomes

Possible outcomes of the project include:

- a working plain-language simplification pipeline for health-related text;
- evidence that transformer-based simplification improves readability over a basic baseline;
- examples showing where meaning is preserved well and where it becomes distorted;
- a practical discussion of the trade-off between accessibility and informational precision in health communication.

Even if the model performs unevenly, that result would still be useful. In a health setting, identifying the limits of automatic simplification may be just as valuable as showing where it works.

## Project Management

This project will follow a staged workflow with incremental milestones. The first phase will focus on literature review, environment setup, and dataset inspection.

The middle phase will cover preprocessing, baseline implementation, transformer fine-tuning, and metric-based evaluation.

The final phase will emphasise analysis, writing, revision, and presentation preparation.

A lightweight project management approach will be used, with weekly goals, version-controlled code, and documented experiment settings. This should help maintain reproducibility and reduce the risk of losing track of parameter choices or dataset variants.

## 13-Week Gantt Chart

| Task | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 |
| :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- | :-- |
| Topic refinement and proposal writing | █ | █ |  |  |  |  |  |  |  |  |  |  |  |
| Literature review | █ | █ | █ | █ |  |  |  |  |  |  |  |  |  |
| Dataset acquisition and inspection |  | █ | █ |  |  |  |  |  |  |  |  |  |  |
| Data cleaning and preprocessing |  |  | █ | █ | █ |  |  |  |  |  |  |  |  |
| Baseline implementation |  |  |  | █ | █ |  |  |  |  |  |  |  |  |
| Transformer model fine-tuning |  |  |  |  | █ | █ | █ |  |  |  |  |  |  |
| Automatic evaluation |  |  |  |  |  | █ | █ | █ |  |  |  |  |  |
| Qualitative analysis |  |  |  |  |  |  | █ | █ | █ |  |  |  |  |
| Results interpretation |  |  |  |  |  |  |  | █ | █ |  |  |  |  |
| Drafting dissertation/report chapters |  | █ | █ | █ | █ | █ | █ | █ | █ | █ | █ |  |  |
| Revision and supervisor feedback |  |  |  |  |  |  |  |  |  | █ | █ |  |  |
| Final editing and submission prep |  |  |  |  |  |  |  |  |  |  | █ | █ | █ |

## Legal, Social and Ethical Issues

The project does not involve highly sensitive personal data. it relies only on public datasets such as Cochrane-auto, ASSET, and WikiAuto. Even so, there are still important ethical issues.[^2][^4][^1]

First, simplifying health information creates a risk of factual distortion. A model may generate fluent text that sounds clearer but omits a limitation, condition, or uncertainty that matters in a medical context. Second, simplification may unintentionally overstate certainty by replacing careful scientific phrasing with overly direct language. Third, there is a social risk that users may rely on machine-generated text in settings where professional interpretation is still needed.

To mitigate these concerns, the project will position the system as an assistive simplification tool rather than a medical decision system. Outputs will be evaluated not only for readability but also for semantic faithfulness. The discussion chapter will explicitly consider whether some kinds of health text should be simplified only under stricter constraints or human review.

## Risks and Mitigation

| Risk | Likely impact | Mitigation |
| :-- | :-- | :-- |
| Dataset access or formatting issues | Delays in experimentation | Use the ACL Anthology source for Cochrane-auto and keep GEM Cochrane Simplification as a backup path.[^1] |
| Transformer model too computationally expensive | Training becomes impractical | Start with T5-small and keep sequence lengths controlled. |
| Automatic metrics give misleading signals | Weak conclusions | Combine readability, overlap, simplification, semantic similarity, and qualitative review.[^5][^6][^3] |
| Simplified outputs lose medical meaning | Research validity weakened | Include semantic similarity checks and manual example analysis. |
| Scope becomes too broad | Project loses focus | Keep the main dataset fixed to Cochrane-auto and limit backup datasets to comparison only.[^1] |
| Over-reliance on benchmark scores | Discussion becomes shallow | Prioritise interpretation of examples and failure cases alongside metrics. |

## References

J. Bakker and J. Kamps, “Cochrane-auto: An Aligned Dataset for the Simplification of Biomedical Abstracts,” in *Proceedings of the Third Workshop on Text Simplification, Accessibility and Readability (TSAR 2024)*, Miami, Florida, USA, 2024, pp. 41–51. doi: 10.18653/v1/2024.tsar-1.5.

F. Alva-Manchego, L. Martin, A. Bordes, C. Scarton, B. Sagot, and L. Specia, “ASSET: A Dataset for Tuning and Evaluation of Sentence Simplification Models with Multiple Rewriting Transformations,” in *Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics*, 2020, pp. 4668–4679. doi: 10.18653/v1/2020.acl-main.424.

C. Jiang, M. Maddela, W. Lan, Y. Zhong, and W. Xu, “Neural CRF Model for Sentence Alignment in Text Simplification,” in *Proceedings of the 58th Annual Meeting of the Association for Computational Linguistics*, 2020, pp. 7943–7960. doi: 10.18653/v1/2020.acl-main.709.

K. Papineni, S. Roukos, T. Ward, and W.-J. Zhu, “BLEU: a Method for Automatic Evaluation of Machine Translation,” in *Proceedings of the 40th Annual Meeting of the Association for Computational Linguistics*, Philadelphia, Pennsylvania, USA, 2002, pp. 311–318. doi: 10.3115/1073083.1073135.

C.-Y. Lin, “ROUGE: A Package for Automatic Evaluation of Summaries,” in *Text Summarization Branches Out*, Barcelona, Spain, 2004, pp. 74–81.

W. Xu, C. Napoles, E. Pavlick, Q. Chen, and C. Callison-Burch, “Optimizing Statistical Machine Translation for Text Simplification,” *Transactions of the Association for Computational Linguistics*, vol. 4, pp. 401–415, 2016. doi: 10.1162/tacl_a_00107.

S. M. Baggio, S. D. Menon, P. Chitra and A. V. Hariharan, “*Reinforcement Learning-Based Text Simplification Framework* for Accessibility in Education and Public Services,” 2025 IEEE 5th International Conference on ICT in Business Industry & Government (ICTBIG), Indore, Madhya Pradesh, India, India, 2025, pp. 1-6, doi: 10.1109/ICTBIG68706.2025.

<span style="display:none">[^8][^9][^10][^11][^12][^13][^14][^15][^16][^17][^18][^19][^20][^21][^22][^23][^24][^25][^26][^27][^28][^29][^30][^7]</span>

<div align="center">⁂</div>

[^1]: <https://aclanthology.org/2024.tsar-1.5/>

[^2]: <https://aclanthology.org/2020.acl-main.424/>

[^3]: <https://aclanthology.org/Q16-1029/>

[^4]: <https://aclanthology.org/2020.acl-main.709/>

[^5]: <https://aclanthology.org/P02-1040/>

[^6]: <https://aclanthology.org/W04-1013/>

[^7]: <https://aclanthology.org/2024.tsar-1.5.pdf>

[^8]: <https://aclanthology.org/volumes/2024.tsar-1/>

[^9]: <https://aclanthology.org/2024.tsar-1.0/>

[^10]: <https://github.com/lmvasque/ts-coherence>

[^11]: <https://www.zora.uzh.ch/server/api/core/bitstreams/a9541768-c903-451a-b9bb-41e1cf75907b/content>

[^12]: <https://www.hu.nl/onderzoek/publicaties/considering-human-interaction-and-variability-in-automatic-text-simplification>

[^13]: <http://arxiv.org/abs/2005.00481>

[^14]: <https://www.proceedings.com/content/077/077395webtoc.pdf>

[^15]: <https://eprints.whiterose.ac.uk/160696/>

[^16]: <https://aclanthology.org/2020.acl-main.707/>

[^17]: <https://tsar-workshop.github.io/resources/>

[^18]: <https://arxiv.org/abs/2005.00481>

[^19]: <https://www.gabormelli.com/RKB/2002_BleuAMethodforAutomaticEvaluati>

[^20]: <https://dl.acm.org/doi/pdf/10.3115/1073083.1073135>

[^21]: <https://www.semanticscholar.org/paper/Bleu:-a-Method-for-Automatic-Evaluation-of-Machine-Papineni-Roukos/d7da009f457917aa381619facfa5ffae9329a6e9>

[^22]: <https://dblp.org/rec/conf/acl/PapineniRWZ02.html>

[^23]: <https://music.amazon.com/podcasts/bad34c42-bf59-4b9a-8085-5742fd63e011/episodes/720b5cc1-609d-4c54-aa18-22c51a9cfc95/ai-post-transformers-bleu-automatic-machine-translation-evaluation>

[^24]: <https://scholar.google.com/scholar_lookup?title=Rouge%3A+A+package+for+automatic+evaluation+of+summaries\&conference=Proceedings+of+the+Workshop+on+Text+Summarization+Branches+Out%2C+Post-Conference+Workshop+of+ACL+2004\&author=Lin%2C+C.Y.\&publication_year=2004\&pages=74%E2%80%9381>

[^25]: <https://www.bohrium.com/paper-details/optimizing-statistical-machine-translation-for-text-simplification/812815775122325504-62359>

[^26]: <https://moitvivt.ru/en/journal/article?id=1830\&class=country-mobile>

[^27]: <http://www.gabormelli.com/RKB/2004_ROUGEAPackageforAutomaticEvalua>

[^28]: <https://www.semanticscholar.org/paper/Optimizing-Statistical-Machine-Translation-for-Text-Xu-Napoles/a494b23f70cec7f5e69b971e9837fcecae5d128d>

[^29]: <https://github.com/chrismcmorran/BLEU/blob/master/README.md>

[^30]: <https://www.oreilly.com/library/view/reinforcement-learning-with/9781788835725/bd4944c5-5dba-476e-968b-2cd244890c46.xhtml>
