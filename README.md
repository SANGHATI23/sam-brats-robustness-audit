# SAM Robustness Audit for Brain MRI Tumor Segmentation

This repository evaluates the Segment Anything Model (ViT-B) for 2D brain tumor segmentation on BraTS MRI slices.

## Overview

Foundational models such as SAM show strong zero-shot segmentation capability. However, their robustness under domain shift in medical imaging remains underexplored.

This project benchmarks:

- Baseline segmentation performance (Dice, IoU, Sensitivity, Specificity)
- Robustness under controlled perturbations:
  - Gaussian noise
  - Contrast scaling
  - Downsampling
  - Intensity bias field
- Stability metrics:
  - Relative performance drop (ΔDice)
  - Failure rate (Dice < 0.5)
  - Variance increase under perturbation

## Prompting Strategy

Bounding-box prompts are automatically derived from ground-truth masks to simulate minimal annotation.

## Dataset

BraTS dataset (not included due to licensing restrictions).

Expected structure:

data/
  images/
  masks/

## Reproducibility

Install dependencies:

```bash
pip install -r requirements.txt
