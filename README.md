# LightThermNet
### Lightweight CNN-Based Thermal Image Denoising and Noise-Aware Visible–Infrared Fusion

LightThermNet is a deep-learning project that aims to improve thermal image quality using a lightweight Convolutional Neural Network (CNN). It combines thermal image denoising with a noise-aware visible–infrared fusion approach to support clearer image interpretation in surveillance and industrial monitoring applications.

## 1. Problem Statement

Thermal images can contain noise that obscures important heat patterns, edges, and scene details. This can make image interpretation difficult, particularly in low-light environments and industrial monitoring scenarios.

LightThermNet explores a lightweight neural-network approach to reduce thermal noise while preserving useful image structures.

## 2. Project Objectives

- Develop a lightweight CNN for thermal image denoising.
- Train and evaluate the denoising model using paired thermal-image data and simulated noise.
- Explore noise-aware fusion of denoised thermal images with corresponding visible-light images.
- Evaluate image reconstruction quality using quantitative metrics and visual comparisons.
- Investigate the potential for efficient thermal image enhancement in real-world applications.

## 3. Proposed Methodology

The project follows a two-stage approach:

1. **Thermal image denoising:** A lightweight CNN learns to reconstruct a clean thermal image from a noisy input.
2. **Visible–infrared fusion:** A separate fusion module combines the denoised thermal image with a corresponding visible-light image, aiming to retain useful information from both modalities.

Synthetic Gaussian and stripe-like noise are used in the denoising experiments to evaluate the model under controlled corruption.

## 4. Dataset

The project uses the MSRS (Multi-Spectral Road Scenarios) dataset, which contains paired visible-light and infrared images.

The dataset is expected to be obtained separately and configured according to the instructions provided with the project. The complete dataset is not included in this repository.

## 5. Technology Stack

- Python
- PyTorch
- NumPy
- OpenCV
- Matplotlib
- scikit-image
- Jupyter Notebook / Google Colab

## 6. Recorded Denoising Results

The recorded v5 experiment reported the following PSNR values:

| Metric | Value |
|---|---:|
| Noisy input PSNR | 25.313 dB |
| Denoised output PSNR | 33.757 dB |
| PSNR improvement | 8.444 dB |
| Model parameters | 31,138 |
| Recorded CPU inference time | Approximately 48.216 ms per frame |

These values apply to the recorded denoising evaluation setup. They should not be interpreted as proof of improved object detection or as a complete evaluation of the fusion module. Results may depend on the dataset split, noise-generation method, hardware, and evaluation configuration.

## 7. Repository Structure

- `notebooks/` — planned location for the training and experimentation notebook.
- `models/` — planned location for model definitions.
- `src/` — planned location for preprocessing, training, evaluation, and inference scripts.
- `app/` — planned location for the demonstration interface.
- `results/` — documentation and evaluation outputs.
- `checkpoints/` — instructions for obtaining trained model weights.

Some components may still be under development.

## 8. Installation

Clone the repository:

```bash
git clone https://github.com/jatyasnichedcoding/LightThermNet.git
cd LightThermNet
```

Create and activate a Python environment, then install the dependencies:

```bash
python -m venv .venv
```

On Windows:

```bash
.venv\Scripts\activate
```

Install the listed packages:

```bash
pip install -r requirements.txt
```

The dependency list and installation instructions must be verified against the final implementation before reproducing the experiments.

## 9. Current Limitations and Future Work

- Validate performance on real thermal-camera noise.
- Complete quantitative evaluation of the visible–infrared fusion module.
- Compare against suitable baseline denoising methods.
- Evaluate whether enhanced images improve downstream object detection.
- Develop and test a user-friendly inference prototype.
- Measure performance on the intended deployment hardware.

## 10. Applications

Potential applications include industrial heat monitoring, security surveillance, vehicle and traffic monitoring, fire-risk observation, and other scenarios where thermal image quality is important.

These are potential application areas; suitability must be established through appropriate testing.

## 11. Acknowledgements

This project uses the MSRS dataset and open-source Python machine-learning tools. Please consult the dataset's original documentation and relevant publications for attribution and usage terms.
