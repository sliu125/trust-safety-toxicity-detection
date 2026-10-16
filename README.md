# Trust & Safety Toxicity Detection with Bias Analysis

## Status
Work in Progress

## Overview
This project explores toxicity detection for online comments, with a focus on model performance, error analysis, and potential differences across identity-related subgroups.

## Dataset
Jigsaw Unintended Bias in Toxicity Classification.

For initial development, I use the first 200,000 rows of the training dataset.

The original dataset is not included in this repository.

## Current Workflow
- Exploratory data analysis
- Class imbalance analysis
- Identity subgroup analysis
- TF-IDF text representation
- Logistic Regression baseline
- Class-weight experiment
- Precision / Recall / F1 evaluation
- Prediction threshold tuning
- Error analysis

## Current Results
The validation set contains approximately 7.7% toxic comments.

Baseline Logistic Regression at the default threshold:
- Precision: 0.84
- Recall: 0.33
- F1: 0.47

After threshold tuning:
- Selected validation threshold: ~0.198
- Precision: ~0.59
- Recall: ~0.62
- F1: ~0.61

## Error Analysis
Initial review suggests that false positives often involve identity-related or politically sensitive vocabulary, while false negatives frequently involve implicit insults and context-dependent toxicity.

## Next Steps
- Expand error analysis
- Evaluate subgroup false-positive and false-negative rates
- Perform model bias analysis
- Compare with Transformer-based models
- Add a held-out test set
- Explore deployment
