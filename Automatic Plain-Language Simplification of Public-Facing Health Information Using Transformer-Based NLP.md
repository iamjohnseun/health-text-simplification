# Automatic Plain-Language Simplification of Public-Facing Health Information Using Transformer-Based NLP

## 1. Literature Review

### 1.1 Plain-language communication and the case for simplification

At the centre of this project is a fairly ordinary but stubborn problem: health information is often written for people who already know how to read it. Public-facing texts may be labelled as patient information or general guidance, yet they still contain compressed scientific phrasing, long sentences, and terms that are familiar to clinicians but not to most readers. Guidance from health literacy work and lay-summary practice suggests that clarity usually depends on concrete choices such as shorter sentences, common vocabulary, explicit explanation of risk, and careful signalling of uncertainty rather than on simplification in the vague sense alone.[5][6]

Text simplification research approaches the problem from a different direction. Instead of starting with health communication, it begins with the broader NLP task of rewriting complex material into a form that is easier to read while trying to retain the underlying meaning. Earlier work often treated simplification as one operation at a time, perhaps lexical substitution or deletion. More recent work appears to suggest that effective simplification is rarely that neat. It tends to involve several edits at once, including paraphrasing, sentence splitting, reordering, and selective compression.[3]

The health domain makes this tension more visible. A sentence about treatment effect, side effects, or statistical uncertainty cannot simply be shortened at any cost. In one setting, replacing a technical phrase with a simpler one may improve access. In another, it may blur a distinction that matters. That is partly why this project focuses on public-facing health information rather than generic simplification alone. The question is not only whether text becomes easier to read, but whether it remains responsible.

### 1.2 Cochrane-auto and domain-specific supervision

The strongest argument for this project probably comes from Cochrane itself. Cochrane reviews already include technical abstracts and plain-language summaries written for non-specialist readers, so the field is not inventing the task from scratch. Bakker and Kamps formalised this practice in Cochrane-auto, an aligned dataset built from biomedical abstracts and their lay summaries.[1][16] That matters because it gives the project a domain-specific training resource instead of forcing it to rely entirely on simplification data from Wikipedia or news text.

Cochrane-auto is particularly useful because it supports alignment at sentence, paragraph, and document level.[1][16] In practical terms, that opens several routes for experimentation. A smaller MSc project is more likely to succeed with sentence or short-paragraph inputs, especially when compute and time are limited. Still, the dataset also points toward a broader question that may become relevant later in the dissertation: whether acceptable health simplification is mainly a sentence-level rewriting task, or whether coherence across a whole explanation matters just as much.

Published experiments on Cochrane-auto suggest that aligned, domain-specific training data can outperform weaker setups built from unaligned technical and lay texts.[1] That finding supports the design of the present project, although it should be treated with some care. Better automatic scores do not automatically mean safer or clearer health communication. A model can learn the style of lay summaries and still miss what a reader actually needs when trying to interpret a sentence about screening, medication, or treatment benefit.

### 1.3 What general simplification datasets still offer

Even though Cochrane-auto will serve as the primary dataset, the wider simplification literature remains useful. ASSET is still one of the clearest reminders that simplification is not a single edit operation. Each source sentence is paired with multiple human rewrites, and those rewrites involve different combinations of splitting, paraphrasing, deletion, and structural change.[3][12][17] That multi-reference design is valuable because it reflects something obvious once you see it: there is often more than one reasonable way to simplify a sentence.

That matters for evaluation. If a model output is judged against only one reference, a perfectly acceptable simplification may look worse than it really is. ASSET pushes against that problem, and it also helps explain why metrics designed for translation can be awkward fits for simplification.[3][12][17] For this project, ASSET may be more useful as a comparative or calibration resource than as a primary training set.

WikiAuto and related alignment work offer a different lesson. Jiang et al. showed that alignment quality has a direct effect on the quality of downstream simplification models.[23] Cleaner supervision usually produces cleaner generation. That sounds almost too obvious to mention, but it becomes important in health text, where poor alignment may not just add noise. It may teach the model the wrong relationship between technical claims and lay explanation.

### 1.4 Metrics and the limits of automatic evaluation

