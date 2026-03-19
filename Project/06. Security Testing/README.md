# 🛡️ Stage 6: Security Testing, Vulnerability Assessment & Hybrid Inference
**Group 7 | NVIDIA AI-Enhanced Cybersecurity Project**

## Objective
This final stage serves as the "adversarial stress test" for our AI guard, transitioning the Deep Autoencoder from a research environment into a simulated production pipeline. In strict accordance with the project rubric, we evaluated the system against practical attack scenarios (Phishing, Malware, MITM, DoS) and developed a robust, Hybrid "Defense in Depth" architecture to mitigate discovered vulnerabilities.

---

## 💥 1. Systematic Vulnerability Assessment (Red Teaming)
We subjected the AI to a battery of realistic cyber-attacks to evaluate its robustness and feature resilience.

### A. Denial of Service (DoS) & Resource Exhaustion
* **Attack:** Sent a massive 70,000-character text bomb to crash the GPU (OOM error) after removing truncation limits.
* **Result:** **PASS.** The system proved highly resilient, processing the payload in ~0.047 seconds. The AI successfully flagged the structural anomaly (MSE: `0.002056`).

### B. Feature Robustness (Noise & Typosquatting)
* **Attack:** Injected typos ("v3rify", "acc0unt") to test if the AI relies on exact keyword matching.
* **Result:** **PASS.** The AI is robust to noise. It treats adversarial obfuscation as a severe semantic anomaly, triggering a block. 

### C. The "Semantic Blind Spot" (Phishing, Malware, MITM)
* **Attack:** Crafted highly targeted Spear-Phishing links, Executable Malware (`.exe`), and Man-In-The-Middle (MITM) offshore bank account alterations cloaked in legitimate corporate Enron jargon.
* **Result:** **CRITICAL VULNERABILITY FOUND.** The Unsupervised AI passed these emails as `SAFE` (MSE: ~`0.0008` to `0.0012`). 
* **Insight:** The model averages semantic intent. Adversaries can bypass detection by "diluting" malicious payloads with high-frequency benign business tokens.

---

## 🏰 2. The Solution: Hybrid Pipeline (Defense in Depth)
To address the critical vulnerability discovered during Red Teaming, we engineered a **Hybrid Scanning Engine**. This pipeline combines our Deep Learning Autoencoder with a traditional Heuristic Rule Engine (Regex), eliminating the AI's semantic blind spot.

**Final Performance under Hybrid Attack Simulation:**
* **Spear-Phishing:** AI Passed ➡️ **Heuristics Flagged (Urgency + URL)** ➡️ 🔒 BLOCKED
* **Malware (.exe/.zip):** AI Passed ➡️ **Heuristics Flagged (Executable)** ➡️ 🔒 BLOCKED
* **MITM Bank Alteration:** AI Passed ➡️ **Heuristics Flagged (Offshore Routing)** ➡️ 🔒 BLOCKED
* **DoS Text Bomb:** Heuristics Passed ➡️ **AI Flagged (Structural Anomaly)** ➡️ 🔒 BLOCKED

*Conclusion:* The systems perfectly complement each other. Static rules catch explicit malicious artifacts, while the AI catches zero-day structural anomalies and noise.

---

## 🧠 3. Explainable AI (xAI) & Token Ablation
To prove the AI operates on semantic understanding, we developed an xAI Vulnerability Map using token ablation (word-by-word removal). 

| Token Removed | Impact on MSE Score | xAI Insight |
| :--- | :--- | :--- |
| *None (Baseline)* | `0.001431` | Base threshold. |
| **"account."** | `0.001769` | **Trigger:** Removing the core subject drastically confused the model's contextual mapping. |
| **"Please"** | `0.001298` | **Statistical Camouflage:** Removing polite language made the email look *more* anomalous. Attackers use polite/business words as a safety shield to lower suspicion scores. |

---

## 🔌 4. Enterprise Integration (API Webhook Simulation)
To demonstrate production readiness, we simulated an API Webhook integration handling JSON payloads from an external Email Gateway (e.g., MS Exchange). 

**Scenario:** An external vendor sends an overdue invoice containing a malicious `.zip` file and credential harvesting instructions.
* **Gateway Response:** The Hybrid API successfully parsed the payload, identified the specific threat vector, and returned an automated `200 OK` response with a `QUARANTINE` action command and the explicit threat reasons.

---
**Project Status:** COMPLETE. The pipeline successfully operationalizes AI-driven anomaly detection while maintaining resilience against adversarial evasion tactics through a Defense in Depth architecture.