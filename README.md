# Classifying-Medical-AI-Risk-Under-the-EU-AI-Act
# Classifying Medical AI Risk Under the EU AI ACT: Small-Scale ML Exploration

## Overview

This project explores how machine learning can help structure and visualize the risk-classification logic that the **EU AI Act** applies to AI-based medical devices (**SaMD – Software as a Medical Device**). Ten AI-enabled medical/health systems (six real, regulator-documented devices and four illustrative examples) were profiled against a set of regulatory-relevant features, then used to train interpretable ML models (Random Forest, Decision Tree) that surface which factors actually drive the High-Risk / Limited-Risk / Minimal-Risk classification.

The goal is not to build a production risk-classifier, but to demonstrate — with a small, transparent, hand-curated dataset — how regulatory reasoning (EU AI Act, MDR) can be translated into structured features and interrogated with basic ML tooling.

## Background

The EU AI Act (Regulation 2024/1689) introduces a horizontal risk-tiering system for all AI systems placed on the EU market. For medical devices specifically, Article 6(1) and Annex I create a direct link to the existing **Medical Device Regulation (MDR)**: any AI system embedded in a medical device that requires Notified Body conformity assessment under MDR is automatically classified as "High-Risk" under the AI Act. This project investigates what that link looks like in practice, using real regulatory data as a starting point.

## Data & Methodology

**Data sources:** Six real, publicly documented AI-enabled medical devices (IDx-DR, Da Vinci Surgical System, Medtronic MiniMed 670G, Viz.ai, AliveCor KardiaMobile, PathAI), profiled using their FDA regulatory documentation (De Novo/510(k) summaries) as the primary factual source. Four additional illustrative (non-product-specific) examples were added to represent lower-risk/administrative use cases and broaden the classification spectrum.

**Features engineered** (based on EU AI Act Article 6/Annex I and MDR Rule 11 criteria):
- `intended_purpose` – diagnosis / treatment / prediction / prevention / administrative
- `physical_harm_severity` – direct physical harm potential (no_harm → high)
- `clinical_decision_severity` – severity of harm if the *clinical decision* the system supports is wrong (independent of physical mechanism)
- `autonomy_level` – fully autonomous / decision support / human-in-the-loop
- `explainability` – transparent vs. black-box
- `is_adaptive` – whether the model continues learning post-deployment or is locked
- `reliance` – sole diagnostic source vs. supporting tool

**Target variable:** `eu_ai_risk_category` (High-Risk / Limited-Risk / Minimal-Risk) — derived independently from each device's FDA classification, by applying EU AI Act/MDR logic rather than copying the US regulatory outcome. FDA class is retained in the dataset for reference only and was **excluded from model training** to avoid data leakage.

**Techniques applied:** exploratory data analysis, ordinal/nominal/binary encoding, Random Forest feature importance, and a single interpretable Decision Tree for visualization.

## Key Findings

1. **All six real, MDR-regulated devices were classified High-Risk without exception** — confirming that the Notified-Body threshold under MDR/AI Act acts as a decisive, binary gate rather than a graded scale.
2. **`clinical_decision_severity` was the single strongest predictor** of risk category (feature importance ≈ 27%), ahead of the device's raw physical harm potential.
3. **AI-specific technical traits (adaptivity, explainability, autonomy) had comparatively low influence on the risk *category* itself** in this dataset — because all regulated devices already sat inside the same High-Risk bucket. This suggests these traits matter more for determining the *scope of compliance obligations* within High-Risk status (transparency, human oversight, data governance) than for the initial classification decision.

## Limitations

- **Sample size:** n = 10. No train/test split was used; models were trained on the full dataset and interpreted for pattern description, not evaluated for predictive accuracy.
- Some decision-tree leaf nodes are based on a single sample and reflect this specific dataset rather than a generalizable rule.
- The dataset mixes real, regulator-documented devices with illustrative (non-product) examples for lower-risk categories.
- `eu_ai_risk_category` is an independent analytical estimate based on publicly available information, **not an official regulatory determination**.
- This project is for educational/portfolio purposes and should not be used as a substitute for formal regulatory classification.

## Tech Stack

Python · pandas · scikit-learn · matplotlib · Kaggle Notebooks

## Repository Contents
-📓 View the full interactive notebook on Kaggle — full analysis (EDA, encoding, modeling, visualization)
https://www.kaggle.com/code/wroudmrae/classifying-medical-ai-risk-under-the-eu-ai-act
- `notebook.ipynb` — full analysis (EDA, encoding, modeling, visualization)
- `medical_ai_devices.csv` — the curated dataset
- `decision_tree.png` — visualization of the final decision tree
- `README.md` — this file

## Author

Wroud Mrae — Pharmacist (Approbation, Germany) transitioning into Regulatory Affairs, Pharmacovigilance, and AI in Healthcare.