Evaluation in simplification research is slightly uncomfortable, and it probably should be. BLEU remains widely used because it is easy to compute and familiar from machine translation, but it was never designed to measure simplicity.[24] A model may produce a clearer sentence with different wording and still be punished because the n-gram overlap is low. ROUGE has similar limits. It can say something about retained content overlap, yet it does not directly reveal whether a sentence is easier to understand.[27]

SARI is generally a better fit for simplification because it attempts to evaluate added, deleted, and retained words in relation to references.[28] Even so, it is not a complete answer. Readability formulas such as Flesch Reading Ease, Flesch-Kincaid Grade Level, and SMOG can indicate whether outputs look simpler on the surface, but those scores do not guarantee that the resulting sentence is accurate, helpful, or even natural. In health communication, that gap matters quite a lot.

Recent work on meaning preservation makes the point more sharply. Human evaluation studies suggest that simplification systems can score well on automatic measures while still distorting meaning in subtle ways.[7] In a public-health context, subtle distortion is not a minor issue. Changing the force of a recommendation, weakening uncertainty language, or smoothing out a limitation statement may alter the reader’s understanding in ways that automatic metrics barely register.

### 1.5 Transformer models in simplification research

Transformer-based systems now dominate the practical side of text simplification, largely because encoder-decoder models can learn paraphrasing, compression, and structural rewriting within a single framework.[10] Models such as T5-small and BART-base are attractive here because they are manageable. They are not trivial, but they are still realistic for a student project in a way that much larger instruction-tuned models may not be.

There is, though, a quiet trade-off in choosing a smaller model. A compact model may be easier to fine-tune and reproduce, yet it may also struggle with longer biomedical passages or with more delicate simplification decisions. That is not necessarily a reason to avoid it. In fact, for this project it may be a strength. A modest model encourages a clearer research design. If the system works reasonably well, the result is easier to interpret. If it fails, the limitations will be easier to explain.

Recent biomedical and health-oriented work suggests that large language models can support lay summarisation and simplification, but the literature also keeps circling back to the same warning: readability improvement is only one part of the task.[11][20] Meaning preservation, factual stability, and suitability for non-specialist readers remain harder to secure. That recurring caution shapes the research design here. The project cannot rely on automatic gains in readability alone as evidence of success.

### 1.6 Human judgement, coherence, and variation

Another recurring theme in the literature is variation. Human writers do not simplify in one uniform way. Some explain first and shorten later. Others split aggressively. Some keep technical terms but gloss them; others replace them. Work on variability in simplification suggests that models often produce narrower, more repetitive outputs than humans do.[13] In health information, that may produce text that is technically simpler but oddly flat or too generic.

Coherence also matters more than sentence-level benchmarks sometimes admit. A patient information page or health summary is not just a bag of simplified sentences. It carries a line of reasoning, and sometimes a warning, across several sentences or paragraphs. Document-level work in simplification has started to take that more seriously.[14][18] Even if the initial implementation in this project stays at sentence level, the evaluation should remain alert to coherence problems that emerge once outputs are read in sequence.

### 1.7 Positioning of the present study

Taken together, the literature points to a fairly clear gap. There is growing evidence that aligned biomedical simplification is feasible, and there are workable datasets and transformer models for it.[1][3][23] Yet there is still a need for careful, smaller-scale studies that ask a more grounded question: can a manageable transformer model improve the readability of public-facing health information without stripping away the meaning that readers actually depend on?

This project is positioned as that kind of study. It does not assume that simplification is automatically beneficial. It treats health accessibility as a real need, but it also treats over-simplification as a genuine risk. That balance may sound cautious, but in this case caution is probably part of the method.

## 2. Dataset Acquisition and Inspection

### 2.1 Dataset selection

The main dataset for the project will be Cochrane-auto because it is the closest match to the task being studied.[1][16] It contains aligned biomedical abstracts and lay summaries, which makes it more suitable than generic simplification corpora for examining public-facing health communication. GEM Cochrane Simplification, ASSET, and WikiAuto will be treated as secondary resources rather than equal alternatives.[19][3][23]

