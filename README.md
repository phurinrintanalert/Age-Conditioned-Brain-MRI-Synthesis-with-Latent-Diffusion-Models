# Age-Conditioned-Brain-MRI-Synthesis-with-Latent-Diffusion-Models
Synthesizing age-conditioned neonatal brain MRIs using Latent Diffusion Models (LDM) and VAEs in PyTorch

## DataFiles
Data files can be downloaded via this link: https://drive.google.com/drive/folders/14WBgNmkFd2bSw9_USNIm7wiR-YZ6fjdC?usp=share_link

## Overview
This repository contains the implementation of an age-conditioned Latent Diffusion Model (LDM) designed to synthesize realistic 2D axial slices of neonatal brain MRIs. Taking inspiration from the foundational work by Rombach et al. (High-Resolution Image Synthesis with Latent Diffusion Models), this project demonstrates a complete generative AI pipeline from data preprocessing to conditional image generation.

## Dataset
The dataset consists of approximately 800 2D axial slices through neonatal brain MRIs. The Gestational Age (GA) at the time of the scan ranges from ~30 to 40 weeks. Each slice includes an associated ground-truth label indicating the subject's exact GA. 

## Project Pipeline
This project is broken down into four core stages:

1. **Data Augmentation:** To ensure robust model training and prevent overfitting, stochastic data augmentation was implemented. This includes applying 5% Gaussian noise to simulate realistic scan-to-scan variations across different MRI machines, as well as random rotations (+/- 30 degrees).
2. **Age Regression Baseline:** A ResNet-based encoder was built and trained to predict the Gestational Age (GA) of a given brain slice. This supervised regression task established a strong baseline understanding of the dataset features, achieving an average Mean Absolute Error (MAE) of ~0.46 weeks on the test set.
3.**Variational Autoencoder (VAE):** A complete ResNet VAE was trained to compress the high-dimensional pixel space of the MRIs into a lower-dimensional latent representation. The model was optimized using MSE, KL Divergence, and Perceptual Loss (via MONAI), paired with a patch-based adversarial discriminator to retain high-fidelity structural details.
4. **Age-Conditioned Latent Diffusion Model (LDM):** A conditional U-Net was implemented to learn the denoising process entirely within the VAE's latent space. By conditioning the U-Net on the Gestational Age, the model learned to synthesize realistic brain morphologies corresponding to specific developmental milestones (e.g., 32 weeks vs. 38 weeks). 

## Results & Evaluation
Both the VAE and LDM were evaluated against real ground-truth images:
* **VAE Reconstructions:** Successfully captured the gross structure of the neonatal brain, though exhibited minor blurring around the complex cortical folds.
* **LDM Generations:** Plausible de novo image generation capable of capturing general brain folding patterns and ventricles conditioned on age. While slightly less sharp than the VAE reconstructions, the LDM successfully demonstrates controlled, generative synthesis.

## Acknowledgements
* Rombach et al., "High-Resolution Image Synthesis with Latent Diffusion Models" (CVPR 2022).
