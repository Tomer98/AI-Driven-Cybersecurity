# 🛡️ Stage 5: Morpheus Spear Phishing Detection (Deep Autoencoder)
**Group 7 | NVIDIA AI-Enhanced Cybersecurity Project**

This project implements an AI-native cybersecurity pipeline leveraging Unsupervised Deep Learning (Autoencoders) to detect sophisticated spear-phishing attempts that bypass traditional signature-based security.

---

## 💻 Environment & Hardware Optimization
To satisfy the computational demands of encoding and training on nearly half a million records, this project utilized cloud-based GPU acceleration:
* **Compute Device**: Google Colab Cloud Instance
* **GPU**: NVIDIA T4 Tensor Core GPU (16GB VRAM)

---

## 🛠️ Technical Implementation

### 1. High-Speed Vectorization
Using Hugging Face's `all-MiniLM-L6-v2` SentenceTransformer, we converted unstructured email text into 384-dimensional dense contextual vectors.
* **Training Set**: 369,753 Normal records
* **Validation Set**: 41,084 Normal records
* **Test Set**: 120,751 Mixed records (Normal + Phishing)

### 2. Deep Autoencoder (Semantic Fingerprinting)
We built a multi-layer Autoencoder to mathematically map the "safe fingerprint" of legitimate Enron communications. 
* **Architecture**: Symmetric Bottleneck (384 → 128 → 64 → 32 → 64 → 128 → 384)
* **Training**: 20 Epochs on **Normal-only** data.
* **Generalization**: Successfully prevented overfitting, with Validation Loss closely tracking Training Loss, settling at an impressive `0.0010`.

---

## 📊 Threshold Tuning & Strategic Trade-offs
Detection is triggered by **Reconstruction Error (MSE)**. If an email cannot be reconstructed by the learned fingerprint, it is flagged as an anomaly. 

In cybersecurity, we must balance Threat Detection (Recall) against Alert Fatigue (False Positive Rate). We mapped the performance across multiple percentiles to find the operational "Sweet Spot":

| Metric | Strict (95th Percentile) | Naive (70th Percentile) | 👑 Optimized (80th Percentile) |
| :--- | :--- | :--- | :--- |
| **Recall (Threats Caught)** | 22.3% | 67.2% | **56.1%** |
| **False Positive Rate (FPR)** | 2.0% | 23.5% | **13.7%** |
| **Business Impact** | Misses too many attacks. | IT overwhelmed by false alarms. | **Ideal balance of high detection & manageable alerts.** |

---

## 🧠 Explainable AI (xAI)
For every alert, our model provides a quantitative reason rather than a "black box" guess. 
The **MSE Anomaly Score** represents the explicit degree of deviation from the learned business norm. A high MSE error indicates language, urgency, or intent that structurally does not exist within the legitimate Enron fingerprint, allowing SOC analysts to instantly prioritize the highest-scoring threats.

---

## 📂 Stage 5 Artifacts
- `5.Deep_Learning_Autoencoder.ipynb`: Main modeling, threshold tuning, and evaluation notebook.
- `spear_phishing_ae_weights.pth`: Saved model weights for the pipeline inference phase.