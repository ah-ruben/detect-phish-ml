# Phishing Email Detection
 
A text classification pipeline for detecting phishing emails using TF-IDF features and two classic ML models: Multinomial Naive Bayes and Logistic Regression.
 
## Overview
 
Raw email text (subject + body) is cleaned and normalized, then vectorized separately for subject and body using `TfidfVectorizer`. The resulting feature matrix is used to train and compare a Naive Bayes classifier and a Logistic Regression classifier.
 
The trained Logistic Regression model and its fitted vectorizers are saved as `.joblib` artifacts for reuse without retraining.
 
## Pipeline
 
1. **Load & combine data** — raw email datasets are merged into a single labeled dataset (phishing = 1, legitimate = 0).
2. **Text cleaning** — HTML unescaping, HTML tag stripping, email/URL masking, separator and whitespace normalization.
3. **Vectorization** — separate TF-IDF vectorizers fit on subject and body text (fit on training split only, to avoid leakage).
4. **Model training** — Multinomial Naive Bayes and Logistic Regression trained on the same TF-IDF features.
5. **Evaluation** — models are evaluated on a held-out test split, and separately re-evaluated against a fully unseen dataset to check generalization.
6. **Model export** — the fitted Logistic Regression model and both TF-IDF vectorizers are serialized with `joblib` for inference elsewhere.
## Files
 
- `nb_lr_phish_detection.ipynb` — main notebook: preprocessing, training, evaluation
- `phish_lr_model.joblib` — trained Logistic Regression model
- `subject_vectorizer.joblib` / `body_vectorizer.joblib` — fitted TF-IDF vectorizers
- `dataset/` — source and processed email data
## Data Sources
 
- [Phishing and Legitimate Emails Dataset](https://www.kaggle.com/datasets/kuladeep19/phishing-and-legitimate-emails-dataset) — Kaggle, kuladeep19
- [Phishing Email Dataset](https://www.kaggle.com/datasets/naserabdullahalam/phishing-email-dataset) — Kaggle, naserabdullahalam

## Notes
 
- The current model uses TF-IDF features only. A lexicon-based keyword count and NRCLex emotion-scoring step exists in the preprocessing code but is not currently wired into the feature matrix.
- Inference on new data requires reusing the fitted vectorizers (`.transform()`, not `.fit_transform()`) to avoid vocabulary mismatch with the trained model.