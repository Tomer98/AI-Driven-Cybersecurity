# 📊 Stage 3: Data Collection and Dataset Preparation

## 3.1 Data Source & Cleaning
The project utilizes two primary data sources to simulate a corporate email environment:
* **Normal Traffic:** The Enron Email Corpus (~513,500 legitimate business emails after cleaning).
* **Attack Traffic:** A multi-source Phishing Dataset (~18,000 malicious samples after cleaning).

**Cleaning & Preprocessing Pipeline:**
* **Header Removal:** Stripped technical SMTP headers to focus purely on semantic text content.
* **HTML/CSS Sanitization:** Removed non-textual noise (tags, styles) that interferes with NLP embeddings.
* **Length Constraints (Outlier Removal):** Applied strict character boundaries (10 < length ≤ 30,000). This removed "empty shell" emails lacking semantic context and purged massive corrupted logs/Base64 attachments that would otherwise crash Transformer memory limits.
* **Memory Optimization:** Forced explicit string typing (`dtype=str`) and robust null-value handling during ingestion to prevent Pandas memory spikes across the half-million rows.
* **Defanging (Safety):** Implemented a safety protocol to convert `http` to `hxxp`, preventing accidental execution of malicious links by developers.

## 3.2 Professional Data Splitting
To support **Unsupervised Anomaly Detection**, the data was split to ensure the model only learns from "Safe" patterns during the training phase:

| Dataset | Row Count | Composition | Purpose |
| :--- | :--- | :--- | :--- |
| **Training Set** | 369,753 | 100% Normal | To build the "Safe Semantic Fingerprint". |
| **Validation Set** | 41,084 | 100% Normal | To monitor for overfitting during model training. |
| **Test Set** | 120,751 | Mixed (Normal + All Phishing) | To simulate a real-world attack battlefield. |

**Outcome:** These sets are exported as individual CSV files into a secure `Clean_data` directory to ensure a consistent, repeatable pipeline for the Modeling (Stage 5) and Security Testing (Stage 6) phases.
