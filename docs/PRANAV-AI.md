1) One-line elevator pitch

BioScan — a phone-based AI that analyzes a 20–30s voice + cough recording to screen for respiratory illness and risk signals (e.g., likely respiratory infection, abnormal breathing patterns, or flagged for clinical follow-up), delivering an instant, explainable risk score and actionable next steps.

2) Why this wins

High impact & scale: low-cost screening tool for low-resource settings.

Technical novelty: deep models on audio spectrograms + explainability + multi-modal features (voice + cough + short questionnaire).

Feasible prototype: you can demonstrate with only a laptop + smartphone mic + public datasets + Azure free credits.

Good fit for Microsoft stack: Azure ML, Cognitive Services, OpenAI for explanations, Power BI for dashboards.

3) Recommended public datasets (start here)

Use multiple public cough/voice datasets to train and validate — combine them to improve generalization:

COUGHVID — large crowdsourced cough dataset (25k–30k+ recordings). Great for cough classification / healthy vs symptomatic signals. 
Nature
EPFL

Coswara — respiratory (breathing, cough, voice) dataset with rich metadata (COVID status, symptoms). Useful for breath + voice tasks. 
Nature
GitHub

Additional datasets & references used in literature: Virufy, Cambridge, ESC-50 (for background sounds augmentation). See comparative studies for dataset combinations. 
ScienceDirect
PMC

(You’ll cite dataset sources in your README and follow their license/consent rules.)

4) System architecture (high level)

Client (Mobile Web / PWA or Streamlit demo)

Records ~30s: coughs (2–3 voluntary coughs), 5–10s breathing, short phrases (counting).

Collects minimal metadata (age bracket, symptoms checkbox, consent).

Sends audio + metadata to backend (or runs on-device for inference if model small).

Preprocessing Service

Audio normalization, denoising (spectral gating), silence trimming.

Extract features: Mel-spectrogram, MFCCs, chroma, spectral contrast; augment with pitch/stats.

Inference (Model Serving)

Primary model: Convolutional neural network (CNN) or CNN+LSTM on spectrograms for cough classification.

Ensembles: Add a separate model for breathing, one for voice (prosody/stress). Combine via small fusion network for final risk score.

Explainability: Grad-CAM on spectrograms + feature importance; short textual explanation using Azure OpenAI.

Decision Module

Produces: risk score (0–100), label (e.g., low/medium/high), top contributing features, recommended actions (test nearby, see doctor, self-isolate).

Dashboard & Analytics

Power BI / Streamlit app for judges showing: model pipeline, sample inferences, aggregated anonymized metrics.

Storage & Orchestration

Azure Blob Storage for audio (encrypted), Azure ML for training & model registry, Azure Functions or App Service for serving APIs.

5) Detailed tech stack (Microsoft-friendly, cheap/free)

Frontend / Demo: Streamlit (quick), React PWA, or Flask.

Audio processing: Python (librosa, torchaudio).

Modeling: PyTorch or TensorFlow. Train on local GPU or Azure ML compute (use student/free credits).

Serving: Azure ML endpoints or Azure Functions + ONNX for light inference.

Explainability & text: Azure OpenAI Service / GPT-4o to convert model signals to human-friendly explanations (use carefully, note policies).

Monitoring / Analytics: Power BI (free tier) or Streamlit dashboards.

Storage: Azure Blob (free student credits), local for prototyping.

CI/CD & Repo: GitHub + Actions (free for public student repos).

Authentication & Consent: Simple OAuth or one-time consent checkbox; store no PII.

6) Model & training plan (practical, reproducible)

A. Preprocessing

Resample to 16 kHz, mono.

Apply bandpass 50–4000 Hz.

Compute 2D Mel-spectrograms (e.g., 128 mel bins), log scale.

Data augmentation: time shift, pitch shift, additive noise, SpecAugment.

B. Model architecture (starter → advanced)

Starter: ResNet18 (pretrained) on mel-spectrogram images (transfer learning).

Advanced: 2-branch network — CNN on cough spectrogram + transformer/CNN on breathing/voice features; fuse latent vectors, then small classifier/regressor for risk score.

C. Loss & outputs

Classification loss (cross-entropy) for disease class / symptomatic vs healthy.

Regression loss (MSE) for severity score if dataset has severity labels.

Calibrate probabilities (Platt scaling).

D. Evaluation metrics

ROC-AUC, Precision/Recall (esp. recall/sensitivity), F1, confusion matrix, and calibration (Brier score).

Clinically important: maximize sensitivity at acceptable specificity (minimize false negatives).

E. Cross-validation

Subject-level split (avoid same person in train & test). Use k-fold stratified by label and demographics.

F. Explainability

Grad-CAM overlays on spectrograms to highlight regions that influenced decision; convert to textual explanation (e.g., “high spectral energy at 800–1200 Hz during cough segment”).

7) Minimal viable demo (what judges want to see)

A short demo video (3–4 min):

