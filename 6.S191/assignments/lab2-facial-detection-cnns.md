# 6.S191 Lab 2: Facial Detection, CNNs, and Debiasing

## Problem Statement
Lab 2 starts with image classification basics and ends with a fairness problem. The warm-up is MNIST digit recognition. The real target is facial detection: decide whether an image contains a face, then stop the model from becoming better on some demographics than others.

The lab is useful because it shows two things at once. A CNN can be a strong classifier and still be biased. And bias can come from the data distribution, not just from a bad architecture.

## Key Techniques
- **Fully connected baseline.** The first part of the lab builds a simple network for MNIST so the CNN comparison is grounded.
- **Convolutional feature extraction.** The CNN uses convolution and pooling to learn spatial features from images.
- **CelebA and ImageNet pairing.** Faces come from CelebA, non-faces from ImageNet.
- **Demographic evaluation.** The model is checked across dark/light and male/female groups instead of only on aggregate accuracy.
- **VAE latent modeling.** A variational autoencoder learns an unsupervised latent representation of face images.
- **KL plus reconstruction loss.** The VAE balances a latent-space regularizer with image reconstruction quality.
- **DB-VAE adaptive resampling.** Learned latent structure is used to upweight rare face features during training.

## Implementation Walkthrough
1. Run the MNIST warm-up and compare a fully connected classifier with a CNN.
2. Load CelebA and ImageNet through the course helper utilities to create face and non-face training sets.
3. Train a standard CNN face detector and evaluate it on the training set and on the PPB-style demographic test split.
4. Inspect the accuracy gap across the four demographic groups. This is the bias signal.
5. Define the VAE encoder and decoder. The encoder outputs latent means and variances, then uses reparameterization to sample latent variables.
6. Implement the VAE loss as KL divergence plus reconstruction loss.
7. Build the DB-VAE on top of the CNN encoder and use latent-frequency estimates to resample training examples.
8. Train the DB-VAE and compare its demographic performance against the standard CNN.

## What You Learn
- CNNs solve the image problem, but they do not solve the fairness problem.
- Aggregate accuracy hides subgroup failure.
- Latent-variable models can recover hidden structure without manual subgroup annotation.
- Adaptive sampling is a practical way to turn fairness from a post-hoc audit into part of the training loop.

## Sources
- `https://github.com/MITDeepLearning/introtodeeplearning/blob/master/README.md`
- `https://github.com/MITDeepLearning/introtodeeplearning/blob/master/lab2/PT_Part1_MNIST.ipynb`
- `https://github.com/MITDeepLearning/introtodeeplearning/blob/master/lab2/PT_Part2_Debiasing.ipynb`
- `https://github.com/MITDeepLearning/introtodeeplearning/blob/master/mitdeeplearning/lab2.py`
- `/tmp/introtodeeplearning/README.md`
- `/tmp/introtodeeplearning/lab2/PT_Part1_MNIST.ipynb`
- `/tmp/introtodeeplearning/lab2/PT_Part2_Debiasing.ipynb`
- `/tmp/introtodeeplearning/mitdeeplearning/lab2.py`
