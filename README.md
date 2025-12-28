# Multimodal Pneumonia Detection from EHR & Chest X-Rays

**Evaluating whether multimodal learning truly improves pneumonia detection in clinical data**

---

## 📌 Project Overview

Pneumonia is a major cause of morbidity and mortality in hospitalized patients, where early and accurate diagnosis is critical. Clinicians typically rely on both **structured electronic health record (EHR) data** (laboratory values, demographics) and **unstructured chest X-ray (CXR) images** to make diagnostic decisions.

This project systematically evaluates whether **multimodal machine learning**, which integrates structured clinical data with medical imaging, outperforms unimodal approaches for pneumonia detection.

Using the **Symile-MIMIC** dataset, we compare:
- **Structured-only models** (EHR data)
- **Image-only deep learning models** (CXR)
- **Explicit multimodal fusion architectures**

A strong emphasis is placed on **recall**, reflecting the clinical priority of minimizing missed pneumonia cases.

---

## 🎯 Objectives

- Develop machine learning models using **structured EHR data**
- Train deep learning models on **chest X-ray images**
- Design and evaluate **multimodal fusion architectures**
- Determine whether **multimodal learning provides measurable gains** over unimodal models

---

## 🧠 Dataset

### Symile-MIMIC (PhysioNet)

A large-scale, de-identified multimodal clinical dataset combining:
- **Structured EHR data** from MIMIC-IV
- **Chest X-ray images** from MIMIC-CXR
- **CheXpert-derived pneumonia labels**

**Key characteristics**
- 11,622 hospital admissions (filtered to definitive labels)
- Near-balanced pneumonia vs non-pneumonia cohort
- High real-world missingness in laboratory values
- Significant co-occurrence of pneumonia with other thoracic findings

> Access to the dataset requires PhysioNet credentialing.

---

## 🛠️ Methodology

### 1️⃣ Structured Modelling (EHR Only)
- Gradient Boosted Trees (**XGBoost**)
- Embedded **Multilayer Perceptron (MLP)**
- Percentile-based lab normalization
- Explicit missingness indicators
- Demographics and admission-level features

---

### 2️⃣ Unstructured Modelling (Chest X-Ray Only)
- **Custom CNN** with view-position embedding
- **ResNet-18** trained from scratch
- Image normalization and augmentation
- Recall-oriented evaluation strategy

---

### 3️⃣ Multimodal Fusion (Two-Stage Architecture)
- **Stage 1:** XGBoost model trained on structured EHR data produces a calibrated pneumonia risk score
- **Stage 2:** CNN fuses chest X-ray features with the structured risk score

This design allows structured data to act as a **clinical prior**, while the CNN refines predictions using visual evidence.

---

### 4️⃣ Ablation Study: Single-Stage Fusion
- A single CNN trained on **images + raw tabular features**
- Used to test whether explicit multimodal design is necessary

---

## 📊 Results Summary

| Model | ROC-AUC | Recall | F1-Score |
|-----|--------|--------|---------|
| XGBoost (Structured) | 0.67 | 0.56 | 0.58 |
| MLP (Structured) | 0.61 | 0.58 | 0.57 |
| Custom CNN (CXR) | 0.57 | **0.83** | 0.62 |
| ResNet-18 (CXR) | 0.64 | 0.54 | 0.56 |
| **CNN + XGBoost (Multimodal)** | 0.63 | **0.74** | **0.62** |
| CNN + Full Tabular (Naïve) | 0.64 | 0.33 | 0.43 |

---

## 🔍 Key Findings

- Structured EHR data alone provides **moderate predictive signal**
- Image-only CNNs can achieve **very high recall**, but often at the cost of many false positives
- Naïvely combining tabular data with CNNs **reduces sensitivity**
- A **two-stage multimodal architecture** significantly improves recall while maintaining balance
- Multimodal learning is effective **only when designed carefully**

---

## 🧪 Why Multimodality Matters

This project mirrors real clinical reasoning:
- **EHR data** establishes baseline clinical risk
- **Imaging** confirms or refines diagnosis

Explicit multimodal fusion better captures complementary information than single-stage or unimodal models.

---

## 🚀 Future Work

- Incorporate clinical notes and temporal lab trends
- Improve label quality beyond automated CheXpert rules
- External validation on non-MIMIC datasets
- Calibration and deployment-oriented threshold optimization

---

## ⚠️ Disclaimer

This project is for **research and educational purposes only** and is **not intended for clinical deployment**.

---

## 📚 References

- Symile-MIMIC Dataset (PhysioNet)
- CheXpert Labeling System
- Recent surveys on multimodal AI in healthcare