That choice is partly methodological and partly practical. Cochrane-auto gives the project health-domain specificity from the start. ASSET is useful, especially for understanding multi-reference evaluation, but it does not reflect the biomedical setting directly.[3] WikiAuto can help as a comparison point, yet it is likely to carry stylistic habits from encyclopaedic writing that do not transfer neatly to patient-oriented health information.[23]

### 2.2 Acquisition procedure

The Cochrane-auto repository provides the materials needed to reconstruct the dataset structure used in the original work, including corpus files, alignments, and preprocessing code.[cite:124] The first stage of implementation should clone or download that repository and preserve the original split structure where possible. That is a small but important discipline. Reusing the published splits reduces the risk of accidental leakage and makes later comparison with prior work less messy.

Supporting datasets can be downloaded through Hugging Face or their published sources.[19][12][17] In practice, these should be stored separately and only brought into the pipeline when they are needed for comparison, metric calibration, or limited evaluation experiments. Keeping them distinct from the Cochrane-auto workflow will make the eventual write-up cleaner and the implementation easier to defend.

### 2.3 What exploratory analysis needs to establish

Exploratory analysis should do more than count rows. It needs to show what kind of rewriting problem the model is actually being asked to solve. At minimum, the analysis should examine the size of each split, length distributions for technical and lay texts, vocabulary differences, and the degree to which lay summaries compress, reorder, or expand the original information.[1][16]

Some of the most useful inspection will probably be manual rather than purely statistical. A quick read through a few aligned pairs can reveal issues that summary tables miss. One abstract sentence may map neatly onto one lay sentence. Another may be spread across two simpler statements, or reframed with an explanation that is not present word-for-word in the source. Those examples matter because they influence what counts as a reasonable baseline and what kinds of model errors should later be taken seriously.

### 2.4 What to look for during inspection

Inspection should pay attention to misalignment, unusual punctuation, duplicated boilerplate, empty fields, and extreme sequence length. It should also note cases where the lay version appears to add context rather than merely simplify wording. That pattern would be particularly interesting because it suggests the task is not pure simplification in the narrow sense. It may involve explanation, condensation, and audience adaptation at the same time.

If that is what the data looks like, the project should say so plainly. A baseline model trained on such data is unlikely to be learning only sentence simplification. It may also be learning a form of constrained lay summarisation. That does not weaken the project, but it does affect how results ought to be interpreted.

## 3. Data Cleansing and Preprocessing

### 3.1 Guiding principles

Preprocessing in this project should stay conservative. Health text is one of those domains where aggressive cleaning can quietly do damage. A phrase that looks repetitive or overly formal may still carry a distinction that matters. For that reason, the preprocessing pipeline should aim to remove noise without flattening medically relevant content.

Three principles guide the process. First, clinically meaningful wording should be preserved wherever possible. Second, formatting artefacts should be cleaned so that models do not waste capacity learning irrelevant patterns. Third, traceability should be maintained, so that any cleaned example can still be linked back to its original aligned pair.

### 3.2 Proposed procedure

The most sensible starting point is to reuse the Cochrane-auto preprocessing logic where it already exists and then add only those project-specific steps that are necessary for analysis or modelling.[cite:124] Text should be normalised to a consistent encoding, whitespace should be standardised, and obvious markup artefacts should be removed. Beyond that, restraint is probably wiser than enthusiasm.

Examples with missing text, severe corruption, or clear misalignment should be flagged. Some may need to be removed from training. Others might be better kept in a separate error set for later inspection. That distinction is worth preserving because bad examples can be analytically useful even when they are not suitable for model fitting.

A further decision concerns granularity. Sentence-level training is likely to be the practical starting point for T5-small, especially if the project aims to remain reproducible on modest hardware.[10] Paragraph-level data can be retained for later comparison, but beginning with shorter units reduces complexity and makes debugging easier. It also gives cleaner examples for baseline evaluation.

### 3.3 Why preprocessing choices matter

Preprocessing is not just a technical stage before the real work begins. In a project like this, it partly defines the task. If long sequences are heavily truncated, the model may learn to simplify only short claims. If misaligned examples are left in place, it may learn transformations that look fluent but are semantically loose. If every unusual term is normalised away, the dataset may stop resembling the health communication problem it was meant to represent.

