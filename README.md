# A Robustness Audit of the Segment Anything Model for Multi-Organ Medical Image Segmentation Under Domain Shift

**Sanghati Basu**
MS Healthcare Informatics, University of Illinois Springfield
`sbasu23@uis.edu` | ORCID: 0009-0001-2437-773X

---

## Overview

This repository contains all code, data, and results for a robustness audit of the **Segment Anything Model (SAM)** across three anatomical targets:

| Organ | Modality | Dataset |
|---|---|---|
| Brain tumour | MRI | BraTS 2021 |
| Liver | CT | Medical Segmentation Decathlon Task 03 |
| Spleen | CT | Medical Segmentation Decathlon Task 09 |

The study evaluates SAM ViT-B under oracle bounding-box prompts on 300 slices per organ, followed by a nine-condition perturbation protocol applied to the spleen CT evaluation set.

---

## Key Findings

- SAM performance is strongly organ- and modality-dependent, driven primarily by boundary morphology and imaging contrast rather than perturbation magnitude.
- Zero-shot SAM performs best on high-contrast CT organs: spleen achieves Dice 0.914, liver 0.853.
- In the spleen robustness study, performance is **largely preserved** under all nine tested perturbations, with several conditions showing small positive ΔDice and no condition causing substantial degradation.
- Brain tumour MRI remains the most challenging setting, with a 39.3% moderate-failure rate (Dice < 0.5) — substantially higher than liver (2.3%) and spleen (0.7%).
- Comparison against published nnU-Net benchmarks shows absolute Dice gaps of 0.049 (spleen), 0.090 (liver), and 0.359 (brain), providing a quantitative reference for deployment-oriented evaluation.
- These findings suggest that imaging modality, organ geometry, and boundary complexity are stronger determinants of SAM robustness than moderate image perturbation magnitude.

---

## Objectives

- Evaluate baseline SAM segmentation performance across organs and modalities
- Analyse robustness under nine controlled perturbation conditions
- Quantify failure rates using dual thresholds (Dice < 0.5 and Dice < 0.1)
- Compare performance against published nnU-Net supervised baselines
- Provide a deployment perspective for SAM in Health Digital Twin pipelines

---

## Methodology

### 1. Data Processing

- BraTS 2021 multi-parametric MRI — T1ce channel selected for maximum tumour contrast
- Medical Segmentation Decathlon Tasks 03 (liver) and 09 (spleen)
- CT images normalised using Hounsfield windowing (centre = 50 HU, width = 400 HU)
- 3D volumes converted to 2D axial slices; only slices with non-empty masks retained
- 300 slices per organ sampled from held-out test volumes

### 2. SAM Inference

- Model: SAM ViT-B (`vit_b` checkpoint)
- Prompting: oracle bounding boxes derived from ground-truth masks
- Results should be interpreted as best-case prompted SAM performance under ideal box localisation
- Single-mask output mode; MRI slices converted to RGB by channel replication

### 3. Evaluation Metrics

- Dice Similarity Coefficient (DSC)
- Intersection over Union (IoU)
- Failure rate at Dice < 0.5 (moderate) and Dice < 0.1 (severe)

### 4. Robustness Testing (Spleen CT)

Nine perturbation conditions applied:

| Family | Conditions |
|---|---|
| Gaussian noise | σ = 10, 25 HU |
| Gaussian blur | kernel k = 3, 7 |
| Downsampling | 0.5×, 0.25× (bicubic) |
| Contrast scaling | 0.8×, 1.2× |
| Gamma correction | γ = 0.8, 1.2 |

ΔDice and ΔIoU computed relative to clean baseline for each condition.

---

## Results Summary

### Baseline Performance vs. nnU-Net

| Organ | SAM Dice | nnU-Net Dice | Abs. Dice Gap | Fail < 0.5 |
|---|---|---|---|---|
| Brain (MRI) | 0.515 ± 0.220 | 0.874 | 0.359 | 39.3% |
| Liver (CT) | 0.853 ± 0.137 | 0.943 | 0.090 | 2.3% |
| Spleen (CT) | 0.914 ± 0.085 | 0.963 | 0.049 | 0.7% |

*nnU-Net values from Isensee et al., Nature Methods 2021, used as published reference benchmarks.*

### Spleen Robustness

Across all nine perturbation conditions, performance was largely preserved relative to the clean baseline (ΔDice range: −0.001 to +0.008). No condition caused substantial degradation. This pattern is consistent with a possible patch-level smoothing effect in ViT-B under moderate intensity shift.

---

## Repository Structure

```
sam-brats-robustness-audit/
│
├── Notebook/
│   ├── 01_brain_brats_.ipynb
│   ├── SAM_liver.ipynb
│   └── sam_spleen_ct_robustness_-5.ipynb
│
├── results/
│   ├── brain_results.csv
│   ├── brain_robustness_summary.csv
│   ├── brain_prompt_variants.csv
│   ├── liver_results.csv
│   ├── liver_robustness_summary.csv
│   ├── spleen_sam_boxprompt_metrics.csv
│   ├── spleen_sam_robustness_results.csv
│   ├── robustness_summary_table_FINAL.csv
│   └── combined_summary.csv
│
├── src/
│   └── inference.py
│
├── brain_example_prediction.png
├── liver_example_prediction.png
├── brain_robustness_plot.png
├── liver_robustness_plot.png
├── robustness_delta_dice_barplot.png
├── worst_cases_overlay.png
└── README.md
```

---

## How to Run

1. Open notebooks in Google Colab or Jupyter
2. Install dependencies: `pip install torch opencv-python numpy pandas matplotlib nibabel tqdm segment-anything`
3. Run each organ pipeline:
   - `01_brain_brats_.ipynb` — brain tumour evaluation
   - `SAM_liver.ipynb` — liver evaluation
   - `sam_spleen_ct_robustness_-5.ipynb` — spleen baseline and robustness

---

## Limitations

- Oracle bounding-box prompts represent best-case SAM performance; operational deployment would require an automated proposal stage
- Evaluation restricted to 2D axial slices; volumetric consistency uncharacterised
- Perturbations applied to pre-processed slices rather than raw DICOM volumes
- nnU-Net comparison uses published benchmark values rather than direct reimplementation under matched conditions

---

## Citation

If you use this code or results, please cite:

```
Basu, S. (2026). A Robustness Audit of the Segment Anything Model for
Multi-Organ Medical Image Segmentation Under Domain Shift.
University of Illinois Springfield.
GitHub: https://github.com/SANGHATI23/sam-brats-robustness-audit
```

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

## License

This project is for research and educational purposes only.

## Author

**Sanghati Basu**
MS Healthcare Informatics | University of Illinois Springfield
ORCID: 0009-0001-2437-773X
