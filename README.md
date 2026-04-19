# A Robustness Audit of the Segment Anything Model for Multi-Organ Medical Image Segmentation Under Domain Shift

**Sanghati Basu**
MS Healthcare Informatics, University of Illinois Springfield
`sbasu23@uis.edu` | ORCID: 0009-0001-2437-773X

---

# SAM Medical Robustness Audit

**Benchmarking Segment Anything Model (SAM) across CT, MRI, and histopathology under domain shift and perturbation conditions**

## Overview

This repository presents a multi-domain robustness audit of the **Segment Anything Model (SAM)** for medical image segmentation. Rather than reporting only average segmentation accuracy, this project examines how SAM behaves across:

- different imaging modalities
- different target types (organs, tumors, nuclei)
- different perturbation settings
- difficult edge cases and outright failures

The study is designed to test whether strong average performance translates into dependable behavior across medical imaging domains.

## Included experiments

This repository currently includes experiments for:

- **Brain tumor MRI**
- **Liver CT**
- **Spleen CT**
- **Kidney tumor CT (KiTS23)**
- **Histopathology nuclei segmentation (MoNuSeg)**

Together, these tasks allow comparison across:

- **MRI vs CT vs histopathology**
- **organ segmentation vs lesion segmentation vs dense microscopic foreground segmentation**
- **clear anatomical boundaries vs irregular tumors vs highly textured small-object domains**

## Why this project matters

General-purpose vision foundation models can appear strong on average, but medical use requires more than high mean Dice. A model may still be risky if it fails under certain perturbations, on certain structures, or in specific domains.

This repository focuses on the more important question:

**When does SAM work well, and when does it become unreliable?**

## Study goals

This project is built to answer the following questions:

1. How well does SAM transfer to medical image segmentation with box prompts?
2. Does SAM behave differently on **organs** versus **tumors**?
3. Does performance change across **CT**, **MRI**, and **histopathology**?
4. Is SAM more sensitive to **noise**, **blur**, **downsampling**, or **intensity shifts**?
5. Do average metrics hide a clinically meaningful failure tail?

---

# Datasets and tasks

## 1. Brain MRI
- **Task:** Brain tumor segmentation
- **Purpose:** Evaluate SAM in an MRI lesion setting with heterogeneous tumor appearance
- **Representative outputs:**
  - `brain_results.csv`
  - `brain_prompt_variants.csv`
  - `brain_robustness_summary.csv`
  - `brain_robustness_plot.png`
  - `brain_example_prediction.png`

## 2. Liver CT
- **Task:** Liver segmentation
- **Purpose:** Evaluate SAM on a comparatively clearer organ boundary in abdominal CT
- **Representative outputs:**
  - `liver_results.csv`
  - `liver_robustness_summary.csv`
  - `liver_robustness_plot.png`
  - `liver_example_prediction.png`

## 3. Spleen CT
- **Task:** Spleen segmentation
- **Purpose:** Evaluate SAM on another abdominal organ and test controlled perturbation robustness
- **Representative outputs:**
  - `spleen_sam_boxprompt_metrics.csv`
  - `spleen_sam_robustness_results.csv`
  - `robustness_summary_table_FINAL.csv`
  - `worst_cases_overlay.png`

## 4. Kidney tumor CT (KiTS23)
- **Task:** Kidney tumor segmentation
- **Purpose:** Test SAM on a harder CT lesion task with irregular tumor boundaries
- **Why important:** This extends the CT analysis beyond easier whole-organ masks and adds a more clinically challenging lesion setting
- **Representative outputs:**
  - `kits23_tumor_sam_boxprompt_metrics.csv`
  - `kits23_tumor_sam_robustness_results.csv`
  - `kits23_tumor_robustness_summary_table.csv`
  - `kits23_tumor_worst_cases_overlay.png`
  - `kits23_tumor_robustness_barplot.png`
  - `kits23_tumor_summary.txt`

## 5. Histopathology nuclei segmentation (MoNuSeg)
- **Task:** Nuclei foreground segmentation from histopathology images
- **Purpose:** Evaluate SAM under a much stronger domain shift, moving beyond radiology into microscopy/pathology
- **Why important:** This introduces tiny dense objects, strong texture variation, and a very different image formation process than CT or MRI
- **Representative outputs:**
  - `monuseg_sam_boxprompt_metrics.csv`
  - `monuseg_sam_robustness_results.csv`
  - `monuseg_robustness_summary_table.csv`
  - `monuseg_worst_cases_overlay.png`
  - `monuseg_robustness_barplot.png`
  - `monuseg_summary.txt`

## 6. Cross-study summary
- Combined outputs may include:
  - `combined_summary.csv`
  - `robustness_delta_dice_barplot.png`

---

# Methodology

## Preprocessing

The pipelines follow a broadly consistent structure across tasks:

- volumetric CT/MRI data are converted into **2D axial slices**
- only **non-empty target slices** are retained
- CT inputs are windowed and normalized
- masks are binarized for the target structure of interest
- images are formatted as RGB inputs for SAM

