# A Robustness Audit of the Segment Anything Model for Multi-Domain Medical Image Segmentation Under Domain Shift

This repository accompanies the bioRxiv preprint.

---

## 📄 Paper

**Title:** A Robustness Audit of the Segment Anything Model for Five-Domain Medical Image Segmentation Under Domain Shift  
**Author:** Sanghati Basu  
**Affiliation:** MS Healthcare Informatics, University of Illinois Springfield  

**Preprint:** (Add bioRxiv link once live)  
**Code & Data:** This repository  

This work presents a systematic robustness evaluation of the Segment Anything Model (SAM, ViT-B) across five medical imaging domains: brain tumour MRI, liver CT, spleen CT, kidney tumour CT, and histopathology nuclei segmentation.

The study focuses on **failure-aware evaluation under domain shift**, rather than relying solely on average performance metrics.

---

## 🚀 Key Contributions

- Five-domain robustness evaluation across MRI, CT, and histopathology
- Failure-aware evaluation using dual thresholds (Dice < 0.5 and Dice < 0.1)
- Nine-condition perturbation protocol simulating real-world imaging variability
- Identification of domain-dependent performance tiers
- Discovery of a strong representational failure in histopathology (MoNuSeg)
- Cross-model comparison against nnU-Net and domain-specific baselines
- A deployment-oriented interpretation framework for Health Digital Twin (HDT) systems

---

## 📊 Main Results

| Domain | Dice | Failure Rate (Dice < 0.5) |
|------|------|---------------------------|
| Spleen CT | 0.914 | 0.7% |
| Kidney Tumour CT | 0.837 | 3.5% |
| Liver CT | 0.853 | 2.3% |
| Brain MRI | 0.515 | 39.3% |
| Nuclei (H&E) | 0.396 | 78.4% |

### Key Observations:
- CT-based organs show **high robustness and stability**
- MRI tumour segmentation shows **boundary-driven instability**
- Histopathology nuclei segmentation shows **near-complete failure**
- Domain characteristics matter more than perturbation noise

---

## 🧪 Robustness Evaluation Protocol

We evaluate SAM under 9 perturbation conditions:

- Gaussian Noise (σ = 10, 25)
- Gaussian Blur (k = 3, 7)
- Resolution Downsampling (0.5x, 0.25x)
- Contrast Scaling (0.8x, 1.2x)
- Gamma Correction (γ = 0.8, 1.2)

### Metrics:
- Dice Similarity Coefficient (DSC)
- Intersection over Union (IoU)
- Failure rates (Dice < 0.5, Dice < 0.1)
- Delta Dice relative to clean baseline

---

## 🧠 Key Insight

Performance variability in SAM is primarily driven by:

- Imaging modality (CT vs MRI vs histopathology)
- Boundary complexity
- Texture scale

### Domain Behavior:

- **CT (spleen, kidney, liver):** High performance, robust to perturbations  
- **MRI (brain tumour):** Unstable due to ambiguous tumour boundaries  
- **Histopathology (nuclei):** Representational failure due to mismatch with natural-image training  

This suggests that foundation models trained on natural images may not generalize reliably to fine-grained biomedical textures without domain adaptation.

---

## 🏥 Relevance to Health Digital Twins (HDT)

Segmentation models act as geometric front-ends for digital twin systems.

This work provides:
- A failure-aware evaluation framework
- A domain-based deployment tier system
- Evidence for where foundation models can be trusted in clinical pipelines

### Deployment Tiers:

- **Tier A:** Spleen, Kidney (High reliability)
- **Tier B:** Liver (Moderate reliability)
- **Tier C:** Brain MRI (Limited reliability)
- **Tier D:** Histopathology (Not suitable in zero-shot setting)

---

## 📂 Repository Structure
sam-brats-robustness-audit/
│
├── brain_mri/
├── liver_ct/
├── spleen_ct/
├── kits23_kidney/
├── monuseg_histopathology/
│
├── notebooks/
├── results/
├── figures/
├── scripts/
│
└── README.md

---

## ⚠️ Important Notes

- SAM is evaluated under **oracle bounding-box prompting (best-case scenario)**
- Results do **not represent end-to-end clinical deployment**
- Specialist model comparisons are based on **published benchmarks**
- This study focuses on **model behavior, not clinical validation**

---

## 📌 Citation

If you use this work, please cite:
@article{basu2026sam_robustness,
title={A Robustness Audit of the Segment Anything Model for Five-Domain Medical Image Segmentation Under Domain Shift},
author={Basu, Sanghati},
year={2026},
journal={bioRxiv (preprint)}
}

---

## 🔗 Acknowledgements

- Segment Anything Model (Meta AI)
- Medical Segmentation Decathlon
- KiTS23 Challenge
- MoNuSeg Dataset
- BraTS 2021 Dataset

---

## 📬 Contact

Sanghati Basu  
MS Healthcare Informatics  
University of Illinois Springfield 
