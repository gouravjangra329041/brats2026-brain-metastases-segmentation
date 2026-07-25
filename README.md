# Brain Metastases Segmentation — BraTS 2026 (Swin-UNETR)

A 3D deep learning pipeline for segmenting **pre- and post-treatment brain metastases**
on the official **BraTS 2026 Challenge (Task 1)** dataset, built with **PyTorch** and **MONAI**
using the **Swin-UNETR** architecture.

## Overview
Brain metastases are often small and multifocal, making them one of the harder
segmentation tasks in medical imaging. This project builds a complete, reproducible
pipeline that segments four clinically defined regions:

- **Tumor Core (TC)**
- **Whole Tumor (WT)**
- **Enhancing Tumor (ET)**
- **Resection Cavity (RC)** — for post-treatment cases

## Dataset
- **BraTS 2026 Challenge — Task 1** (Brain Metastases), accessed via Synapse after
  official registration and data-use agreement.
- Each patient has 4 MRI sequences: T1, T1-contrast (T1c), T2, and T2-FLAIR.
- Label scheme: 1 = NETC, 2 = SNFH, 3 = ET, 4 = RC.
- A 100-patient subset was used for training due to compute constraints.

## Method
- **Model:** Swin-UNETR (transformer-based 3D encoder–decoder)
- **Framework:** PyTorch + MONAI
- **Preprocessing:** channel-wise intensity normalization, foreground cropping,
  tumor-centered patch sampling (`RandCropByPosNegLabeld`), and data augmentation
- **Loss:** DiceCELoss (Dice + Cross-Entropy, sigmoid — for overlapping regions)
- **Inference:** sliding-window inference over the full volume
- **Evaluation:** Dice Score per region

## Results
- The model successfully learns to segment brain metastases on unseen validation patients.
- **Whole Tumor** is segmented well (predicted shape closely matches expert annotation);
  Tumor Core is reasonable; Enhancing Tumor and Resection Cavity are the most challenging.
- The current result is an early baseline, limited primarily by free-tier GPU compute.
  Prediction visualizations are included in this repository.

## Why Dice Score (not Accuracy)?
Tumors occupy a very small fraction of the brain. A model that predicts "no tumor"
could reach ~98% accuracy while detecting nothing. The Dice Score measures overlap
between predicted and true tumor regions, ignoring the healthy background — making it
the fair, standard metric used in the BraTS challenge.

## Tools
Python · PyTorch · MONAI · nibabel · Google Colab · Synapse

## Notes & Limitations
- Trained on a subset with limited epochs due to free-Colab GPU limits; more compute
  and the full dataset are expected to improve performance.
- Data is not included in this repository, in compliance with the BraTS data-use agreement.

## Author
Gourav Jangra