Problem statement + personal story (20–30s).

Live demo: record cough → model returns “High risk: recommend testing” + explanation (60–90s).

Quick peek at training & performance metrics (ROC curve, dataset counts) (30s).

Scalability & impact plan (30–60s).

Live web demo URL (Streamlit) where judges can record or upload audio and get results.

8) Minimal 12-week build timeline (you can compress; this is aggressive but doable)

Week 1 — Research & data prep

Collect COUGHVID + Coswara subsets; read licenses.

Prototype recording UI (Streamlit).

Baseline preprocessing scripts.

Week 2–3 — Baseline model

Train baseline ResNet on mel-specs. Evaluate.

Implement augmentation, subject-split CV.

Week 4 — Improve & ensemble

Add breathing/voice branch; fuse models.

Implement explainability (Grad-CAM).

Week 5 — UI & demo

Build Streamlit demo, integrate API endpoint.

Add consent/metadata collection.

Week 6 — Metrics & validation

Generate ROC, confusion matrices, perform ablation studies.

Calibrate model probabilities.

Week 7 — Explainability + OpenAI explanations

Hook model signals to short GPT-4o prompts to produce layman explanations for outputs.

Week 8 — Dashboard & pitch

Make Power BI dashboard showing performance & ethical safeguards.

Draft pitch deck (10–12 slides).

Week 9 — Pilot recordings & user testing

Collect 50–200 test recordings from friends (consent). Evaluate on real audio.

Week 10 — Polish demo video & slides

Produce 3–4 minute demo video.

Week 11 — Rehearse Q&A

Prepare answers for likely judge questions (bias, false positives, clinical deployment).

Week 12 — Final polish & submission

9) Team roles (small & lean)

You (Lead dev/ML) — pipeline, modeling, training.

Frontend dev / Demo creator — Streamlit + video.

Domain advisor (medicine/public health) — validate clinical claims (can be a mentor).

(Optional) Data engineer — dataset curation & augmentation.

You can do this as a team of 2–3 to be efficient.

10) Cost & Azure student perks (keep it near-free)

Compute: Train locally on a decent laptop / free Colab GPU (or Azure ML free credits).

Azure: Student credits and free tiers can cover hosting + OpenAI trial credits. (Sign up early).

Storage: Tiny — audio files are small. Use Azure Blob free with credits.
Estimated out-of-pocket: ₹0 – ₹5,000 (domain/hosting optional). Mostly free.

11) Data privacy & ethics (must cover in judges Qs)

Obtain explicit consent for any recordings in demos.

Anonymize metadata; store only what’s required.

Emphasize tool is a screening aid, not a diagnosis. Recommend confirmatory clinical tests.

Prepare a short ethics slide describing bias mitigation (balanced datasets, subject-split CV), privacy (encryption), and regulatory pathway (clinical trials, approvals).

12) Submission materials you must prepare

Working demo link (Streamlit or PWA) — judges will try it.

3–4 minute demo video (clear story + live demo).

Pitch deck (10 slides) — problem, solution, tech, impact, business model, timeline, team, ask.

Code repo (GitHub) — README, instructions to run.

Datasheet: dataset sources, preprocessing steps, model card & limitations (ethical).

13) Demo video script (quick 3-4 min)

0:00–0:20 — Hook: quick story (“My uncle waited too long...”)

0:20–0:50 — Problem stats & solution overview (BioScan)

0:50–1:50 — Live demo: record cough + breath, show risk + explanation; show Grad-CAM spectrogram.

1:50–2:30 — Model performance highlights (ROC AUC, sensitivity) with dataset counts & sources. (Cite COUGHVID / Coswara.) 
Nature
+1

2:30–3:10 — Business & deployment: free app for screening; partnerships with clinics; scaling plan.

3:10–3:40 — Team & ask: mentorship, Azure credits, pilot partners.

3:40–4:00 — Call to action & gratitude.

14) Likely Judge Questions & short answers (prep)

Q: How accurate is it? A: Share ROC-AUC and emphasize high sensitivity; state dataset & CV method.

Q: Could it cause false reassurance? A: We calibrate thresholds to prioritize sensitivity and provide clear disclaimers + next steps.

Q: Have you tested on different microphones/environments? A: Use noise augmentation and domain randomization; show results from pilot.

Q: Regulatory path? A: It’s a screening tool; plan clinical validation and FDA/CE if moving to medical device.

15) Quick starter checklist (today)

Clone/open a project repo.

Download COUGHVID and Coswara subsets. 
Kaggle
GitHub

Create Streamlit UI to record/upload audio.

Write preprocessing pipeline (librosa).

Train a quick ResNet baseline on mel-specs.

Make one demo inference and record video.

16) Useful references (datasets & papers)

COUGHVID dataset (EPFL / Nature paper). 
Nature
EPFL

Coswara dataset (IISc paper / GitHub). 
Nature
GitHub

Survey papers on cough audio detection & ML methods. 
PMC
ScienceDirect