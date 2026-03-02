# 📊 Stage 3: Data Collection and Dataset Preparation

## 3.1 Data Source & Cleaning
The project utilizes two primary data sources to simulate a corporate email environment:
* **Normal Traffic:** The Enron Email Corpus (~515,000 legitimate business emails).
* **Attack Traffic:** A multi-source Phishing Dataset (~18,000 malicious samples).

**Cleaning Process:**
* **Header Removal:** Stripped technical SMTP headers to focus on semantic content.
* **HTML/CSS Sanitization:** Removed non-textual noise (tags, styles) that interferes with NLP embeddings.
* **Defanging (Safety):** Implemented a safety protocol to convert `http` to `hxxp`, preventing accidental execution of malicious links by developers.

## 3.2 Professional Data Splitting
To support **Unsupervised Anomaly Detection**, the data was split to ensure the model only learns from "Safe" patterns during the training phase:

| Dataset | Row Count | Composition | Purpose |
| :--- | :--- | :--- | :--- |
| **Training Set** | 370,951 | 100% Normal | To build the "Safe Semantic Fingerprint". |
| **Validation Set** | 41,217 | 100% Normal | To monitor for overfitting during model training. |
| **Test Set** | 121,693 | Mixed (Normal + All Phishing) | To simulate a real-world attack battlefield. |

**Outcome:** These sets are exported as individual CSV files to ensure a consistent, repeatable pipeline for the Modeling (Stage 5) and Security Testing (Stage 6) phases.