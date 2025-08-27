# BioScan – AI Disease Prediction from Cough & Voice 🩺

## 1) One-line Elevator Pitch

BioScan — a phone-based AI that analyzes a 20–30s voice + cough recording to screen for respiratory illness and risk signals (e.g., likely respiratory infection, abnormal breathing patterns, or flagged for clinical follow-up), delivering an instant, explainable risk score and actionable next steps.

## 2) Why this Wins

* High impact & scale: low-cost screening tool for low-resource settings.
* Technical novelty: deep models on audio spectrograms + explainability + multi-modal features (voice + cough + short questionnaire).
* Feasible prototype: can demonstrate with only a laptop + smartphone mic + public datasets + Azure free credits.
* Good fit for Microsoft stack: Azure ML, Cognitive Services, OpenAI for explanations, Power BI for dashboards.

## 3) Recommended Public Datasets

* **COUGHVID** — large crowdsourced cough dataset (25k–30k+ recordings). Great for cough classification / healthy vs symptomatic signals.

  * Sources: Nature, EPFL
* **Coswara** — respiratory (breathing, cough, voice) dataset with rich metadata (COVID status, symptoms). Useful for breath + voice tasks.

  * Sources: Nature, GitHub
* **Additional datasets**: Virufy, Cambridge, ESC-50 (for background sounds augmentation).

  * Sources: ScienceDirect, PMC
* *(Cite datasets in README and follow license/consent rules.)*

## 4) System Architecture (High Level)

1. **Client (Mobile Web / PWA or Streamlit demo)**

   * Records \~30s: coughs (2–3 voluntary), 5–10s breathing, short phrases.
   * Collects minimal metadata (age, symptoms, consent).
   * Sends audio + metadata to backend or runs on-device.

2. **Preprocessing Service**

   * Audio normalization, denoising, silence trimming.
   * Feature extraction: Mel-spectrogram, MFCCs, chroma, spectral contrast, pitch/stats.

3. **Inference (Model Serving)**

   * Primary model: CNN or CNN+LSTM on spectrograms for cough classification.
   * Ensembles: Separate model for breathing, one for voice; fuse for final risk score.
   * Explainability: Grad-CAM + Azure OpenAI textual explanation.

4. **Decision Module**

   * Produces: risk score (0–100), label (low/medium/high), contributing features, recommended actions.

5. **Dashboard & Analytics**

   * Power BI / Streamlit: pipeline visualization, sample inferences, anonymized metrics.

6. **Storage & Orchestration**

   * Azure Blob Storage (encrypted), Azure ML (training + model registry), Azure Functions or App Service for APIs.

## 5) Detailed Tech Stack (Microsoft-friendly, cheap/free)

* **Frontend/Demo:** Streamlit, React PWA, or Flask.
* **Audio Processing:** Python (librosa, torchaudio).
* **Modeling:** PyTorch or TensorFlow.
* **Serving:** Azure ML endpoints or Azure Functions + ONNX.
* **Explainability & Text:** Azure OpenAI Service / GPT-4o.
* **Monitoring / Analytics:** Power BI free tier or Streamlit dashboards.
* **Storage:** Azure Blob (student credits) or local.
* **CI/CD & Repo:** GitHub + Actions.
* **Authentication & Consent:** OAuth or one-time checkbox.

## 6) Model & Training Plan

**A. Preprocessing:** Resample 16kHz, mono; bandpass 50–4000 Hz; Mel-spectrogram; augment (time shift, pitch, noise, SpecAugment).

**B. Model Architecture:**

* Starter: ResNet18 on mel-spec images.
* Advanced: 2-branch CNN + transformer/CNN for cough + breathing/voice; fuse vectors; classifier for risk score.

**C. Loss & Outputs:** Classification loss (cross-entropy), regression loss (MSE for severity).

**D. Evaluation Metrics:** ROC-AUC, Precision/Recall, F1, confusion matrix, calibration (Brier score).

**E. Cross-Validation:** Subject-level split, k-fold stratified.

**F. Explainability:** Grad-CAM + textual explanation.

## 7) Minimal Viable Demo

* 3–4 min video: problem + story, live demo (cough → risk + explanation), training metrics, scalability plan.
* Live web demo (Streamlit) for judges.

## 8) Minimal 12-week Build Timeline

* **Week 1:** Research & data prep, prototype recording UI, baseline preprocessing.
* **Weeks 2–3:** Baseline model training (ResNet), augmentation, CV.
* **Week 4:** Breathing/voice branch, ensemble, Grad-CAM.
* **Week 5:** Streamlit demo, API integration.
* **Week 6:** Metrics & validation.
* **Week 7:** OpenAI explanations.
* **Week 8:** Dashboard & pitch.
* **Week 9:** Pilot recordings, user testing.
* **Week 10:** Polish demo video & slides.
* **Week 11:** Rehearse Q\&A.
* **Week 12:** Final polish & submission.

## 9) Team Roles

* Lead Dev/ML: pipeline, modeling, training.
* Frontend/Demo creator: Streamlit + video.
* Domain Advisor: validate clinical claims.
* Optional: Data engineer.

## 10) Cost & Azure Student Perks

* Compute: local laptop / free Colab GPU / Azure ML free credits.
* Storage: Azure Blob free tier.
* Out-of-pocket: ₹0 – ₹5,000 (mostly free).

## 11) Data Privacy & Ethics

* Obtain consent, anonymize metadata.
* Screening aid, not a diagnosis.
* Ethics slide for bias mitigation, privacy, regulatory pathway.

## 12) Submission Materials

* Working demo link, 3–4 min video, pitch deck (10 slides), GitHub repo, dataset & model datasheet.

## 13) Demo Video Script (3–4 min)

* 0:00–0:20: Hook story
* 0:20–0:50: Problem stats + solution overview
* 0:50–1:50: Live demo (cough + breath → risk + Grad-CAM)
* 1:50–2:30: Model performance (ROC, dataset counts)
* 2:30–3:10: Business & deployment plan
* 3:10–3:40: Team & ask
* 3:40–4:00: Call to action & gratitude

## 14) Likely Judge Questions

* Accuracy: share ROC-AUC, emphasize sensitivity.
* False reassurance: calibrate thresholds + disclaimers.
* Microphone/environment variation: noise augmentation, domain randomization.
* Regulatory path: screening tool, plan clinical validation / FDA/CE.

## 15) Quick Starter Checklist (Today)

* Clone/open repo.
* Download COUGHVID + Coswara subsets.
* Streamlit UI for record/upload audio.
* Preprocessing pipeline (librosa).
* Train quick ResNet baseline.
* Make one demo inference + record video.

## 16) Useful References

* COUGHVID (EPFL / Nature)
* Coswara (IISc / GitHub)
* Survey papers on cough audio detection & ML methods (PMC / ScienceDirect)
