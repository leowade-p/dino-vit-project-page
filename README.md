# Patient-Level 3D Tumor Classification with Vision Transformers and Multiple Instance Learning

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.9+-blue.svg" alt="Python">
  <img src="https://img.shields.io/badge/PyTorch-2.0+-ee4c2c.svg" alt="PyTorch">
  <img src="https://img.shields.io/badge/Status-Manuscript%20in%20Preparation-red.svg" alt="Status">
</p>

### Overview
This project introduces a fully automated pipeline for patient-level tumor classification from 3D medical scans. Addressing the clinical limitations of single-slice 2D analysis, our method makes a holistic diagnosis by considering a patient's entire volumetric scan as a single entity. The framework leverages a **Vision Transformer (ViT)** within a **Multiple Instance Learning (MIL)** paradigm and achieves a high performance of **0.96 AUC** on our test dataset.

### Motivation
In clinical practice, a pathologist's diagnosis is rarely based on a single 2D tissue slice. Instead, they examine multiple slices to understand the full 3D context of a tumor, as a single cross-section can be ambiguous or unrepresentative. However, many conventional AI models are limited to single-slice 2D analysis, creating a disconnect with the clinical workflow and limiting their real-world value. Our goal is to bridge this gap by developing an automated system that mimics the expert's diagnostic process, analyzing the entire 3D scan to provide a more robust and clinically relevant patient-level classification.

### Methodology
Our pipeline consists of two main stages: (1) an efficient, slice-level feature extractor, and (2) a patient-level aggregator and classifier.

#### 1. Feature Extraction via Finetuned DINOv2
To extract meaningful features from each 2D slice, we use a powerful, pre-trained Vision Transformer, **DINOv2**. To adapt this foundation model to our specific medical imaging domain without incurring the massive computational cost of full fine-tuning, we employ **Low-Rank Adaptation (LoRA)**.

The process for each slice is as follows:
- The finetuned DINOv2 model generates a high-resolution feature map from the input image.
- **ROIAlign** is used to extract features specifically from the tumor's region of interest.
- The resulting features are flattened into a compact and informative feature vector representing that specific slice.

![(https://github.com/leowade-p/dino-vit-project-page/blob/main/feature-extraction.png?raw=true)](把你的feature-extraction.png图片链接粘贴到这里)

#### 2. Patient-Level Classification via MIL-ViT
We frame the 3D classification task as a **Multiple Instance Learning (MIL)** problem. A patient's entire 3D scan is treated as a "bag," and each extracted 2D slice feature vector is an "instance" within that bag.

The classification process is as follows:
- The feature vectors from all relevant slices of a single patient are collected into a sequence.
- A special `[CLS]` token is added to the beginning of this sequence.
- The entire sequence is passed through a **Transformer Encoder**, which uses its self-attention mechanism to weigh the importance of each slice and aggregate information across the entire 3D volume.
- The final representation of the `[CLS]` token, which now encapsulates a holistic understanding of the patient's scan, is fed into a simple MLP Head for the final binary classification into "Benign" or "Malignant".

![(https://github.com/leowade-p/dino-vit-project-page/blob/main/model-architecture.png?raw=true)](把你的model-architecture.png图片链接粘贴到这里)

### Results
Our pipeline achieves a high performance on the test dataset, reaching an **Area Under the Curve (AUC) of 0.96**. This strong result validates the effectiveness of our approach in making accurate, patient-level diagnoses from complex 3D medical data.

---

### Status & Code Access
*   **Status:** Manuscript in preparation.
*   **Code Access:** The source code for this project is currently held in a private repository pending publication. Access can be granted to faculty and collaborators on an individual basis upon request.
