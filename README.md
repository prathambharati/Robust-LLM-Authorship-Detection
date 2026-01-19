# Robust LLM Authorship Detection Under Human Editing
Overview

The rapid adoption of large language models (LLMs) such as ChatGPT has raised concerns around AI-generated content in academic and professional settings. A common belief is that human editing or “humanisation” of AI-generated text can reliably evade AI detection systems.

This project investigates that assumption in a controlled and rigorous manner. Rather than focusing solely on raw classification accuracy, we study how AI authorship detection behaves under different levels of human editing, and what tradeoffs arise when attempting to reduce false accusations of human-written text.

The key goal of this work is not to build the most complex detector, but to understand robustness, failure modes, and decision tradeoffs in AI authorship detection.

## Research Questions

- This project is driven by the following questions:

  1. Does AI-generated text remain detectable after light and heavy human editing?

  2. What errors are introduced when detection systems become more aggressive?

  3. Can decision threshold tuning reduce false accusations of human-written text while maintaining reasonable AI detection performance?

## Key Contributions

- Designed a leakage-free evaluation framework using prompt-grouped train/test splits.

- Analyzed robustness of AI authorship detection under controlled light and heavy edits.

- Identified false positives on human writing as the dominant failure mode.

- Demonstrated how threshold tuning allows explicit control over the tradeoff between AI recall and human recall.

- Combined quantitative metrics with qualitative lineage analysis to explain model behavior.

## Dataset Description

The dataset consists of text from three domains:

- WP_LLMs: Wikipedia-style explanatory writing

- Essay_LLMs: Academic and essay-style responses

- Reuters_LLMs: News-style informational writing

Each AI-generated response is associated with:

- none → original AI output

- light → lightly edited version

- heavy → heavily edited version

Each human-written response appears only as none, serving as a realistic baseline.

Dataset Integrity Checks

- Every AI response has exactly three variants (none, light, heavy)

- The dataset is balanced across labels and edit levels

- No missing text samples

## Controlling for Confounders

Several factors could bias authorship detection results. We explicitly account for them:

- Prompt leakage : Prevented using a prompt-grouped train/test split with zero overlap.

- Domain effects : Mitigated by evaluating across multiple writing domains.

- Length and verbosity differences : Analyzed via word-count distributions, showing substantial overlap between AI and human text.

- Editing realism : Edits are simulated and may not capture all real-world human revision strategies.

- Classifier conservatism : Explicitly analyzed through decision threshold tuning.

## Experimental Setup
Train/Test Split

- Rather than randomly splitting rows, we split the data by prompt ID to ensure that the model never sees the same prompt in both training and testing. This avoids memorization and enforces true generalization.

  - Train size: ~80%

  - Test size: ~20%

  - Prompt overlap: 0
 
## Model

We use a simple but interpretable baseline:

1. TF-IDF features

2. Logistic Regression classifier

This choice intentionally prioritizes analysis and interpretability over model complexity.

## Results
Overall Performance

- The baseline model achieves strong AI detection performance but exhibits poor recall on human-written text, indicating a conservative decision boundary.

- Robustness Under Editing

- AI recall remains consistently high across all edit levels:

  - none: high AI recall

  - light: high AI recall

  - heavy: high AI recall
 
This indicates that editing alters surface phrasing but does not reliably remove deeper stylistic signals used by the detector.

## Qualitative Lineage Analysis

By inspecting original, lightly edited, and heavily edited versions of the same AI response, we observe that:

- Word choice changes significantly

- Structural and explanatory patterns largely persist

This explains why detection remains effective even after heavy editing.

## Threshold Tuning: Operating Point Analysis

The default classification threshold prioritizes AI recall at the cost of falsely labeling human-written text as AI.

To address this, we perform a threshold sweep and analyze tradeoffs between:

- AI recall

- Human recall

- Macro-averaged F1 score

## Key Finding

- At a threshold of approximately 0.60:

- Human recall improves from ~34% to ~65%

- AI recall remains reasonably high (~88%)

- Macro F1 score is maximized

## Key Takeaways

- Human editing alone does not reliably erase AI authorship signals.

- Robust detection systems tend to become conservative and over-label human writing as AI.

- Threshold selection plays a critical role in balancing robustness and fairness.

- Over-aggressive detection risks false accusations in real-world educational settings.

## Limitations and Future Work

- Editing transformations are simulated and may not reflect natural human rewriting.

- The study focuses on aggregate robustness rather than per-model attribution.

- Future work could explore discourse-level rewriting, personal narrative injection, or adaptive thresholds based on application context.

## Reproducibility

- All experiments use fixed random seeds.

- Prompt-grouped splitting ensures leakage-free evaluation.

- The notebook can be run end-to-end in Google Colab.

Note: When running locally, replace files.upload() with direct file paths.

## Project Motivation

This project was built as a personal research-oriented study to better understand the practical limitations, risks, and tradeoffs of AI authorship detection systems, particularly in academic and educational contexts.
This demonstrates that AI authorship detection is a decision-making problem, not just a modeling problem.