That is why the preprocessing script should log what it changes and what it removes. A good implementation leaves a trail. Later, when a model output looks suspiciously neat or strangely vague, those records make it easier to ask whether the problem began in training data preparation rather than in the model alone.

## 4. Baseline Implementation

### 4.1 Rationale for a baseline

The baseline should be modest, reproducible, and slightly sceptical in spirit. Its role is not to impress. It is there to establish a fair lower bound against which the transformer model can be judged. If the baseline already improves readability without doing too much harm to meaning, then the transformer has to earn its place rather than benefiting from a weak comparison.

For that reason, the initial implementation will include two baselines. The first is a heuristic baseline that performs light sentence splitting, punctuation-aware shortening, and a small amount of lexical substitution using hand-written rules. The second is a transformer baseline built around T5-small, configured as a text-to-text simplification model.[10]

### 4.2 Heuristic baseline

The heuristic system should remain intentionally simple. It may split very long sentences at safe punctuation boundaries, replace a limited set of biomedical terms with plainer alternatives where the substitution is low risk, and remove obvious parenthetical clutter when that removal does not alter the core claim. It will not be elegant, and that is acceptable. In fact, a slightly awkward baseline is useful because it reveals what straightforward readability-oriented manipulation can and cannot achieve.

### 4.3 Transformer baseline

The transformer baseline will use T5-small because it is practical, well documented, and easy to fine-tune within a Python workflow built on Hugging Face Transformers.[10] Inputs should be prefixed in the usual text-to-text style, for example with an instruction such as `simplify:` followed by the source sentence. Early experiments should stay small, perhaps using a reduced sample of sentence-level Cochrane-auto pairs until the pipeline is stable.

There is a temptation to move immediately to stronger models or instruction-tuned systems. At this stage that would probably blur the research question. T5-small is enough to test whether domain-specific aligned data can support measurable readability gains under realistic project constraints.

### 4.4 Evaluation approach for the baseline stage

The first baseline evaluation should combine readability scores with overlap and meaning-oriented measures. Flesch Reading Ease, Flesch-Kincaid Grade Level, and SMOG can indicate whether outputs look easier to read on the surface. BLEU, ROUGE, and where possible SARI can provide a complementary view of textual similarity and simplification behaviour.[24][27][28]

Still, automatic metrics should not be allowed to dominate interpretation. A small set of qualitative examples will be needed from the outset. It is often in three or four carefully chosen outputs, perhaps one clearly improved case, one neutral case, and one distorted case, that the real behaviour of a simplification system becomes visible. That kind of example-based discussion is likely to matter as much as the score table.

## References

[1]: J. Bakker and J. Kamps, “Cochrane-auto: An Aligned Dataset for the Simplification of Biomedical Abstracts,” *Proceedings of the Third Workshop on Text Simplification, Accessibility and Readability (TSAR 2024)*, 2024. Available: <https://aclanthology.org/2024.tsar-1.5/>

[2]: J. Bakker and J. Kamps, “Cochrane-auto: An Aligned Dataset for the Simplification of Biomedical Abstracts” (PDF), 2024. Available: <https://aclanthology.org/2024.tsar-1.5.pdf>

[3]: F. Alva-Manchego et al., “ASSET: A Dataset for Tuning and Evaluation of Sentence Simplification Models with Multiple Rewriting Transformations,” ACL 2020. Available: <https://arxiv.org/abs/2005.00481>

[4]: Cochrane-auto presentation slides describing aligned biomedical abstracts and lay summaries. Available: <http://simpletext-project.com/2024/en/slides/Cochrane-auto.pdf>

[5]: UK Health Research Authority, “Writing a plain language (lay) summary of your research findings,” 2025. Available: <https://www.hra.nhs.uk/planning-and-improving-research/best-practice/writing-plain-language-lay-summary-your-research-findings/>

[6]: MRCT Center, “Plain Language - Health Literacy in Clinical Research,” 2025. Available: <https://mrctcenter.org/health-literacy/tools/overview/plain-language/>

[7]: “Do Text Simplification Systems Preserve Meaning? A Human Evaluation Framework for Meaning Preservation,” *Transactions of the Association for Computational Linguistics*, 2024. Available: <https://aclanthology.org/2024.tacl-1.24.pdf>

