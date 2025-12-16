# FakeImageScan
**Beyond Visual Artifacts: Detecting AI-Generated Images via Model Affinity and Task Difficulty**

FakeImageScan is a **dual-pathway framework** for detecting AI-generated images produced by **GAN** and **diffusion models**.  
Instead of relying on fragile visual artifacts, FakeImageScan exploits **model affinity** and **task difficulty** signals to achieve **robust and generator-agnostic detection**.

---

## 📌 Key Idea
> **AI-generated images are more compatible with AI models than real images.**

FakeImageScan detects this asymmetry by measuring:
- how *well* AI models perform on an image (**Model Affinity**), and
- how *difficult* the image should be for those tasks (**Task Difficulty**).

The mismatch between the two provides a powerful detection cue.

---

## 🔍 Framework Overview

<p align="center">
  <img src="Overview.PNG" width="700">
</p>

FakeImageScan consists of **two complementary pathways**:

### 1️⃣ Model Affinity Pathway (Performance Signals)
Measures how confidently AI models process an image.

- **Inpainting Accuracy (A)**  
  Evaluates reconstruction fidelity using SSIM after masked inpainting.
- **Segmentation Confidence (S)**  
  Measures average per-pixel confidence from a semantic segmentation model.

### 2️⃣ Task Difficulty Pathway (Context Signals)
Estimates how challenging the image should be for AI models.

- **Pixel Naturalness (N)**  
  Quantifies predictability using global and local pixel probability models.
- **Image Complexity (C)**  
  Combines edge density, texture variance, entropy, and frequency-domain cues.

---
## 📂 Dataset Structure
The dataset follows a **binary folder structure** with only two classes: **Real** and **AI-Generated** images.

```text
data/
├── real/
│   ├── img_0001.jpg
│   ├── img_0002.png
│   ├── img_0003.jpeg
│   └── ...
└── ai_generated/
    ├── img_1001.png
    ├── img_1002.jpg
    ├── img_1003.webp
    └── ...

---
## 🧠 Feature Vector
For each image `I`, FakeImageScan extracts a 4D feature vector:

**x(I) = [ A(I), S(I), N(I), C(I) ]**

Where:
- **A(I)**: Inpainting Accuracy (SSIM-based)
- **S(I)**: Segmentation Confidence
- **N(I)**: Pixel Naturalness
- **C(I)**: Image Complexity

Final classification is performed using an **SVM (RBF kernel)**.



## 📁 Repository Structure

```text
FakeImageScan/
│
├── models/
│   ├── inpainting/
│   ├── segmentation/
│   └── naturalness/
│   └── Complexity/
│
├── features/
│   ├── inpainting_accuracy.py
│   ├── segmentation_confidence.py
│   ├── pixel_naturalness.py
│   └── image_complexity.py
│
├── classifier/
│   └── svm_classifier.py
│
├── data/
│   ├── real/
│   └── synthetic/
│
├── scripts/
│   ├── extract_features.py
│   ├── train.py
│   └── evaluate.py
│
├── Overview.PNG
├── requirements.txt
└── README.md
