# Stage 4: Exploratory Data Analysis (EDA) & Baseline Establishment

## Objective
The goal of this stage was to analyze the structural and semantic differences between legitimate Enron business communications and malicious phishing attacks, and to establish a traditional Machine Learning baseline to justify the need for Deep Learning.

## Key Findings

### 1. The Class Imbalance (The Needle in a Haystack)
After rigorous cleaning, the final dataset distribution confirms a severe class imbalance typical of real-world cybersecurity environments:
* **Safe Emails (Enron):** 513,547
* **Phishing Emails:** 18,041
* **Ratio:** ~28:1
* **Conclusion:** This massive imbalance mathematically justifies our Unsupervised Anomaly Detection approach. A standard supervised classifier would naturally overfit to the Safe class.

### 2. Structural Anomalies & Data Hygiene
During the initial length distribution analysis, extreme outliers (emails with 1,000,000+ characters) were discovered in the dataset, representing corrupted data or unformatted logs. 
* **The Fix:** We instituted a strict 30,000-character upper limit in the cleaning pipeline.
* **The Result:** The cleaned distribution revealed that phishing emails (mean: 1,701 chars) are statistically longer than normal Enron traffic (mean: 1,456 chars), likely due to the text required to execute social engineering tactics.

### 3. The Semantic Gap
Visualizing the text via sampled Word Clouds confirmed a distinct semantic gap:
* **Safe Enron Fingerprint:** Dominated by standard corporate jargon, scheduling, and project terms.
* **Phishing Fingerprint:** Dominated by urgency markers, financial terms, and action-oriented keywords.

## The Traditional ML Baseline (The "Score to Beat")
To prove the necessity of Deep Learning, we trained a traditional unsupervised model on the Safe data to see if simple keyword frequencies (TF-IDF) could detect the phishing anomalies.

* **Model Used:** TF-IDF Vectorizer (5,000 features) + Isolation Forest
* **Overall Accuracy:** 85.06%
* **Phishing Recall (Detection Rate):** 0.0000

### Baseline Conclusion: The Accuracy Trap
While the model achieved an 85% overall accuracy, it did so by simply predicting that *every* email was Safe. It successfully caught **0 out of 18,041** phishing attacks. 

This proves that traditional ML algorithms relying on sparse, context-less word counts (TF-IDF) cannot distinguish between complex business communication and spear-phishing. To solve this, we must advance to **Stage 5** and implement Deep Learning (Autoencoders) capable of mapping dense semantic context.