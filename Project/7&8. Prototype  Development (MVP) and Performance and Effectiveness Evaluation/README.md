# 🛡️ Stages 7 & 8 | Morpheus Hybrid Guard: AI-Driven Spear Phishing Detection
###  Cybersecurity MVP & Performance Evaluation

This repository documents the evolution of the Morpheus Guard, showcasing a robust **Defense-in-Depth** architecture designed to bridge the "Semantic Gap" between structural AI detection and adversarial social engineering.

---

## 🏗️ Stage 7: Prototype Development (MVP)
The goal of this stage was to transition from experimental models to a functional, real-time traffic processing pipeline.

* **User Interface (Gradio Dashboard)**: A real-time web interface allowing for instant scanning of email content and visual status feedback.
* **Context-Aware Pipeline**: An automated backend that combines high-dimensional semantic embeddings with a heuristic rule engine.
* **Logging & Alerting**: A module providing granular logs, including timestamps, anomaly scores, and specific threat classification (e.g., MITM vs. Phishing).

---

## 🚨 Critical Vulnerability Disclosure: Learning from Failure
During initial testing of the **AI-only model**, we identified critical "Semantic Blind Spots" where attacks were misclassified as safe:

1. **Phishing Bypass**: An urgent credential theft email was scored **0.001523**, falling under our 0.0018 threshold and marked as `✅ CLEAN`.
2. **MITM Fraud**: A social engineering attempt for bank account changes was scored **0.001535** and marked as `✅ CLEAN`.
3. **Reason for Failure**: The AI was "over-fitted" to business jargon. Polite language (e.g., "Please," "Regards") acted as statistical camouflage.

---

## 📊 Stage 8: The Hybrid Evolution & Evaluation
To mitigate these failures, we upgraded the system to a **Hybrid Pipeline** incorporating real-world business context.

### 🛠️ Key Architectural Hardening
* **Context-Aware Whitelisting**: Added a "Trusted Domains" sensor (e.g., Zoom, Microsoft) to prevent **False Positives** when legitimate links are sent with urgent language.
* **Financial Routing Sensor**: Added detection for high-risk financial keywords (e.g., "offshore," "Cayman," "account changed") to block MITM fraud.
* **3-Tier Alerting Logic**:
    * **✅ [CLEAN]**: Score $\le 0.0018$ or Trusted Domain.
    * **⚠️ [WARNING]**: Score $> 0.0018$. Structural anomaly (AI-driven).
    * **🚩 [BLOCKED]**: Explicit rule trigger (Phishing/MITM/Malware).



### 📈 Final Performance Matrix
| Attack Scenario | AI Score | AI Verdict | Hybrid Verdict | Improvement Status |
| :--- | :--- | :--- | :--- | :--- |
| **Normal Business Talk** | 0.00098 | ✅ CLEAN | ✅ CLEAN | Verified Baseline |
| **Urgent Phishing** | 0.00152 | ❌ CLEAN (FAIL) | 🚩 **BLOCKED** | **Fixed via Rules** |
| **MITM Fraud** | 0.00153 | ❌ CLEAN (FAIL) | 🚩 **BLOCKED** | **Fixed via Rules** |
| **Malware Injection** | 0.00109 | ❌ CLEAN (FAIL) | 🚩 **BLOCKED** | **Fixed via Rules** |
| **DoS "Text Bomb"** | 0.00205 | 🚩 BLOCKED | 🚩 **BLOCKED** | Verified AI Anomaly |

---

## 🧠 Stage 6: Explainable AI & Red Teaming
We performed rigorous stress testing to validate the system's decision-making process:
* **Token Ablation**: Visualized how polite business words lower the anomaly score, identifying them as potential adversarial vectors.
* **Robustness**: Verified that character-level noise and typos are correctly identified as structural anomalies by the AI.

---

## 🏁 Final Conclusion
The **Morpheus Hybrid Guard** proves that effective cybersecurity requires more than just sophisticated AI. By integrating Deep Learning for structural anomalies with Heuristic Rules for malicious intent, we created a resilient system that balances high security with business continuity.

## Screenshots
### --------------- Non-Anomaly ------------------
![Clean](/7%20&%208.%20Prototype%20%20Development%20(MVP)%20and%20Performance%20and%20Effectiveness%20Evaluation/screenshots/clean.png)

### ----------------- Suspicious ------------------
![Warning](/7%20&%208.%20Prototype%20%20Development%20(MVP)%20and%20Performance%20and%20Effectiveness%20Evaluation/screenshots/warning.png)

### ----------------- Anomaly ---------------------
![Blocked](/7%20&%208.%20Prototype%20%20Development%20(MVP)%20and%20Performance%20and%20Effectiveness%20Evaluation/screenshots/blocked.png)

### --------------- Non-Anomaly Attachment ------------------
![Clean](/7%20&%208.%20Prototype%20%20Development%20(MVP)%20and%20Performance%20and%20Effectiveness%20Evaluation/screenshots/clean_attachment.png)

### ----------------- Anomaly-Attachment ------------------
![Warning](/7%20&%208.%20Prototype%20%20Development%20(MVP)%20and%20Performance%20and%20Effectiveness%20Evaluation/screenshots/blocked_attachment.png)

### ----------------- MITM ---------------------
![Blocked](/7%20&%208.%20Prototype%20%20Development%20(MVP)%20and%20Performance%20and%20Effectiveness%20Evaluation/screenshots/MITM.png)

### ----------------- Semantic-Anomaly ---------------------
![Blocked](/7%20&%208.%20Prototype%20%20Development%20(MVP)%20and%20Performance%20and%20Effectiveness%20Evaluation/screenshots/semantic.png)
