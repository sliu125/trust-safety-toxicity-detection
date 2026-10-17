# Trust & Safety Toxicity Detection with Bias Analysis

## Status
Work in Progress

Current stage: Manual Error Analysis completed; Identity Subgroup Model Bias Analysis is next.

## Overview
This project explores toxicity detection for online comments, with a focus on model performance, error analysis, and potential differences across identity-related subgroups.

The project starts with an interpretable TF-IDF + Logistic Regression baseline and examines not only overall classification performance, but also the types of mistakes the model makes.

## Dataset
Jigsaw Unintended Bias in Toxicity Classification.

For initial development, I use the first 200,000 rows of the training dataset.

The original dataset is not included in this repository.

The binary toxicity label is defined as:

- Toxic: target >= 0.5
- Non-toxic: target < 0.5

The sampled dataset contains approximately:

- 92.3% non-toxic comments
- 7.7% toxic comments

## Current Workflow
- Exploratory data analysis
- Class imbalance analysis
- Identity subgroup exploration
- TF-IDF text representation
- Logistic Regression baseline
- Class-weight experiment
- Precision / Recall / F1 evaluation
- Prediction threshold tuning
- False-positive and false-negative analysis
- Manual error categorization
- Identity subgroup model bias analysis — next

## Baseline Model

A TF-IDF representation is used with Logistic Regression.

At the default classification threshold of 0.5, the validation results were approximately:

- Precision: 0.84
- Recall: 0.33
- F1: 0.47

The model achieved high precision but missed a large proportion of toxic comments.

A class-weight experiment increased recall but substantially reduced precision.

## Threshold Tuning

Prediction probabilities were evaluated across different classification thresholds.

The F1-optimal threshold selected on the validation set was approximately:

- Threshold: 0.198
- Precision: 0.59
- Recall: 0.62
- F1: 0.61

This improved the balance between precision and recall compared with the default 0.5 threshold.

Because the threshold was selected and evaluated on the same validation set, these results should not be interpreted as final unbiased test performance. A separate held-out test set will be added later.

## Error Analysis

At the selected validation threshold, the confusion matrix was:

- True Negatives: 35,629
- False Positives: 1,295
- False Negatives: 1,176
- True Positives: 1,900

To better understand model behavior, I manually reviewed random samples of:

- 30 false positives
- 30 false negatives

### False Positive Patterns

Among the 30 manually reviewed false positives:

- Strong negative wording: 11 (36.7%)
- Identity-sensitive terms: 6 (20.0%)
- Political context: 6 (20.0%)
- Label ambiguity: 4 (13.3%)
- Violence/crime context: 3 (10.0%)

The reviewed false positives suggest that the model can sometimes assign excessive toxicity risk to comments containing strongly negative vocabulary, identity-related language, political discussion, or violence/crime terminology even when the comment is labeled non-toxic.

### False Negative Patterns

Among the 30 manually reviewed false negatives:

- Sarcasm: 9 (30.0%)
- Weak or sparse toxic signals: 7 (23.3%)
- Unusual or figurative wording: 5 (16.7%)
- Long-context comments: 4 (13.3%)
- Label ambiguity: 3 (10.0%)
- Implicit insult: 1 (3.3%)
- Context-dependent toxicity: 1 (3.3%)

Sarcasm and weak or sparse toxic signals accounted for 53.3% of the manually reviewed false-negative sample.

The reviewed false negatives suggest that the TF-IDF + Logistic Regression model has difficulty with toxicity that depends on sarcasm, indirect language, unusual or figurative phrasing, and localized toxic expressions within longer comments.

These categories are qualitative observations from manually reviewed samples and should not be interpreted as overall error rates or causal explanations of model behavior.

## Preliminary Findings

The baseline model shows two different types of failure patterns:

**False positives**
- Strong lexical cues can sometimes trigger toxicity predictions even when the overall comment is non-toxic.
- Identity-sensitive, political, and violence-related vocabulary appears frequently in the reviewed false-positive sample.

**False negatives**
- Toxicity expressed through sarcasm, indirect language, unusual phrasing, or relatively weak lexical signals is more difficult for the model to detect.
- These limitations motivate later comparison with contextual language models such as Transformers.

## Next Steps
- Evaluate false-positive and false-negative rates across identity subgroups
- Compare subgroup precision, recall, FPR, and FNR
- Investigate potential model bias across identity-related groups
- Add a held-out test set
- Compare the TF-IDF baseline with Transformer-based models
- Explore model deployment
