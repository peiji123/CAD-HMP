# CAD-HMP

**Hypergraph-based Multitask Learning for Per-Treatment Prognosis Estimation in Coronary Artery Disease from EHRs**

This repository accompanies the manuscript introducing **CAD-HMP**, a dual-hypergraph multitask learning framework for treatment-specific prognosis estimation in coronary artery disease.

## Overview

CAD-HMP integrates structured electronic health records (EHRs), clinical text, guideline knowledge and patient-derived evidence to compare three treatment strategies: medical management without revascularization (**MEDICAL**), coronary artery bypass grafting (**CABG**) and percutaneous coronary intervention (**PCI**). For each strategy, it estimates 30-day mortality, severe acute kidney injury (AKI), major bleeding and remaining hospital length of stay (LOS).

![CAD-HMP framework](figures/figure1_framework.png)

*Figure 1. Workflow from patient-state construction and dual-hypergraph evidence integration to treatment-specific predictions and supporting evidence. Reproduced from manuscript Fig. 1.*

## Method

1. **Patient representation.** Structured clinical variables and temporally filtered clinical text describe the patient at the 24-hour admission landmark. Clinical text is extracted using Llama 3.3 70B and encoded using BioClinical-ModernBERT.
2. **Guideline-based quality control.** A quality-control agent queries the guideline hypergraph for source-linked statements and assesses support, contradiction or evidence gaps for each candidate strategy.
3. **Patient evidence support.** An evidence-support agent retrieves similar, treatment-matched patients from a hypergraph constructed using the training cohort. Similarity-weighted outcome estimates are smoothed towards treatment-group averages and combined with neural predictions. Outcome labels are excluded from graph features.
4. **Multitask prediction.** A shared Transformer integrates patient inputs to estimate treatment probabilities and strategy-specific outcomes. Outcome supervision uses available labels only from the treatment actually received. The trained model and evidence resources remain frozen during testing.

Treatment probabilities reflect observed care patterns. Predictions for unreceived treatments are model-based projections; they do not establish causal treatment benefits.

## Evaluation

The retrospective study includes 9,639 training patients and 2,378 internal-test patients from MIMIC-IV, plus 2,058 external-test patients from MIMIC-III CareVue. Treatment-classification macro-AUCs are **0.939** internally and **0.860** externally. Pooled factual-outcome AUCs are **0.894** for mortality, **0.878** for severe AKI and **0.839** for major bleeding.

![Treatment classification and factual outcome prediction performance](figures/figure2_performance.png)

*Figure 2. Treatment classification in the internal and external test cohorts (a,b), and factual outcome prediction in the pooled test cohorts (c–f). Reproduced from manuscript Fig. 2; outcome evaluation uses the observed-treatment branch.*

## Data access

[MIMIC-IV](https://physionet.org/content/mimiciv/) and [MIMIC-III](https://physionet.org/content/mimiciii/) are available through PhysioNet subject to credentialing, required training and data-use agreements. Patient-level datasets are not distributed in this repository.
