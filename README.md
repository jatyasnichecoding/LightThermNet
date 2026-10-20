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

- `LightThermNet.ipynb` — training, evaluation, and the existing Gradio prototype.
- `requirements.txt` — Python package dependencies.
- `results/` — recorded visual comparisons and evaluation plots, where uploaded.
- `.gitignore` — common files to exclude from version control.

The full MSRS dataset and trained checkpoint are not bundled in this repository. Obtain the dataset according to its original terms and arrange access to the v5 checkpoint before running the app.

## Open the notebook in Google Colab

[Open LightThermNet in Google Colab](https://colab.research.google.com/github/jatyasnichedcoding/LightThermNet/blob/main/LightThermNet.ipynb)

Google Colab is the intended environment for this notebook. It uses Google Drive paths and may not run unchanged in a local Python environment.

## Run the interactive prototype without retraining

**Do not select “Run all” if you only want to demonstrate the app.** The notebook also contains dataset setup, training, and evaluation cells, which are not needed for inference and may take a long time.

1. Open the notebook in Colab and connect to a runtime.
2. Make sure you have access to the trained checkpoint `best_checkpoint_v5.pth`. Place a copy in your own Google Drive at:
   `MyDrive/Research_Project/Checkpoints/best_checkpoint_v5.pth`
3. Run the first code cell, which imports the libraries, mounts Drive, sets the device and image size, and creates the checkpoint directory.
4. Skip the dataset-discovery cell. Run the code cell that defines `ThermalNoiseResidualCNN`, `NoiseAwareFusionGate`, and instantiates `model` and `fusion_model`.
5. Skip the dataset-loading, training, evaluation, and optional YOLO cells.
6. Run the final code cell titled **“THERMAL FUSION STUDIO — INTERACTIVE COLAB APP”**. It loads the checkpoint and starts Gradio. Open the temporary link printed below the cell.
7. Upload a matching visible-light image and thermal image, process them, inspect the denoised/fused output, and optionally download the generated result package.

The checkpoint must contain both `model_state_dict` and `fusion_model_state_dict` from the same v5 architecture. If the checkpoint is unavailable, the app cannot reproduce the trained model's output. Do not replace the trained checkpoint with randomly initialized model weights.

### Sharing the live app

The Gradio share link is temporary and works only while the Colab runtime and app remain active. For a professor to open your existing live link, you must share the currently running link with them. For independent reproduction, they need their own Colab runtime and access to the dataset/checkpoint as applicable.

## Train and evaluate the model

To rerun the complete experiment rather than just demo the app:

1. Obtain the MSRS dataset and place/extract it under `MyDrive/Research_Project/`, preserving paired `train/vi` and `train/ir` directories (and a test split if available).
2. Open `LightThermNet.ipynb` in Colab and run notebook cells in order.
3. Confirm the printed dataset root and matching image-pair count before training.
4. Review the final evaluation output and saved plots/checkpoint. Training from scratch can take time and may produce results different from a previously recorded run.

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

These results describe the recorded denoising evaluation and the noise-generation/evaluation setup used in that run. Reproduce the corresponding evaluation before treating the values as newly verified results. They do not establish improved object-detection accuracy or independently validate fusion quality.

## Environment and dependencies

The notebook uses Python with PyTorch, NumPy, OpenCV, Matplotlib, tqdm, Pillow, and Gradio. For Colab, install missing packages in the notebook runtime if prompted. `requirements.txt` is provided as a starting point; dependency versions should be pinned after a clean environment test.

## Limitations and future work

- Evaluate on real thermal-camera noise and on additional datasets.
- Compare denoising against established baselines.
- Complete independent quantitative evaluation of the fusion component.
- Test whether enhancement improves a downstream detection or segmentation system before making detection-performance claims.
- Verify runtime and memory use on the intended deployment hardware.

## Dataset and attribution

The project uses the MSRS (Multi-Spectral Road Scenarios) dataset. Consult the dataset's original documentation for citation, access, and usage terms. The dataset itself is not redistributed here.
