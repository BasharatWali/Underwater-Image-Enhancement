# Underwater Image Enhancement Pipeline

This repository contains an implementation of an underwater image enhancement pipeline. The project reproduces the DIRS-CLAHS method and extends it with several improvements to restore color balance and enhance local details in underwater images.

## Overview

Underwater images often suffer from low contrast, color imbalance, and haze due to the water medium. Our approach first reproduces the baseline DIRS-CLAHS method and then improves it by:
- Compensating for color attenuation (boosting the red channel)
- Refining global and local contrast correction
- Integrating a Multi-Scale Retinex module with color restoration
- Applying post-processing (gamma correction and weighted blending)

## Pipeline

The enhancement pipeline consists of the following steps:

- **Input:** Read the underwater image (and reference image if available for evaluation).
- **Color Attenuation Compensation (CAC):**  
  - Split the image into its B, G, and R channels.
  - Compute average intensities and derive a red compensation factor.
  - Boost the red channel while slightly attenuating the blue channel.
- **Global Contrast Correction:**  
  - Compute the 5th and 95th percentiles for each color channel.
  - Apply a piecewise linear stretch that maps the lower intensity region to [0, 127] and the higher region to [128, 255].
- **Adaptive Local Enhancement:**  
  - Convert the image to the LAB color space.
  - Apply CLAHE on the luminance (L) channel with an adaptive clip limit based on the image's brightness.
- **Multi-Scale Retinex with Color Restoration (MSRCR):**  
  - For each color channel, compute the Single-Scale Retinex (SSR) using Gaussian blurring with multiple scales.
  - Combine the SSR outputs using weighted averaging.
  - Apply a color restoration function (CRF) to mitigate desaturation.
- **Gamma Correction and Blending:**  
  - Apply mild gamma correction to adjust brightness.
  - Blend the gamma-corrected image with the locally enhanced image to achieve a balanced final output.
- **Evaluation:**  
  - Compute full-reference metrics (PSNR, SSIM, MSE) when a reference image is available.
  - Always compute no-reference metrics (UIQM, UCIQE) to assess visual quality.

## Evaluation Metrics

We use the following metrics:
- **PSNR:** Peak Signal-to-Noise Ratio
- **SSIM:** Structural Similarity Index
- **MSE:** Mean Squared Error
- **UIQM:** Underwater Image Quality Measure
- **UCIQE:** Underwater Color Image Quality Evaluation
