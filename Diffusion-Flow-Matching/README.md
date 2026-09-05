# DGM Assignment 4 — Diffusion & Flow Matching

Implementation and analysis of modern generative models, covering **Diffusion Models**, **Stable Diffusion**, **DreamBooth/LoRA**, and **Flow Matching** for financial time-series generation.

## Overview

This assignment explores both the theoretical foundations and practical implementation of modern generative modeling techniques.

The project is divided into two main parts:

1. **Diffusion Models** — implementation and analysis of DDPM, DDIM, Stable Diffusion, and fine-tuning techniques.
2. **Flow Matching** — learning and generating realistic financial time-series data using a continuous generative model.

## Part 1 — Diffusion Models

The first part focuses on understanding and implementing diffusion-based generative models.

### DDPM & DDIM

The implementation covers:

* Noise scheduling
* Forward diffusion process
* Reparameterization and input perturbation
* Noise prediction and training
* DDPM reverse sampling
* DDIM sampling and accelerated generation

The theoretical section also examines the closed-form diffusion process, noise prediction, VLB, DDIM sampling, and classifier-free guidance.

### Stable Diffusion

The project further explores **Latent Diffusion Models** and their main components:

* Variational Autoencoder (VAE)
* U-Net
* CLIP text encoder
* Latent-space diffusion

### DreamBooth & LoRA

A pretrained diffusion model is fine-tuned for a specific subject using a small number of example images.

The implementation includes:

* Instance and class data preparation
* Dataset processing
* Preservation Prior
* LoRA-based fine-tuning
* Model inference with different prompts and guidance scales

## Part 2 — Flow Matching

The second part focuses on **Flow Matching** and its application to financial time-series generation.

The model learns a time-dependent vector field that transforms samples from a simple Gaussian distribution into the target data distribution.

### Financial Time-Series

The dataset is based on **SPY** market data from 2010 to 2023.

The preprocessing pipeline includes:

* Converting adjusted closing prices to log returns
* Creating overlapping sequences of length 64
* Global standardization
* Chronological train/test split

The first 90% of the data is used for training and the final 10% is reserved exclusively for evaluation.

### Training

The Flow Matching model predicts the time-dependent vector field using a neural network.

The suggested training configuration includes:

```text id="8x3kpn"
Optimizer:    Adam
Batch Size:   512
Learning Rate: 2e-4
Epochs:       100
```

### Evaluation

Generated time-series are evaluated from both qualitative and statistical perspectives, including:

* Training and test loss
* Generated time-series visualization
* Real vs. generated samples
* Return distributions
* Mean, variance, and volatility
* Sliced Wasserstein Distance (SWD)
* Autocorrelation metrics

These evaluations measure both the similarity of the generated distribution and its temporal dependencies.
## Technologies

* Python
* PyTorch
* NumPy
* Pandas
* Matplotlib
* Diffusion Models
* Stable Diffusion
* LoRA / DreamBooth
* Flow Matching
* Statistical Evaluation

## Documentation

Directory contains the detailed theoretical analysis, implementation details, experimental results, and evaluation of the models in DGM-HW4-Report file.

## Assignment

**Course:** Deep Generative Models (DGM)
**University:** University of Tehran
**Instructor:** Dr. Mostafa Tavassolipour
**Assignment:** Homework 4
**Semester:** Fall 1404
