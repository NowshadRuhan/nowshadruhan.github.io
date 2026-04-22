---
title: "Evaluating Fine-Tuned LLM Model For Medical Transcription With Small Low-Resource Languages Validated Dataset"
collection: publications
permalink: /publication/2026-3-25-Evaluating Fine-Tuned LLM Model For Medical Transcription With Small Low-Resource Languages Validated Dataset
excerpt: "This paper is about fixing template issue #693."
date: 2026-3-25
venue: "arxiv"
paperurl: "https://arxiv.org/pdf/2603.24772"
citation: "Mohammed Nowshad Ruhani Chowdhury (2026). &quot;Evaluating Fine-Tuned LLM Model For Medical Transcription With Small Low-Resource Languages Validated Dataset.&quot; <i>Arxiv Journal</i>. 1(3)."
---

Clinical documentation is a critical factor for patient safety, diagnosis, and continuity of care. The administrative burden of EHRs is a significant factor in physician burnout. This is a critical issue for low-resource languages, including Finnish. This study aims to investigate the effectiveness of a domain-aligned **natural language processing (NLP)**; large language model for medical transcription in Finnish by fine-tuning LLaMA 3.1-8B on a small validated corpus of simulated clinical conversations by students at Metropolia University of Applied Sciences. The fine-tuning process for medical transcription used a controlled preprocessing and optimization approach. The fine-tuning effectiveness was evaluated by sevenfold cross-validation. The evaluation metrics for fine-tuned **LLaMA 3.1-8B** were **BLEU = 0.1214**, **ROUGE-L = 0.4982**, and **BERTScore F1 = 0.8230**. The results showed a **low n-gram overlap** but a strong semantic similarity with reference transcripts. This study indicate that fine-tuning can be an effective approach for translation of medical discourse in spoken Finnish and support the feasibility of fine-tuning a privacy-oriented domain-specific large language model for clinical documentation in Finnish. Beside that provide directions for future work.

### Fig. 1. Overview of Fine-Tuned LLM model using Small Validate Dataset.

![llm-flow](https://github.com/NowshadRuhan/nowshadruhan.github.io/blob/4c39fc4f973a3a2003397bfa8a56c7f8aab38956/images/llm-flow.png)

### Fig. 2. K-Fold Cross-Validation (k=7) Structure.

![K-Fold](https://github.com/NowshadRuhan/nowshadruhan.github.io/blob/4c39fc4f973a3a2003397bfa8a56c7f8aab38956/images/2af85a5cadd1799c9288b21683a84669d93e5ee8.png)

### Fig. 3. Model results in k-fold validation.

![results](https://github.com/NowshadRuhan/nowshadruhan.github.io/blob/4c39fc4f973a3a2003397bfa8a56c7f8aab38956/images/9cc41cc21953a1ecbeb88156db6e838926e0161d.png)
