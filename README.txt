🔍 Anomaly Detection in Credit Card Transactions (Unsupervised Learning)
📌 Project Overview

This project applies unsupervised anomaly detection to identify potentially fraudulent credit card transactions without using labels during training. 
The goal is to understand how well unsupervised models can surface rare and suspicious behavior in an extremely imbalanced, 
real-world dataset, and to evaluate their effectiveness using withheld labels.

📂 Dataset

Source: Kaggle – Credit Card Fraud Detection

Transactions: 284,807 (2 days of European cardholders, Sept 2013)

Fraud cases: 492 (~0.17%)

Features:

V1–V28: anonymized PCA components

Time: seconds since first transaction

Amount: transaction value

Label (Class) was hidden during training and used only for evaluation


🎯 Objective

Detect anomalous transactions using unsupervised learning only

Avoid label leakage at all stages of training and thresholding

Evaluate performance using precision–recall metrics appropriate for extreme class imbalance

Analyze what the model detects, what it misses, and why


🛠️ Methodology
1. Exploratory Data Analysis

Examined distributions of Time and Amount

Identified non-uniform transaction density over time

Observed heavy right-skew in transaction amounts

2. Preprocessing

Time-based train/test split to simulate real deployment (train on past, test on future)

RobustScaler applied to all features to handle heavy-tailed distributions

Scaling fit only on training data to prevent leakage

3. Unsupervised Model

Isolation Forest

Trained on scaled training data only

Continuous anomaly scores extracted via decision_function

No internal predictions or contamination assumptions used

4. Thresholding (Label-Free)

Percentile-based thresholding on training scores

Alert rate hypothesis: 1% most anomalous transactions

Same threshold applied to test data

5. Evaluation (Labels Revealed)

Metrics: Precision, Recall, F1, AUPRC

Confusion matrix and Precision–Recall curve

Baseline comparison against random detection

📊 Key Results (Isolation Forest)

Recall (fraud): ~51%

Precision (fraud): ~5.8%

AUPRC: 0.0367

≈ 21× higher than random baseline (~0.0017)

Despite low precision, the model provides strong lift over chance, which is expected in unsupervised settings with extreme imbalance.


🔎 Error Analysis & Insights

Fraud is not the most extreme anomaly

False positives dominate the most extreme anomaly scores

Many fraudulent transactions are only moderately anomalous

Low-amount fraud is often missed

Small transactions blend into dense regions of normal behavior

False positives overlap heavily with normal transactions

Especially at low transaction amounts

Anomaly score ≠ fraud likelihood

The model detects rarity, not malicious intent

Core insight:

Unsupervised anomaly detection identifies unusual behavior, not fraud itself. Fraud is a subtle subset of anomalies.


⚠️ Limitations

No fraud labels used during training by design

PCA features prevent domain-level interpretability

Precision constrained by extreme class imbalance

Threshold selection reflects operational policy, not optimality

Not suitable as a standalone fraud decision system

🏭 Practical Implications

In production, this model would be used as a first-stage filter:

Flag a small percentage of anomalous transactions

Pass flagged cases to supervised models, rules, or human review

Use anomaly detection to surface novel or emerging patterns

✅ Conclusion

Unsupervised anomaly detection provides meaningful signal for fraud detection and significantly outperforms random selection. 
However, because extreme anomalies are often legitimate, 
such models are best used as supporting components within a multi-stage fraud detection pipeline rather than as final decision makers.