# VAE-Disentanglement-Study

A comprehensive study of Variational Autoencoders (VAE) and Beta-VAE for learning disentangled representations on the dSprites dataset. This project implements and compares standard VAE and Beta-VAE architectures to investigate how different beta values affect the disentanglement quality of learned latent representations.

##  Overview

This project implements a Variational Autoencoder (VAE) and its variant, Beta-VAE, to study disentangled representation learning. The goal is to learn latent representations where each dimension corresponds to a distinct generative factor (shape, scale, orientation, position) from the dSprites dataset.

**Key Objectives:**
- Implement a standard VAE with convolutional encoder-decoder architecture
- Implement Beta-VAE with different beta values (β = 1, 2, 4, 8, 16)
- Compare reconstruction quality and disentanglement across different beta values
- Visualize learned latent representations and their effects on generated images

##  Dataset

The project uses the **dSprites dataset**, which contains:
- **737,280 images** of 2D shapes (64×64 pixels, grayscale)
- **6 ground truth latent factors**: Shape, Scale, Orientation, Position X, Position Y, Color
- All possible combinations of these factors are present exactly once, making it ideal for disentanglement studies

**Dataset Source**: [dSprites Dataset](https://github.com/deepmind/dsprites-dataset)

##  Architecture

**VAE Architecture:**
- **Encoder**: 3 convolutional layers (1→32→64→128 channels) → Flatten → FC layers for μ and log(σ²)
- **Decoder**: FC layer → 3 transposed convolutional layers (128→64→32→1 channels)
- **Latent dimension**: 32
- **Loss function**: `L = BCE(x, x_recon) + β * KL(q(z|x) || p(z))`

##  Results

The project compares VAE and Beta-VAE models across different beta values:

- **β = 1**: Standard VAE (baseline)
- **β = 2, 4, 8, 16**: Beta-VAE with increasing KL divergence weight

### Key Findings:

**Trade-off Analysis:**
- **Higher beta values (β = 8, 16)**: Better disentanglement quality, but potentially lower reconstruction fidelity
- **Lower beta values (β = 1, 2)**: Better reconstruction quality, but potentially less disentangled representations
- **Optimal balance**: Intermediate beta values (β = 4) often provide a good trade-off

**Observations:**
- As beta increases, the KL divergence term becomes more dominant, encouraging the model to learn more independent latent factors
- Higher beta values lead to more structured latent spaces where individual dimensions control specific generative factors
- Reconstruction quality may decrease with higher beta values due to the stronger regularization constraint

**Results Include:**
- Training loss curves (total, reconstruction, KL divergence) for all beta values
- Visual comparisons of original vs. reconstructed images
- Latent space traversals showing the effect of varying individual latent dimensions
- Comparative analysis of disentanglement quality across different beta configurations

##  Features

-  Memory-efficient dataset loading using NumPy memory mapping
-  Standard VAE implementation with reparameterization trick
-  Beta-VAE with configurable beta values (β = 1, 2, 4, 8, 16)
-  Training with checkpoint saving and resuming
-  Comprehensive loss tracking (total, reconstruction, KL divergence)
-  Visualization tools for:
  - Training loss curves comparison
  - Original vs. reconstructed images
  - Latent space traversals
  - Beta value comparisons

##  Requirements

```python
torch >= 1.9.0
numpy >= 1.21.0
matplotlib >= 3.4.0
```

The project was developed and tested on Google Colab with GPU support.

##  Usage

### 1. Setup Dataset

Download the dSprites dataset and place it in your working directory:
- `dsprites_ndarray_co1sh3sc6or40x32y32_64x64.npz`

### 2. Configure Paths

Update the dataset path in the notebook:
```python
folder_path = '/path/to/your/dsprites_dataset'
npz_path = os.path.join(folder_path, 'dsprites_ndarray_co1sh3sc6or40x32y32_64x64.npz')
```

### 3. Training

The notebook includes:
- **Standard VAE training** (β = 1)
- **Beta-VAE training** with multiple beta values (β = 1, 2, 4, 8, 16)

**Training hyperparameters:**
- Epochs: 50
- Batch size: 128
- Learning rate: 0.001
- Latent dimension: 32
- Optimizer: Adam

### 4. Evaluation

After training, the notebook provides:
- Loss curve visualizations comparing all beta values
- Reconstruction quality comparisons
- Latent space analysis and traversals

##  Project Structure

```
VAE-Disentanglement-Study/
│
├── DGM_HW1.ipynb              # Main notebook with implementation
├── DGM_HW1.pdf                # Assignment description
├── Report.pdf                 # Project report (Persian)
└── README.md                  # This file
```

##  Report

The project includes a detailed report in Persian (`HW1_Hashemi_810101549.pdf`) covering:
- Theoretical background of VAE and Beta-VAE
- Implementation details and architecture design
- Experimental results and comprehensive analysis
- Discussion of findings and trade-offs between reconstruction and disentanglement

---

**Note**: This project was completed as part of a Deep Generative Models course assignment. The implementation follows the architecture specifications provided in the assignment.

