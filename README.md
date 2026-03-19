# SAM Medical Robustness Audit

**Cross-Organ Evaluation of Segment Anything Model (SAM) in Medical Imaging**

---

## Overview

This repository presents a comprehensive evaluation of the **Segment Anything Model (SAM)** across multiple medical imaging domains.
The study investigates **segmentation performance, robustness under perturbations, and failure behavior** across different organs and imaging modalities.

### Organs & Modalities

* **Brain (MRI)** – BraTS dataset (tumor segmentation)
* **Liver (CT)** – abdominal organ segmentation
* **Spleen (CT)** – robustness-focused evaluation

---

## Objectives

* Evaluate baseline segmentation performance of SAM
* Analyze robustness under real-world perturbations
* Quantify failure rates across organs
* Compare performance across imaging modalities (MRI vs CT)

---

## Methodology

### 1. Data Processing

* MRI (Brain) and CT (Liver, Spleen) datasets used
* CT images normalized using windowing (level=50, width=400)
* 3D volumes converted into 2D axial slices
* Only slices with non-empty masks retained

---

### 2. SAM Inference

* Model: **SAM ViT-B**
* Prompting: **Bounding box derived from ground truth masks**
* Inference performed per slice

---

### 3. Evaluation Metrics

* **Dice Score**
* **Intersection over Union (IoU)**
* **Failure Rate (Dice < 0.5)**
* **Severe Failure Rate (Dice < 0.1)**

---

### 4. Robustness Testing (Spleen Study)

The model was evaluated under multiple perturbations:

* Gaussian Noise (σ = 10, 25)
* Gaussian Blur (kernel = 3, 7)
* Downsampling (0.5×, 0.25×)
* Contrast scaling (0.8×, 1.2×)
* Gamma correction (0.8, 1.2)

Performance degradation measured using:

* ΔDice (relative to clean baseline)
* Failure rate under perturbation

---

## Results Summary

### Key Findings

* SAM performs strongly on **clean medical images**
* Significant performance drop under **noise and resolution degradation**
* CT-based organs show **higher sensitivity to perturbations**
* Failure rates increase under domain shift conditions

---

## Repository Structure

```
sam-medical-robustness/
│
├── notebooks/
│   ├── brain.ipynb
│   ├── liver.ipynb
│   └── spleen.ipynb
│
├── results/
│   ├── brain_results.csv
│   ├── liver_results.csv
│   ├── spleen_sam_boxprompt_metrics.csv
│   ├── spleen_sam_robustness_results.csv
│   ├── robustness_summary_table.csv
│   └── combined_summary.csv
│
├── figures/
│   ├── figure1_dice.png
│   ├── figure2_failure.png
│   ├── figure3_robustness.png
│   └── worst_cases_overlay.png
│
├── README.md
└── requirements.txt
```

---

## Example Outputs

* Distribution of Dice and IoU scores
* Failure rate analysis across organs
* Robustness degradation curves
* Visualization of worst-performing segmentation cases

---

## Key Contributions

* First comparative evaluation of SAM across **multiple organs and modalities**
* Detailed robustness analysis under **realistic image perturbations**
* Failure-aware evaluation highlighting model limitations
* Reproducible pipeline for medical segmentation benchmarking

---

## Requirements

```
torch
opencv-python
numpy
pandas
matplotlib
nibabel
tqdm
segment-anything
```

---

## How to Run

1. Open notebooks in Google Colab
2. Install dependencies
3. Run each organ pipeline:

   * brain.ipynb
   * liver.ipynb
   * spleen.ipynb
4. Generate results and summary tables

---

## Limitations

* SAM requires prompt guidance (bounding box)
* Performance degrades under domain shift
* Evaluation limited to 2D slice-based inference

---

## Future Work

* Prompt-free segmentation evaluation
* Fine-tuning SAM for medical imaging
* Multi-modal fusion (MRI + CT)
* Clinical validation across larger datasets

---

## Author

**SANGHATI BASU**
MS Healthcare Informatics | Biomedical AI Research

---

## 📄 License

This project is for research and educational purposes only.
