#  Hybrid Fraud Detection: Diffusion + XGBoost

This project presents a **hybrid fraud detection approach** that combines a **Diffusion Model** (unsupervised) and an **XGBoost Classifier** (supervised).  
The goal is to detect fraudulent transactions effectively while minimizing false positives.

---

##  Project Objective

Fraud detection is a constant trade-off between:
- **Unsupervised models** that identify anomalies (high recall but many false alarms),
- **Supervised models** that learn discriminative patterns (high precision but require labeled data).

This project bridges both worlds:

1. **Diffusion Model:** Learns the distribution of normal transactions and assigns an **anomaly score** to each record.  
2. **XGBoost Classifier:** Trained only on suspicious cases to **differentiate true frauds** from false positives.

---

##  Full Pipeline Overview

```text
Raw transactions
        ↓
   Diffusion Model
        ↓
   Anomaly Scores
        ↓
   Threshold (e.g., 99.4 percentile)
        ↓
   Suspicious cases (~1% of data)
        ↓
   XGBoost Classifier
        ↓
   Final Decision: True Fraud / False Positive
```

---
##  Key Results

| Model | Precision | Recall | F1-score | AUC |
|:--------|:-----------:|:----------:|:-----------:|:------:|
| **Diffusion (alone)** | 0.20 | 0.80 | 0.30 | 0.96 |
| **XGBoost (verification)** | 0.91 | 0.91 | 0.91 | 0.998 |
| **Full Pipeline** | 0.97 | 0.91 | 0.94 | 0.998 |

 **Major improvement:** Precision increased from 0.20 → 0.91 while maintaining high recall.

---

##  Technical Details

###  Diffusion Model
- Implemented in PyTorch  
- Linear noise schedule with 1000 steps (β ∈ [1e-4, 0.02])  
- Trained exclusively on **normal transactions**  
- Anomaly score derived from denoising MSE loss

###  XGBoost Model
- Trained on suspicious cases detected by the diffusion model  
- Added feature: `diffusion_score`  
- Optimized using `scale_pos_weight` for class imbalance  
- Achieved **AUC ≈ 0.998**

---

##  Dataset

**Credit Card Fraud Detection Dataset**  
Transactions made by European cardholders in September 2013.

- 284,807 total transactions  
- 492 frauds (≈ 0.172%)  
- Features anonymized via PCA (`V1–V28`) + `Amount`, `Time`

 Source: [Kaggle - Credit Card Fraud Detection](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)

---

##  Tools & Technologies

- **Python 3.10**
- **PyTorch** – Diffusion model  
- **XGBoost** – Supervised classification  
- **scikit-learn** – Metrics, preprocessing  
- **NumPy / Pandas / Matplotlib / Seaborn**

---

##  Critical Analysis

- Excellent performance (**AUC > 0.99**) but requires **validation on new data**.  
- XGBoost was trained on a **small subset (~1%)**, which may introduce variance.  
- Results are promising but should be **confirmed with cross-validation** or an out-of-time test split.

---

##  Next Steps

- Extend to **real-time detection** (API or streaming pipeline).  
- Integrate **explainability tools** (e.g., SHAP) for fraud analyst insights.  
- Experiment with alternative diffusion noise schedules (cosine, variance offset).  
- Add **temporal aggregation features** (e.g., per-client or per-merchant stats).

---

##  Author

**Nawfal Benhamdane** 

Machine Learning Student at CentraleSupélec

📧 [nawfal.benhamdane@student-cs.fr]  
🌐 [LinkedIn](https://linkedin.com/in/nawfal-benhamdane-6298b1285//) | [GitHub](https://github.com/NawfalBenhamdane)

**Mohamed BENKIRANE** 

Machine Learning Student at CentraleSupélec

📧 [mohamed.benkirane@student-cs.fr]  
🌐 [LinkedIn](https://www.linkedin.com/in/benkirane10/) | [GitHub](https://github.com/simobenk)



