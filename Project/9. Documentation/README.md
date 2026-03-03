# 🛡️ Morpheus Hybrid Guard: Full Project Documentation
### Stages 1-9: Unsupervised AI Spear Phishing Defense & Security Audit

This repository documents the complete lifecycle of **Morpheus Guard**, an AI-driven anomaly detection system designed to bridge the "Semantic Gap" in email security.

---

## 🏗️ 1. System Architecture
The system implements a **Defense-in-Depth** architecture, moving beyond simple AI toward a robust, production-ready hybrid pipeline.

### Technical Stack
* **Vectorization**: `SentenceTransformer` (all-MiniLM-L6-v2) - converts text into 384-dimensional semantic embeddings.
* **Core AI**: Deep Autoencoder (PyTorch).
    * **Encoder**: 384 ➡️ 128 ➡️ 64 ➡️ 32 (Bottleneck).
    * **Decoder**: 32 ➡️ 64 ➡️ 128 ➡️ 384.
* **Hybrid Layer**: Context-Aware Heuristic Engine (Regex) + Domain Whitelisting.
* **Interface**: Gradio Web UI.

---

## 🔄 2. Workflow Diagram & Documentation
The system process is visualized in the project's **Workflow Diagram**.
![Diagram](screenshots/Diagram.png)

### Diagram Overview
* **Software Used**: Created using **Lucidchart** (or Draw.io) for professional-grade flow visualization.
* **Operational Flow**: Input ➡️ Embedding ➡️ AE Reconstruction ➡️ MSE Calculation ➡️ **Hybrid Security Check** ➡️ Final Decision.

### Required Updates (Optimized Logic)
Our final version improves upon the baseline diagram:
1.  **Threshold Shift**: Increased sensitivity to **0.0018** to reduce False Positives.
2.  **Hybrid Injection**: Added a parallel "Heuristic Rules" track that overrides AI decisions if explicit threats (Phishing/MITM) are detected.

---

## 🛠️ 3. User Instructions
### Installation
1.  **Activate Environment**: `conda activate cyber_ai`.
2.  **Install Dependencies**: `pip install torch gradio sentence-transformers`.

### Running the System
1.  Open `Performance and Evaluation.ipynb`.
2.  Run all cells to load `spear_phishing_ae_weights.pth`.
3.  Access the **Gradio Dashboard** to scan traffic in real-time.

---

## 🚨 4. Security Audit: Learning from Failure
Through rigorous testing, we discovered the **"Semantic Blind Spot"** of unsupervised AI:
* **Initial Fail**: A phishing email with urgent credentials theft scored **0.001523**, passing as `✅ CLEAN` because it mimicked business jargon.
* **MITM Fail**: An offshore account change request scored **0.001535**, also passing as `✅ CLEAN`.
* **The Fix**: Implemented **Context-Aware Rules** and **Whitelist** filtering. The hybrid engine now intercepts these threats immediately.

---

## 🧪 5. xAI & Red Teaming (Stage 6)
* **Token Ablation**: Proved that polite words like "Please" act as statistical camouflage, lowering the suspicion of malicious emails.
* **Robustness**: System correctly flags typos (typosquatting) and character noise as structural anomalies.

---

## 🏁 Final Conclusion
**Morpheus Guard** proves that high-performance cybersecurity requires a **Hybrid Mindset**. By using AI for structural detection and Heuristics for intent mapping, we achieved a **100% block rate** on tested adversarial scenarios without disrupting business continuity.

---
# 🎥 Video Demo
[*[Watch the System in Action]*](https://youtu.be/nnoqyaXLrgw)