For histopathology:
- original color microscopy images are used directly
- nuclei instance masks are merged into a **binary union mask** for semantic foreground evaluation

## Prompting strategy

All experiments use **ground-truth-derived bounding box prompts**.

This design isolates segmentation quality after localization is given, allowing more direct comparison of how SAM behaves on the segmentation task itself.

## Model

- **Segment Anything Model (SAM)**
- **ViT-B checkpoint**

## Metrics

Performance is evaluated using:

- **Dice score**
- **Intersection over Union (IoU)**
- **Failure rate:** Dice < 0.5
- **Severe failure rate:** Dice < 0.1

## Robustness analysis

Each task includes controlled perturbation experiments such as:

- Gaussian noise
- Gaussian blur
- downsampling / resolution degradation
- contrast scaling
- gamma correction

Robustness is summarized using:

- mean Dice under each condition
- mean IoU under each condition
- delta Dice relative to clean baseline
- perturbation-specific failure rates
- worst-case qualitative inspection

---

# Repository structure

```text
sam-brats-robustness-audit/
│
├── Notebook/
├── src/
│
├── brain_results.csv
├── brain_prompt_variants.csv
├── brain_robustness_summary.csv
├── brain_robustness_plot.png
├── brain_example_prediction.png
│
├── liver_results.csv
├── liver_robustness_summary.csv
├── liver_robustness_plot.png
├── liver_example_prediction.png
│
├── spleen_sam_boxprompt_metrics.csv
├── spleen_sam_robustness_results.csv
├── robustness_summary_table_FINAL.csv
├── worst_cases_overlay.png
│
├── kits23_tumor_sam_boxprompt_metrics.csv
├── kits23_tumor_sam_robustness_results.csv
├── kits23_tumor_robustness_summary_table.csv
├── kits23_tumor_worst_cases_overlay.png
├── kits23_tumor_robustness_barplot.png
├── kits23_tumor_summary.txt
│
├── monuseg_sam_boxprompt_metrics.csv
├── monuseg_sam_robustness_results.csv
├── monuseg_robustness_summary_table.csv
├── monuseg_worst_cases_overlay.png
├── monuseg_robustness_barplot.png
├── monuseg_summary.txt
│
├── combined_summary.csv
├── robustness_delta_dice_barplot.png
│
└── README.md

This repository is structured to move beyond a single-dataset demonstration.
It asks whether SAM behaves consistently across increasingly different difficulty types:
Liver / spleen CT: clearer abdominal organ boundaries
Brain tumor MRI: lesion segmentation in another modality
Kidney tumor CT: irregular lesion boundaries in CT
Histopathology nuclei: dense, small objects in a domain far from natural-image pretraining and radiology
This progression gives a stronger scientific narrative than simply adding more similar organs.
Main contribution
The main contribution of this repository is not just applying SAM to medical images. It is building a failure-aware, perturbation-aware audit framework for medical foundation model evaluation.
In practical terms, the repository contributes:
a cross-domain medical SAM benchmark
comparisons across CT, MRI, and histopathology
comparisons across organs, tumors, and nuclei
robustness testing under realistic perturbations
worst-case qualitative analysis
structured CSV and figure outputs for reproducible reporting


This repository is structured to move beyond a single-dataset demonstration.
It asks whether SAM behaves consistently across increasingly different difficulty types:
Liver / spleen CT: clearer abdominal organ boundaries
Brain tumor MRI: lesion segmentation in another modality
Kidney tumor CT: irregular lesion boundaries in CT
Histopathology nuclei: dense, small objects in a domain far from natural-image pretraining and radiology
This progression gives a stronger scientific narrative than simply adding more similar organs.
Main contribution
The main contribution of this repository is not just applying SAM to medical images. It is building a failure-aware, perturbation-aware audit framework for medical foundation model evaluation.
In practical terms, the repository contributes:
a cross-domain medical SAM benchmark
comparisons across CT, MRI, and histopathology
comparisons across organs, tumors, and nuclei
robustness testing under realistic perturbations
worst-case qualitative analysis
structured CSV and figure outputs for reproducible reporting


Typical workflow
prepare dataset
extract usable slices or images
create binary masks
derive box prompts from masks
run SAM inference
compute Dice and IoU
apply perturbations
export summary tables, overlays, and plots
Requirements
Typical core dependencies include:
torch
opencv-python
numpy
pandas
matplotlib
nibabel
tqdm
datasets
pillow
segment-anything


Limitations
A few important limitations apply:
bounding boxes are derived from ground truth, so this evaluates segmentation after localization is provided
most analyses are performed slice-wise or image-wise rather than as full 3D volumetric inference
histopathology evaluation uses a binary union of nuclei instances, not full instance-level separation
strong average performance does not eliminate the importance of low-frequency severe failures
results may vary with checkpoint choice, preprocessing strategy, and prompt design

