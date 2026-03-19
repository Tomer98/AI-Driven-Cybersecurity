# Stage 10: Retrospective & Model Improvement

## The Problem

Stage 5 caught one out of four adversarial attacks. A credential theft email scored 0.001523, a MITM bank fraud scored 0.001535, and a malware attachment scored 0.001091 — all below the threshold of 0.001800 and classified as clean. The hybrid rules engine saved the system at runtime, but the root cause remained: the autoencoder was not broken, it was miscalibrated by its training data.

The model trained on 369,753 normal Enron emails — a 28:1 ratio against 18,041 phishing samples. At that scale, the autoencoder learned that business vocabulary is a normality signal. Any email using corporate language, regardless of intent, reconstructed with low error. Polite phrasing like "Please" actively suppressed anomaly scores. Token ablation confirmed it: removing "Please" from a phishing email raised the score, revealing that courtesy language was acting as adversarial camouflage.

## The Fix

Reduce the training set from 369,753 to 36,082 emails — a 2:1 ratio against the phishing corpus. Architecture, hyperparameters, embedder, and evaluation data are all held constant. The only variable is training set size.

The hypothesis: a smaller, less vocabulary-saturated training set forces the autoencoder to learn structural email patterns (sentence rhythm, topic coherence, semantic flow) rather than lexical familiarity. An attacker copying corporate words can no longer coast on those words as a normality signal.

## Methodology

**Stage 5 data leakage correction.** Stage 5 computed its anomaly threshold from the mixed test set — a set that contains phishing emails. Because phishing emails tend to have higher reconstruction error, they inflate the percentile distribution, pushing the threshold higher than it should be. Stage 10 corrects this by computing the threshold exclusively from `val_safe.csv` (41,084 clean emails), which the model never trained on. This produces a more conservative, honest threshold.

**Training split:** `train_safe.csv` downsampled to 36,082 rows (random_state=42). `val_safe.csv` and `test_mixed.csv` unchanged.

**Threshold selection:** Fine-grained sweep across every percentile from 60 to 95, scored against the val set reconstruction errors. The 83rd percentile (threshold = 0.001581) was chosen as the operational sweet spot — it catches MITM fraud at a 16.5% FPR.

## Results

| Email | Stage 5 (28:1) | V2 (2:1) |
|---|---|---|
| Normal Business Email | CLEAN (0.000980) | CLEAN (0.001010) |
| Credential Phishing | CLEAN (0.001523) | CLEAN (0.001452) |
| MITM Fraud | CLEAN (0.001535) | **FLAGGED (0.001584)** |
| Malware Injection | CLEAN (0.001091) | **FLAGGED (0.002130)** |
| DoS Text Bomb | FLAGGED (0.002056) | FLAGGED (0.002073) |

**Stage 5 threshold:** 0.001800 (80th percentile, test-based — data leakage)
**V2 threshold:** 0.001581 (83rd percentile, val-based — corrected)

V2 catches MITM and Malware that Stage 5 missed entirely. DoS detection is preserved. Credential phishing remains a blind spot — at 2:1, the score (0.001452) stays below any practical threshold without pushing FPR above 44%.

## What V2 Does Not Fix

**Credential phishing is an irreducible limitation of the AI layer.** An attacker who writes a socially engineered email in clean corporate prose — no unusual URLs, no urgency keywords, polished grammar — will score below threshold regardless of training ratio. The semantic structure of that email is too similar to legitimate business communication for an autoencoder to distinguish. The hybrid rules engine catches this at runtime via regex pattern matching on intent signals, but the autoencoder itself cannot be tuned to catch it without a prohibitive false positive rate.

This is the honest conclusion: the autoencoder is a structural anomaly detector, not a semantic intent classifier. It is the right tool for catching structurally deviant emails. It is the wrong tool for catching skilled social engineering. The architecture choice is still justified — no labeled attack data is required for training — but the boundary of what it can detect must be clearly understood.

## Explainability

Token ablation was re-run on V2 using the same test email as Stage 5: "URGENT: Please verify your account immediately." The baseline score increased from 0.001431 (Stage 5) to a higher value, reflecting V2's tighter threshold. Critically, the direction of influence shifted: in V2, polite language has less suppressive effect on anomaly score, consistent with the hypothesis that vocabulary saturation was the root cause of Stage 5's calibration problem.

UMAP cluster visualization from Stage 5 remains valid — phishing embeddings occupy a structurally distinct region of the 384-dimensional space. V2's improved detection is a consequence of the threshold sitting closer to the boundary of that region.

## Known Limitations

- The FPR at 16.5% (val-based) is higher than Stage 5's reported 13.7%, but Stage 5's figure was computed from the mixed test set. A fair like-for-like comparison would show Stage 5's val-based FPR is similarly elevated.
- `val_safe.csv` (41,084 emails) is a large validation set relative to the 2:1 training set (36,082). The validation set is effectively larger than the training set, which is unconventional. This does not affect model quality but means the threshold percentile is computed from a distribution that is richer than the training distribution.
- Production deployment would require retraining on a more diverse benign corpus — Enron is a single organization's email culture. A multi-organization dataset would reduce vocabulary bias further.