[8]: “Using ChatGPT to Improve the Presentation of Plain Language Summaries,” 2025. Available: <https://pmc.ncbi.nlm.nih.gov/articles/PMC11939027/>

[9]: “Online Plain Language Tool and Health Information Quality,” 2024. Available: <https://pmc.ncbi.nlm.nih.gov/articles/PMC11581667/>

[10]: “Text Simplification Using Transformer and BERT,” 2023. Available: <https://www.techscience.com/cmc/v75n2/52031/html>

[11]: “Large Language Models for Biomedical Text Simplification,” 2024. Available: <https://arxiv.org/html/2408.03871v2>

[12]: Hugging Face, “facebook/asset dataset card,” 2023. Available: <https://huggingface.co/datasets/facebook/asset>

[13]: “Considering Human Interaction and Variability in Automatic Text Simplification,” TSAR 2024. Available: <https://aclanthology.org/2024.tsar-1.6.pdf>

[14]: “Document-level Text Simplification with Coherence Evaluation,” TSAR 2023. Available: <https://aclanthology.org/2023.tsar-1.9.pdf>

[15]: “Biomedical Text Simplification Models Trained on Aligned Abstracts,” TREC 2024. Available: <https://trec.nist.gov/pubs/trec33/papers/UAmsterdam.plaba.pdf>

[16]: JanB100, “cochrane-auto GitHub repository,” 2024. Available: <https://github.com/JanB100/cochrane-auto>

[17]: Dataset card or mirror information for ASSET. Available: <https://www.atyun.com/datasets/info/asset.html?lang=en>

[18]: “Document-level Simplification and Illustration Generation aimed at enhancing information accessibility,” TSAR 2025. Available: <https://aclanthology.org/2025.tsar-1.2/>

[19]: GEM Benchmark, “cochrane-simplification data card.” Available: <https://www.dei.unipd.it/~faggioli/temp/clef2025/paper_344.pdf>

[20]: “Automated Lay Language Summarization of Biomedical Scientific Reviews,” 2020. Available: <https://arxiv.org/pdf/2012.12573.pdf>

[21]: ACL 2020 virtual conference page for ASSET. Available: <https://virtual.acl2020.org/paper_main.424.html>

[22]: University of Amsterdam repository mirror for Cochrane-auto PDF. Available: <https://pure.uva.nl/ws/files/243722458/2024.tsar-1.5.pdf>

[23]: C. Jiang et al., “Neural CRF Model for Sentence Alignment in Text Simplification,” ACL 2020. Available: <https://aclanthology.org/2020.acl-main.709/>

[24]: K. Papineni et al., “BLEU: a Method for Automatic Evaluation of Machine Translation,” ACL 2002. Available: <https://aclanthology.org/P02-1040/>

[25]: PDF version of BLEU paper. Available: <https://dl.acm.org/doi/pdf/10.3115/1073083.1073135>

[26]: DBLP entry for BLEU paper. Available: <https://dblp.org/rec/conf/acl/PapineniRWZ02.html>

[27]: C.-Y. Lin, “ROUGE: A Package for Automatic Evaluation of Summaries,” 2004. Available: <https://aclanthology.org/W04-1013/>

[28]: W. Xu et al., “Optimizing Statistical Machine Translation for Text Simplification,” TACL 2016. Available: <https://aclanthology.org/Q16-1029/>


## References

[1]: J. Bakker and J. Kamps, “Cochrane-auto: An Aligned Dataset for the Simplification of Biomedical Abstracts,” *Proceedings of the Third Workshop on Text Simplification, Accessibility and Readability (TSAR 2024)*, 2024. Available: <https://aclanthology.org/2024.tsar-1.5/>

[2]: J. Bakker and J. Kamps, “Cochrane-auto: An Aligned Dataset for the Simplification of Biomedical Abstracts” (PDF), 2024. Available: <https://aclanthology.org/2024.tsar-1.5.pdf>

[3]: F. Alva-Manchego et al., “ASSET: A Dataset for Tuning and Evaluation of Sentence Simplification Models with Multiple Rewriting Transformations,” ACL 2020. Available: <https://arxiv.org/abs/2005.00481>

