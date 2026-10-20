# LightThermNet
### Lightweight CNN-Based Thermal Image Denoising and Noise-Aware Visible–Infrared Fusion

LightThermNet is a research project exploring lightweight thermal image denoising and visible–infrared fusion. It uses a residual CNN to estimate noise in grayscale thermal images and a separate noise-aware fusion gate to combine denoised thermal information with a paired visible-light image.

## Project goals

- Reduce simulated noise in thermal images while preserving useful image structure.
- Explore a compact CNN architecture with a low parameter count.
- Combine visible-light and infrared information using a learned, spatially varying fusion gate.
- Evaluate denoising with reference-based metrics and qualitative examples.
- Provide an interactive Gradio interface, **Thermal Fusion Studio**, for trying image pairs.

## Repository contents

- `LightThermNet.ipynb` — training, evaluation, and the Gradio prototype.
- `requirements.txt` — Python package dependencies.
- `checkpoints/best_checkpoint_v5.pth` — trained v5 denoising and fusion model weights and training metadata.
- `results/` — recorded visual comparisons and evaluation plots, where uploaded.
- `.gitignore` — common files to exclude from version control.

The MSRS dataset is not included in this repository. Obtain it according to its original terms for full training/reproduction.

## Open the notebook in Google Colab

[Open LightThermNet in Google Colab](https://colab.research.google.com/github/jatyasnichedcoding/LightThermNet/blob/main/LightThermNet.ipynb)

Google Colab is the intended environment for this notebook.

## Run the interactive prototype without retraining

**Do not select “Run all” if you only want to demonstrate the app.** The notebook also contains dataset setup, training, and evaluation cells, which are not needed for inference and may take a long time.

1. Open the notebook in Colab and connect to a runtime.
2. Run the first code cell (`Imports / Configuration`). It does not need to mount Google Drive for the demo-only path.
3. Run the model-architecture cell that defines `ThermalNoiseResidualCNN`, `NoiseAwareFusionGate`, creates `model` and `fusion_model`, and defines their inference helpers.
4. Skip dataset discovery, preprocessing, training, evaluation, and optional YOLO cells.
5. Run the final cell titled **“THERMAL FUSION STUDIO — INTERACTIVE COLAB APP”**. It checks for the checkpoint locally or in Google Drive and otherwise downloads it from the public repository at `checkpoints/best_checkpoint_v5.pth`.
6. Open the temporary Gradio link printed below the cell. Upload a matching visible-light image and thermal image, process them, inspect the denoised/fused outputs, and optionally download the generated result package.

The checkpoint must contain both `model_state_dict` and `fusion_model_state_dict` from the v5 architecture. If the GitHub download fails, confirm that the checkpoint file is present in the public repository and named exactly `best_checkpoint_v5.pth`.

### Sharing the live app

The Gradio share link is temporary and works only while the Colab runtime and app remain active. To let your professor use the currently running instance, share the live link while it is active. For independent reproduction, they need their own Colab runtime. They can run the demo-only sequence above without the MSRS dataset; the dataset is required for full retraining.

## Train and evaluate the model

1. Obtain the MSRS dataset and place/extract it under `MyDrive/Research_Project/`, preserving paired `train/vi` and `train/ir` directories (and a test split if available).
2. Open `LightThermNet.ipynb` in Colab and run notebook cells in order. The dataset/training path mounts Drive when needed.
3. Confirm the printed dataset root and matching image-pair count before training.
4. Review the final evaluation output and saved plots/checkpoint. Training from scratch can take time and may produce results different from the recorded run.

## Method summary

### Denoising

The `ThermalNoiseResidualCNN` estimates the added noise in a grayscale thermal input. Its predicted noise is subtracted from the noisy image and clipped to the valid normalized range. Training generates simulated Gaussian and vertical stripe-like corruption from clean thermal targets.

### Visible–infrared fusion

The `NoiseAwareFusionGate` receives the visible image, contrast-enhanced denoised thermal image, and a normalized noise map. It estimates a spatially varying visible-image contribution and combines the visible and thermal information. The fusion output should be treated as an experimental result; the fusion training target is a proxy, not a ground-truth fused reference image.

## Recorded denoising metrics

The recorded v5 experiment reported:

| Metric | Recorded value |
|---|---:|
| Noisy input PSNR | 25.313 dB |
| Denoised output PSNR | 33.757 dB |
| PSNR improvement | 8.444 dB |
| Combined model parameters | 31,138 |

These values describe the recorded denoising evaluation and its noise-generation/evaluation setup. Reproduce the corresponding evaluation before treating them as newly verified results. They do not establish improved object-detection accuracy or independently validate fusion quality.

## Environment and dependencies

The notebook uses Python with PyTorch, NumPy, OpenCV, Matplotlib, tqdm, Pillow, and Gradio. Google Colab is recommended. `requirements.txt` is a starting dependency list; versions should be pinned after a clean environment test.

## Limitations and future work

- Evaluate on real thermal-camera noise and additional datasets.
- Compare denoising against established baselines.
- Complete independent quantitative evaluation of the fusion component.
- Test whether enhancement improves a downstream detection or segmentation system before making detection-performance claims.
- Verify runtime and memory use on the intended deployment hardware.

## Dataset and attribution

The project uses the MSRS (Multi-Spectral Road Scenarios) dataset. Consult the dataset's original documentation for citation, access, and usage terms. The dataset itself is not redistributed here.
