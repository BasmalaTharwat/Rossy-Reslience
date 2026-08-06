# Rossy Resilience

**An AI-powered breast cancer detection and staging system**

Rossy Resilience is a multi-modal AI system designed to support breast cancer diagnosis and staging by integrating four data sources: **mammograms, ultrasound, MRI scans, and biopsy data**. The system combines deep learning models for detection, classification, segmentation, and staging into a unified diagnostic support pipeline, aiming to assist **radiologists and oncologists** with faster, more consistent, evidence-aligned assessments while also providing **patients** with a more accessible way to understand their results and next steps.

## Overview

Breast cancer diagnosis typically requires synthesizing information across multiple imaging modalities and pathology data — a process that can be time-consuming and highly dependent on specialist expertise. Rossy Resilience addresses this by developing dedicated pipelines for each modality, with the goal of integrating them into a unified system that can:

* Detect and classify suspicious findings in **mammograms**
* Classify breast ultrasound findings as **benign, malignant, or normal**
* Segment tumors and derive tumor measurements for **MRI-based staging**
* Incorporate **biopsy data** for pathological confirmation and staging refinement

## Key Features

* **Multi-modal architecture** — Separate, purpose-built pipelines for mammogram, ultrasound, MRI, and biopsy data designed to work together within one diagnostic workflow
* **Deep learning-based classification** — CNN-based models for classifying breast findings
* **Automated tumor segmentation** — Pixel/voxel-level tumor localization from medical imaging
* **Clinically grounded staging logic** — Tumor measurements are mapped to **AJCC 8th Edition T-staging criteria** through a rule-based staging module
* **Patient-facing support** — Recommended next steps after results, along with a chatbot to help patients navigate the platform and understand basic information about breast cancer

## 1. Ultrasound Classification Model

A **ResNet-50**-based deep learning model trained to classify breast ultrasound images into **benign, malignant, and normal** classes.

* **Architecture:** ResNet-50 with transfer learning
* **Input:** Breast ultrasound images

### Results

| Configuration                 | Accuracy | Malignant Recall |
| ----------------------------- | -------: | ---------------: |
| Before fine-tuning            |      83% |              78% |
| After fine-tuning (30 layers) |      88% |              91% |

Fine-tuning the last 30 layers of the backbone improved both overall accuracy and malignant recall, helping reduce the number of malignant cases missed by the model.

## 2. MRI Tumor Segmentation & T-Staging Pipeline

An end-to-end pipeline that segments breast tumors from **DCE-MRI** scans and derives clinical **T-stage classifications** from the resulting tumor measurements.

### Pipeline Components

* **Input representation:** DCE-MRI sequences stacked into 3-channel volumes to capture contrast-enhancement dynamics across time points
* **Segmentation architecture:** ResNet34-U-Net encoder-decoder architecture with residual connections
* **Post-processing:** Connected component analysis with relative thresholding to reduce segmentation noise while preserving relevant secondary lesions
* **Tumor measurement:** PCA-based diameter estimation from the segmented tumor volume
* **Staging:** Tumor measurements are mapped to **AJCC 8th Edition T-stage categories** using a rule-based staging module

### Segmentation Results

The ResNet34-U-Net with 3-channel input achieved:

* **Validation Dice:** 0.79
* **3D volumetric Dice:** 0.79 on the ISPY1 dataset
* **5-fold cross-validation:** Used to evaluate model stability and generalization

### Staging Results

* **Staging accuracy:** 91.3% (21/23 patients) on the held-out test set

## Repository Structure

```text
Rossy-Resilience/
│
├── Rossy_Resilience_Documentation.pdf
│
├── Rossy_Resilience_Presentation.pdf
│
├── demo.mp4
│
├── Model_Implementation/
│   │
│   ├── mri-staging.ipynb
│   │
│   └── ultrasound-classification.ipunb
│
└── README.md
```
