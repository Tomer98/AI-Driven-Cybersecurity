# 🧠 Artificial Intelligence-Driven Cybersecurity
**Coursework & Capstone Project Portfolio**

This repository contains hands-on labs and the final capstone project for the **Artificial Intelligence-Driven Cybersecurity** course. This portfolio demonstrates the integration of machine learning, deep learning, and automation to detect and prevent sophisticated cyber threats.

## 🧑‍💻 Student
* **Tomer Barel**
* **Roi Baly**
* **Michael Tsegaye**

---

# 🛡️ Project: Unsupervised Spear Phishing Anomaly Detection

## 📖 Project Overview
This project implements a **Proof-of-Concept (PoC) offline pipeline** designed to detect advanced "Spear Phishing" attacks. Unlike traditional systems that rely on technical signatures or blacklisted URLs, this system analyzes the **semantic meaning** of email content to identify anomalies.

By training a **Deep Autoencoder** on text embeddings of "normal" business communications (the Enron Corpus), the model learns a "Safe Fingerprint." It flags any semantic deviation—such as social engineering attempts—without requiring prior knowledge of the attack, enabling **Zero-Day detection**.

## 🎯 Project Goals
* **Primary Objective:** Develop an Unsupervised Zero-Day Detection System based on semantic context rather than technical signatures.
* **Business Metric:** Maintain a low False Positive Rate (FPR) to ensure business continuity while maximizing Recall.
* **Security Metric:** Detect "Zero-Day" attacks where malicious intent is hidden in social engineering language rather than a known virus or link.

## 🏗️ System Architecture & Workflow
The system operates as a five-stage pipeline:

1. **Data Ingestion:** Large-scale ingestion of the Enron Email Dataset (Safe) and Phishing datasets.
2. **Text Vectorization:** Converting raw email text into 384-dimensional dense vectors using **SentenceTransformers (all-MiniLM-L6-v2)**.
3. **The Brain (Autoencoder):** A Deep Neural Network architecture (384 -> 128 -> 64 -> **32 (Bottleneck)** -> 64 -> 128 -> 384) that learns to reconstruct normal emails.
4. **Statistical Thresholding:** Anomaly detection based on **Reconstruction Error**. If the error exceeds the mathematical boundary ($Threshold = \mu + \alpha \cdot \sigma$), the email is flagged.
5. **Explainable AI (xAI):** A **Token Ablation** module that identifies which specific words in a flagged email caused the high anomaly score, providing "transparency" for security analysts.

## ⚠️ Known Limitations & Boundaries
* **Scope:** The system focuses on **Subject Lines and Body Text** and does not currently analyze PDF/EXE attachments or OCR-based image text.
* **Adversarial Risk:** Sophisticated attackers may attempt **Evasion Attacks** by mimicking legitimate organizational language to lower the reconstruction error.
* **Data Privacy:** To comply with GDPR/Privacy standards, PII within the training data must be anonymized.

## 🚀 User Instructions
1. **Setup:** Ensure you have a Python environment with `torch`, `sentence-transformers`, and `pandas` installed.
2. **Data:** Place your `processed_emails.csv` in the root directory or mount via Google Drive.
3. **Execution:** Run the Jupyter Notebook stages sequentially to train the model and generate the performance Confusion Matrix.

## 📄 Documentation
* [Full Project README](Project/9.%20Documentation/README.md) — detailed write-up covering data pipeline, model training, explainability, hybrid architecture, and adversarial testing results.
* [Retrospective & Model Improvement](Project/10.%20Retrospective%20&%20Model%20Improvement/README.md) — analysis of Stage 5 limitations and V2 improvements with a 2:1 training ratio.

---

# 🧪 Labs


## 🧪 Labs Completed
| Lab # | Description | Link |
| :--- | :--- | :--- |
