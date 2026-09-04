# Multilabel Emotion Classification from Audio Features

## Project Overview

This project explores multilabel classification on the MULAN Emotions dataset.

Each song can belong to more than one emotion category, so the task is not a standard multiclass classification problem. Instead, each observation can have several positive labels at the same time.

The dataset contains 72 numerical audio features and six emotion labels:

- amazed-surprised
- happy-pleased
- relaxing-calm
- quiet-still
- sad-lonely
- angry-aggressive

The main goal of the project was to compare different modeling approaches, evaluate how well they handle the multilabel structure, and investigate why some emotion labels were more difficult to predict than others.

---

## Project Approach

The project followed these main stages:

1. Data exploration and preprocessing
2. Logistic Regression baseline
3. Random Forest baseline
4. Comparison between independent and dependent multilabel approaches
5. Detailed analysis of the `happy-pleased` label
6. Random Forest refinement using Classifier Chains and threshold tuning
7. Final evaluation

---

## Models Tested

### Logistic Regression + One-vs-Rest

A separate Logistic Regression classifier was trained for each emotion label.

This provided a simple linear baseline and treated each emotion independently.

### Random Forest + One-vs-Rest

Random Forest was used to test whether nonlinear relationships between the audio features and emotion labels could improve performance.

Each emotion was still predicted independently.

### Random Forest + Classifier Chain

A Classifier Chain was used to allow predictions from earlier emotion labels to influence later labels.

This was useful because emotion labels may not be independent from one another.

The Classifier Chain improved overall multilabel performance compared with the independent Random Forest approach.

---

## Evaluation

The models were evaluated using cross-validation and several complementary metrics:

- Macro F1-score
- Micro F1-score
- Precision
- Recall
- Per-label F1-score
- Hamming loss
- Subset accuracy
- Confusion matrices
- ROC-AUC

---

## Key Findings

Logistic Regression outperformed the independent Random Forest model on both macro and micro F1-score, while the Random Forest achieved lower Hamming loss and higher subset accuracy.
Random Forest tended to be more conservative when predicting positive labels. This often resulted in higher precision but lower recall.

One label, `happy-pleased`, remained particularly difficult to predict.

Further analysis showed that:

- no individual feature strongly explained `happy-pleased`;
- Mutual Information scores were generally low;
- Random Forest feature importance was distributed across many features;
- permutation importance showed little dependence on any single feature;
- the signal for this label therefore appeared to be relatively weak, distributed, redundant, or dependent on combinations of features.

The Random Forest Classifier Chain achieved an ROC-AUC of approximately 0.70 for `happy-pleased`, despite having a relatively poor F1-score at the default classification threshold.

This suggested that the model had learned some useful ranking information, but its default decision threshold was too conservative.

Moving `happy-pleased` to the end of the Classifier Chain slightly improved both its F1-score and ROC-AUC.

Threshold tuning produced a much larger improvement, increasing the `happy-pleased` F1-score to approximately 0.56 in the training set. 

---

## Main Takeaway

One of the main lessons from this project was that poor classification performance does not always mean that a model has failed to learn useful information.

For `happy-pleased`, the Random Forest model showed reasonable ranking ability through ROC-AUC, but the default threshold did not convert those probability estimates into effective positive predictions.

Threshold tuning therefore became an important part of the final model refinement.

The project also highlighted the importance of investigating model behaviour before simply moving to more complex algorithms.

---

## Repository Structure

```text
01_data_exploration.ipynb
03-1_modeling_logistic-regression.ipynb
03-2_modeling_random-tree.ipynb
03-2-1_random-forest_tuning.ipynb
04_feature_analysis_happy-pleased.ipynb
README.md
CONCLUSIONS.md
