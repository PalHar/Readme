# BATS-Net: A Lightweight Boundary-Aware Transition Statistics Network for Subretinal Fluid Segmentation in Fundus Images

Official implementation of **BATS-Net**, a lightweight deep learning framework for **subretinal fluid (SRF) segmentation in fundus images**.

---

# Abstract

Accurate delineation of **Central Serous Chorioretinopathy (CSC)** lesions in fundus images is essential for disease monitoring and treatment assessment, yet remains challenging due to subtle appearance variations, diffuse boundaries, and low contrast between affected and healthy retinal regions.

We propose **BATS-Net**, a lightweight **Boundary-Aware Transition Statistics Network** that formulates SRF segmentation as a **transition-statistics learning problem**. The proposed framework explicitly models:

* **First-order local appearance deviation**
* **Second-order feature variance**

to characterize **boundary instability and texture heterogeneity** in fundus images.

These compact transition representations are refined through a **transition encoder** and a **multi-scale spatial coherence module**, enabling robust contextual aggregation for accurate SRF boundary delineation.

Experiments on a manually annotated CSC fundus dataset demonstrate that **BATS-Net achieves competitive segmentation performance while reducing model parameters by nearly 60–70× compared to heavy encoder–decoder architectures**, making it suitable for efficient clinical deployment and large-scale screening.

---

# Method Overview

The proposed **BATS-Net** architecture consists of four main components:

1. Texture Encoder
2. Transition Statistics Modeling
3. Spatial Coherence Module
4. Segmentation Prediction Head

---

# Network Architecture

![BATS-Net Architecture](figures/batsnet_architecture.png)

*Figure: Architecture of the proposed BATS-Net. The network models local transition statistics and refines them using spatial coherence for accurate SRF segmentation.*

---

## Architecture Pipeline

```
Fundus Image
     │
     ▼
Texture Encoder
     │
     ▼
Transition Statistics Modeling
  (Appearance Deviation + Feature Variance)
     │
     ▼
Transition Encoder
     │
     ▼
Spatial Coherence Module
 (Dilated Convolutions)
     │
     ▼
Segmentation Head
     │
     ▼
SRF Segmentation Mask
```

---

# Dataset

Experiments were conducted on a **manually annotated CSC fundus dataset**.

The dataset contains:

* **1050 fundus images**
* **Pixel-level SRF segmentation masks**

Each dataset sample contains the **fundus image and mask concatenated into a single image file**.

During preprocessing:

* **Left half → Fundus image**
* **Right half → Ground truth mask**

---

## Dataset Statistics

| Property         | Value               |
| ---------------- | ------------------- |
| Total Images     | 1050                |
| Training Images  | 735                 |
| Testing Images   | 315                 |
| Image Resolution | 256 × 256           |
| Annotation       | Pixel-wise SRF mask |

---

# Qualitative Results

![Segmentation Results](figures/segmentation_results.png)

*Example qualitative segmentation results. From left to right: Input fundus image, ground truth mask, and BATS-Net prediction.*

The proposed BATS-Net produces segmentation masks that closely match expert annotations while preserving lesion boundary details.

---

# Training Details

The model is implemented in **PyTorch**.

### Hyperparameters

| Parameter     | Value             |
| ------------- | ----------------- |
| Batch size    | 4                 |
| Epochs        | 200               |
| Optimizer     | Adam              |
| Learning rate | 1e-4              |
| LR Scheduler  | ReduceLROnPlateau |

### Data Augmentation

* Random horizontal flips
* Random vertical flips

---

# Loss Function

The network is trained using a hybrid loss:

```
L = λ1 * BCE + λ2 * Dice
```

where

* **BCE Loss** handles pixel-wise classification
* **Dice Loss** addresses class imbalance

---

# Quantitative Results

| Method       | Dice       | IoU        | Params (M) |
| ------------ | ---------- | ---------- | ---------- |
| U-Net        | 0.7349     | 0.5810     | 31.03      |
| U-Net++      | 0.9186     | 0.8690     | 36.63      |
| DeepLabV3+   | 0.9285     | 0.8667     | 40.35      |
| **BATS-Net** | **0.9194** | **0.8510** | **0.58**   |

BATS-Net achieves **competitive performance with significantly fewer parameters and computational cost**.

---

# Installation

### Requirements

* Python ≥ 3.8
* PyTorch
* NumPy
* OpenCV
* Matplotlib
* scikit-learn

Install dependencies:

```
pip install -r requirements.txt
```

---

# Project Structure

```
BATS-Net
│
├── dataset
│
├── models
│   └── batsnet.py
│
├── train.py
├── test.py
├── utils.py
│
├── checkpoints
│
├── figures
│   ├── batsnet_architecture.png
│   └── segmentation_results.png
│
└── README.md
```

---

# Future Work

* **5-fold cross-validation** for more robust evaluation
* Comparison with additional **lightweight segmentation architectures**
* Extension to other **retinal disease segmentation tasks**

---

# Citation

If you find this work useful, please cite:

```
@article{batsnet2026,
title={BATS-Net: A Lightweight Boundary-Aware Transition Statistics Network for Subretinal Fluid Segmentation in Fundus Images},
author={Anonymous},
journal={Submitted to MICCAI},
year={2026}
}
```

---

# Acknowledgements

This work focuses on **efficient deep learning models for retinal image analysis**, aiming to enable scalable automated screening systems.
