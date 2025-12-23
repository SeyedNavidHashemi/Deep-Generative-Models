# MAF and CycleGAN Implementations

This project explores two distinct classes of Deep Generative Models: **Normalizing Flows** for density estimation and anomaly detection, and **CycleGAN** for unpaired image-to-image translation. The implementation covers the theoretical foundations and practical applications of **Masked Autoregressive Flow (MAF)** and **Cycle-Consistent Adversarial Networks (CycleGAN)** using PyTorch.

## Overview

The project is divided into two main sections:

1.  **Normalizing Flows (MAF):** Implements a Masked Autoregressive Flow model to learn the probability density of the MVTec AD dataset. The learned density is utilized for unsupervised anomaly detection, distinguishing between defective and defect-free samples.
2.  **CycleGAN:** Implements the CycleGAN architecture to perform image-to-image translation between two unpaired domains (Horses $\leftrightarrow$ Zebras) without requiring one-to-one mapping in the training data.

---

## Part 1: Masked Autoregressive Flow (MAF)

### Goal
To implement a flow-based generative model that transforms a simple base distribution (e.g., Gaussian) into a complex data distribution using a sequence of invertible and differentiable transformations. The primary application demonstrated here is **Anomaly Detection**.

### Dataset
* **Dataset:** MVTec AD (Capsule category).
* **Content:** Contains images of "good" (defect-free) capsules for training, and both "good" and "defective" (e.g., crack, squeeze, scratch) capsules for testing.
* **Preprocessing:** Images are resized to $128 \times 128$ and **dequantized** (adding uniform noise) to convert discrete pixel values into continuous data, a requirement for strictly training flow-based models.

### Architecture
The MAF model is composed of stacked **Masked Autoencoder for Distribution Estimation (MADE)** blocks:
* **MaskedLinear Layers:** Custom linear layers with binary masks to enforce the autoregressive property (i.e., the $i$-th output depends only on the first $i-1$ inputs).
* **MADE Blocks:** Each block models a conditional probability distribution.
* **Flow Composition:** Multiple MADE blocks are stacked to increase expressivity.
* **Permutation:** Random permutations are applied between blocks to ensure all dimensions can influence each other over the deep network.

### Key Results
* **Density Estimation:** The model successfully learns to assign high likelihoods to normal data.
* **Anomaly Detection:** By evaluating the log-likelihood of test images, the model detects anomalies. Defective images yield significantly lower likelihood scores compared to normal ones.
* **Metrics:** The performance is evaluated using the **Area Under the Receiver Operating Characteristic (AUROC)** curve, demonstrating the model's effectiveness in unsupervised anomaly detection.

---

## Part 2: CycleGAN

### Goal
To learn a mapping between two image domains $X$ (Horses) and $Y$ (Zebras) using unpaired training data. The model learns functions $G: X \rightarrow Y$ and $F: Y \rightarrow X$ such that images translated from one domain to the other are indistinguishable from real images.

### Dataset
* **Dataset:** `horse2zebra` dataset.
* **Type:** Unpaired image collection.
* **Preprocessing:** Resized to $128 \times 128$, normalized to $[-1, 1]$.

### Architecture
* **Generators ($G_{AB}$ and $G_{BA}$):**
    * ResNet-based architecture with residual blocks to preserve spatial information.
    * Uses Instance Normalization and Reflection Padding to reduce artifacts.
* **Discriminators ($D_A$ and $D_B$):**
    * **PatchGAN** architecture: Classifies $N \times N$ patches of an image as real or fake, rather than the whole image, capturing high-frequency local texture details.

### Loss Functions
The model optimizes a composite objective function:
1.  **Adversarial Loss:** Ensures generated images look realistic in the target domain.
2.  **Cycle Consistency Loss:** Ensures that $F(G(x)) \approx x$ and $G(F(y)) \approx y$. This prevents mode collapse and ensures the learned mapping is geometrically consistent.
3.  **Identity Loss:** Ensures that generators preserve the content of images that already belong to the target domain (e.g., $G(y) \approx y$).

### Results
* **Style Transfer:** The model successfully transforms horses into zebras and vice-versa while maintaining the original pose and background.
* **Loss Curves:** Tracking of Generator, Discriminator, Cycle, and Identity losses confirms stable training dynamics.

---

## Project Structure

```
MAF-and-CycleGAN-Implementations/
│
├── MAF.ipynb # Implementation of Masked Autoregressive Flow
├── CycleGAN.ipynb # Implementation of CycleGAN
├── DGM_HW2.pdf # Assignment requirements and theoretical questions
├── Report.pdf # Detailed project report (Persian)
└── README.md # This file
```

---
## Requirements

* Python >= 3.7
* PyTorch >= 1.9.0
* Torchvision
* NumPy
* Matplotlib
* Pillow
* Google Colab (Recommended for GPU training)

## Usage

### 1. Setup Environment
Open the notebooks in Google Colab or a local Jupyter environment with GPU support.

### 2. Dataset Preparation
* **MAF:** The notebook automatically handles extracting the `capsule.zip` dataset (ensure the zip file is available in your Drive or upload it).
* **CycleGAN:** The notebook includes scripts to download and extract the `horse2zebra` dataset directly.

### 3. Training
* Run the cells in `MAF.ipynb` to train the flow model. Training typically takes ~100 epochs for convergence.
* Run the cells in `CycleGAN.ipynb` to train the GANs. Training is computationally intensive; 100-200 epochs are recommended for high-quality results.

### 4. Evaluation
* **MAF:** The notebook generates histograms comparing log-likelihoods of normal vs. anomalous samples and plots the ROC curve.
* **CycleGAN:** The notebook visualizes test samples after every epoch to monitor the quality of the translation (e.g., Horse $\rightarrow$ Zebra).

---
*Note: This project was completed as part of the Deep Generative Models course assignment (HW2). It includes detailed theoretical derivations for Normalizing Flows (Jacobian, Change of Variables) and CycleGANs in the accompanying report.*
