# 🛡️ Unsupervised Spear Phishing Anomaly Detection
### Deep Learning Autoencoders, Sentence Transformers, and Explainable AI (XAI) for Zero-Day Defense

![Python](https://img.shields.io/badge/Python-3.9%2B-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-Deep%20Learning-red)
![MITRE](https://img.shields.io/badge/MITRE-T1566-orange)

---

## 1. Defining the Cybersecurity Problem & Project Goal

### 1.1 The Cybersecurity Problem: The Semantic Gap
The primary threat addressed by this project is the successful evasion of existing email security defenses by sophisticated, low-volume spear phishing attacks. 

* **The Problem:** Traditional filters rely on signatures, blacklisted URLs, or specific keywords (e.g., "Free money").
* **The Gap:** Advanced Spear Phishing attacks (**MITRE ATT&CK T1566.002**) utilize sophisticated Natural Language and Social Engineering techniques to bypass these static filters.
* **The Risk:** These targeted messages pose a critical risk of credential theft and data breach.
* **The Requirement:** There is a critical need for systems that detect **behavioral and semantic anomalies** in email content, rather than just matching predefined rules, to protect against zero-day phishing attempts.

### 1.2 Project Goal
The project aims to validate a novel, unsupervised approach to email security.
* **Goal:** Develop an offline Proof-of-Concept (PoC) pipeline that utilizes an unsupervised deep learning model (Autoencoder) to learn the latent patterns of legitimate organizational email traffic.
* **Mechanism:** The model calculates a reconstruction error for incoming emails; any statistically significant deviation from the learned "normal" semantic fingerprint is flagged as a potential threat.
* **Success Metrics:** Success is measured using a Confusion Matrix to maximize **Recall** (detecting actual phishing) while minimizing the **False Positive Rate (FPR)**.

---

## 2. System Architecture & Methodology

### 2.1 Feature Engineering (Semantic Embeddings)
* **Tool:** Utilization of **Sentence Transformers** (`all-MiniLM-L6-v2`) from HuggingFace.
* **Process:** Raw email subjects and bodies are converted into 384-dimensional dense vector embeddings.
* **Benefit:** This captures the *context* and *intent* (semantics) of the message, providing a nuanced representation beyond simple word frequency.

### 2.2 The Model (Deep Autoencoder)
* **Architecture:** Deep Autoencoder implemented in **PyTorch**.
* **Training Strategy:** The model is trained **exclusively on normal traffic** (e.g., the Enron Corpus) to learn the patterns of "safe" business language.
* **Detection:** When a phishing email is processed, the model fails to reconstruct the unfamiliar malicious semantic pattern, resulting in a **High Reconstruction Error**—the anomaly score.

### 2.3 Explainability (XAI)
To satisfy the requirement for **Explainable AI techniques**, the system includes:
* **Token Ablation:** A module that identifies specific words causing high anomaly scores by removing them one-by-one and measuring the score change.
* **UMAP Visualization:** High-dimensional reduction to visually inspect the physical separation between the "Normal" cluster and "Phishing" outliers.
* **SHAP Analysis:** Local feature importance analysis to explain *why* specific emails were flagged (e.g., semantic features related to "Urgency").

---

## 3. Threat Modeling & Scope

### 3.1 Adversary Thinking
* **Evasion Attacks:** Attackers may craft emails to mimic legitimate content to lower reconstruction errors.
    * *Mitigation:* Robust feature engineering and adversarial retraining.
* **Data Poisoning:** Malicious emails slipped into training data to teach the model that phishing is "normal".
    * *Mitigation:* Rigorous data sanitation and verification pipelines.
* **Model Degradation:** Shifts in organizational communication patterns over time.
    * *Mitigation:* Scheduled retraining and dynamic threshold monitoring.

### 3.2 Project Boundaries (Scope)
* **In Scope:** Semantic analysis of subject lines and bodies; URL link structure feature extraction.
* **Out of Scope:** Attachment analysis (PDF/EXE), OCR on images, or automatic remediation.
* **Safety:** All malicious indicators are **defanged** (e.g., `hxxp://`) and code is run in isolated environments.

---

## ⚙️ Tech Stack
* **Language:** Python
* **Deep Learning:** PyTorch
* **NLP:** HuggingFace SentenceTransformers
* **Visualization:** Matplotlib, Seaborn, UMAP, SHAP