[4]: Cochrane-auto presentation slides describing aligned biomedical abstracts and lay summaries. Available: <http://simpletext-project.com/2024/en/slides/Cochrane-auto.pdf>

[5]: UK Health Research Authority, “Writing a plain language (lay) summary of your research findings,” 2025. Available: <https://www.hra.nhs.uk/planning-and-improving-research/best-practice/writing-plain-language-lay-summary-your-research-findings/>

[6]: MRCT Center, “Plain Language - Health Literacy in Clinical Research,” 2025. Available: <https://mrctcenter.org/health-literacy/tools/overview/plain-language/>

[7]: “Do Text Simplification Systems Preserve Meaning? A Human Evaluation Framework for Meaning Preservation,” *Transactions of the Association for Computational Linguistics*, 2024. Available: <https://aclanthology.org/2024.tacl-1.24.pdf>

[8]: “Using ChatGPT to Improve the Presentation of Plain Language Summaries,” 2025. Available: <https://pmc.ncbi.nlm.nih.gov/articles/PMC11939027/>

[9]: “Online Plain Language Tool and Health Information Quality,” 2024. Available: <https://pmc.ncbi.nlm.nih.gov/articles/PMC11581667/>

[10]: “Text Simplification Using Transformer and BERT,” 2023. Available: <https://www.techscience.com/cmc/v75n2/52031/html>

[11]: “Large Language Models for Biomedical Text Simplification,” 2024. Available: <https://arxiv.org/html/2408.03871v2>

[12]: Hugging Face, “facebook/asset dataset card,” 2023. Available: <https://huggingface.co/datasets/facebook/asset>

[13]: “Considering Human Interaction and Variability in Automatic Text Simplification,” TSAR 2024. Available: <https://aclanthology.org/2024.tsar-1.6.pdf>

[14]: “Document-level Text Simplification with Coherence Evaluation,” TSAR 2023. Available: <https://aclanthology.org/2023.tsar-1.9.pdf>

[15]: “Biomedical Text Simplification Models Trained on Aligned Abstracts,” TREC 2024. Available: <https://trec.nist.gov/pubs/trec33/papers/UAmsterdam.plaba.pdf>

[16]: JanB100, “cochrane-auto GitHub repository,” 2024. Available: <https://github.com/JanB100/cochrane-auto>

[17]: Dataset card or mirror information for ASSET. Available: <https://www.atyun.com/datasets/info/asset.html?lang=en>

[18]: “Document-level Simplification and Illustration Generation aimed at enhancing information accessibility,” TSAR 2025. Available: <https://aclanthology.org/2025.tsar-1.2/>

[19]: GEM Benchmark, “cochrane-simplification data card.” Available: <https://www.dei.unipd.it/~faggioli/temp/clef2025/paper_344.pdf>

[20]: “Automated Lay Language Summarization of Biomedical Scientific Reviews,” 2020. Available: <https://arxiv.org/pdf/2012.12573.pdf>

[21]: ACL 2020 virtual conference page for ASSET. Available: <https://virtual.acl2020.org/paper_main.424.html>

[22]: University of Amsterdam repository mirror for Cochrane-auto PDF. Available: <https://pure.uva.nl/ws/files/243722458/2024.tsar-1.5.pdf>

[23]: C. Jiang et al., “Neural CRF Model for Sentence Alignment in Text Simplification,” ACL 2020. Available: <https://aclanthology.org/2020.acl-main.709/>

[24]: K. Papineni et al., “BLEU: a Method for Automatic Evaluation of Machine Translation,” ACL 2002. Available: <https://aclanthology.org/P02-1040/>

[25]: PDF version of BLEU paper. Available: <https://dl.acm.org/doi/pdf/10.3115/1073083.1073135>

[26]: DBLP entry for BLEU paper. Available: <https://dblp.org/rec/conf/acl/PapineniRWZ02.html>

[27]: C.-Y. Lin, “ROUGE: A Package for Automatic Evaluation of Summaries,” 2004. Available: <https://aclanthology.org/W04-1013/>

[28]: W. Xu et al., “Optimizing Statistical Machine Translation for Text Simplification,” TACL 2016. Available: <https://aclanthology.org/Q16-1029/>
