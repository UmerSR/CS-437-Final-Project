# Vision-Language Model (VLM)-based Alzheimer's Disease Detection

## Overview

This repository contains the codebase for a research project focused on the detection and classification of Alzheimer's Disease (AD) using a fine-tuned Vision-Language Model (VLM), specifically BioMedCLIP. This study explores both binary and multi-class classification scenarios, leveraging multimodal learning and interpretability techniques to achieve high diagnostic accuracy and transparency.

## Abstract

We propose a multimodal approach employing BioMedCLIP, enhanced by domain-specific textual captions and a dedicated classification head, to classify Alzheimer's Disease using MRI imaging data from the OASIS-I dataset. Our model integrates Eigen-CAM for interpretability, providing activation heatmaps highlighting influential neuroanatomical regions. The fine-tuned BioMedCLIP achieves an accuracy of 93.14% (binary classification) and 85.27% (multi-class classification), significantly outperforming baseline methods.

## Contents

* [Installation](#installation)
* [Dataset](#dataset)
* [Methodology](#methodology)

  * [Data Preprocessing](#data-preprocessing)
  * [Caption Generation](#caption-generation)
  * [Model Architecture](#model-architecture)
  * [Training Setup](#training-setup)
* [Results](#results)
* [Interpretability (Eigen-CAM)](#interpretability-eigen-cam)
* [Limitations](#limitations)
* [Future Work](#future-work)
* [References](#references)

## Installation

Clone this repository:

```bash
git clone https://github.com/UmerSR/CS-437-Final-Project.git
cd CS-437-Final-Project
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```

## Dataset

The study uses the [OASIS-I dataset](https://www.kaggle.com/datasets/ninadaithal/imagesoasis), containing approximately 80,000 T1-weighted MRI images classified into five diagnostic categories:

* Non-Demented
* Very Mild Dementia
* Mild Dementia
* Moderate Dementia

The dataset is preprocessed into standardized 2D slices for direct integration with Vision-Language Models.

## Methodology

### Data Preprocessing

* Selection of the middle axial slice (slice index 130) per patient for consistency and anatomical relevance.
* Image resizing and normalization tailored to BioMedCLIP requirements.

### Caption Generation

* Textual captions generated from demographic and clinical metadata (age, gender, MMSE, eTIV, nWBV, ASF) provided with the OASIS-I dataset.
* Standardized caption structure used consistently across MRI slices of each patient.

### Model Architecture

* Baseline model: BioMedCLIP, a pretrained VLM.
* Fine-tuning approach: Selective unfreezing of last two transformer blocks of BioMedCLIP visual encoder.
* Dedicated lightweight classification head:

  ```
  FFN(x) = Linear(1024, 256) → ReLU() → Dropout(0.3) → Linear(256, N)
  ```

### Training Setup

* Hardware: NVIDIA T4 GPU (Google Colab)
* Optimizer: Adam (learning rate = 1e-5)
* Loss Function: Binary Cross-Entropy (binary) and Cross-Entropy (multi-class)
* Train-Test Split: 80/20
* Epochs: 10

## Results

### Binary Classification

| Model                 | Accuracy   | AUC        |
| --------------------- | ---------- | ---------- |
| Vanilla BioMedCLIP    | 47.71%     | 0.4899     |
| Fine-tuned BioMedCLIP | **93.14%** | **0.9751** |

### Multi-Class Classification

| Model                            | Accuracy   | AUC        |
| -------------------------------- | ---------- | ---------- |
| Vanilla BioMedCLIP (ViT/B-16)    | 41.09%     | 0.7032     |
| Fine-tuned BioMedCLIP (ViT/B-16) | **85.27%** | **0.9654** |
| Fine-tuned BioMedCLIP (VGG-16)   | 75.19%     | 0.9517     |

## Interpretability (Eigen-CAM)

* Eigen-CAM used to visualize spatial attention and interpret model predictions.
* Highlights clinically relevant regions such as hippocampal shrinkage, ventricular enlargement, and cortical thinning.
* VGG-16-based models provided more clinically relevant activations compared to ViT-based models.

## Limitations

* Limited computational resources constrained broader model comparisons.
* Uniform caption per class rather than individual MRI slices.
* Small dataset size potentially affecting generalizability.

## Future Work

* Explore broader model variants and architectures.
* Develop individualized caption generation strategies.
* Conduct larger-scale studies for enhanced generalizability.

## References

The detailed list of references is available in the associated research paper included in this repository (`38_Paper.pdf`).

## Citation

Please cite this repository as follows if you use it in your research:

```
Ashraf, M. A., & Raja, U. (2024). Vision-Language Model (VLM)-based Alzheimer's Disease Detection [Source code]. GitHub. https://github.com/UmerSR/CS-437-Final-Project
```

---

For additional details, please refer to the research paper provided in the repository or contact the authors directly via GitHub issues.
