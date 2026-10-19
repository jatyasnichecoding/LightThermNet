# LightThermNet

### Lightweight CNN-Based Thermal Image Denoising and Noise-Aware Visible–Infrared Fusion

LightThermNet explores a lightweight residual CNN for thermal-image denoising and a separate noise-aware gate for combining a denoised thermal image with a paired visible-light image. The current implementation, training pipeline, evaluation, checkpoint loading, and interactive Gradio prototype are contained in `LightThermNet.ipynb`.

[**Open the notebook in Google Colab**](https://colab.research.google.com/github/jatyasnichedcoding/LightThermNet/blob/main/LightThermNet.ipynb)

## Problem

Thermal images can contain noise that obscures heat patterns and scene structure. This project tests whether a compact CNN can remove synthetic thermal noise while retaining useful image information, then explores visible–infrared fusion for a more informative display.

## Pipeline

1. Pair visible (`vi`) and infrared (`ir`) images by filename.
2. Convert the paired images to grayscale and resize them to 160 × 160 pixels.
3. Generate controlled synthetic corruption using stripe-like and Gaussian noise.
4. Train a residual CNN to estimate the corruption; the denoised image is computed as `noisy image − predicted noise`.
5. Train a separate noise-aware fusion gate that blends visible and contrast-enhanced thermal information based on an estimated noise map.
6. Evaluate denoising quality against clean thermal references and launch the interactive **Thermal Fusion Studio** Gradio interface.

**Fusion evaluation note:** MSRS does not provide ground-truth fused images in this setup. The fusion gate is trained against a designed proxy target, so the reported denoising metrics should not be presented as a quantitative evaluation of fusion quality.

## Dataset

The notebook uses the MSRS (Multi-Spectral Road Scenarios) dataset. The data is not included in this repository. Place the extracted dataset somewhere below this Google Drive folder:

```text
MyDrive/Research_Project/
```

The notebook searches beneath that directory for paired training folders:

```text
<dataset-root>/train/vi/
<dataset-root>/train/ir/
```

If available, a separate test split should have:

```text
<dataset-root>/test/vi/
<dataset-root>/test/ir/
```

Visible and infrared files are paired by matching filename. Please follow the dataset's own terms and citation requirements.

## Run in Google Colab

1. Open the notebook using the link above.
2. In **Runtime → Change runtime type**, select a GPU if one is available. The notebook falls back to CPU if CUDA is unavailable.
3. Make sure MSRS is extracted somewhere below `MyDrive/Research_Project/` in Google Drive.
4. Run the notebook cells from top to bottom. The notebook mounts Google Drive, finds paired data, trains the models, evaluates denoising, saves the best checkpoint and plots, and then launches the Gradio prototype.
5. When the last cell starts, open the temporary Gradio link and upload a matching visible-light and thermal image pair.

The notebook saves checkpoints and results under `MyDrive/Research_Project/`, including `Checkpoints/best_checkpoint_v5.pth`, `final_qualitative_comparison_v5.png`, `training_loss_v5.png`, and `validation_psnr_v5.png`.

## Recorded v5 experiment

The uploaded notebook contains the following recorded test-evaluation output:

| Measure | Recorded value |
|---|---:|
| Paired training images found | 1,083 |
| Training / validation split | 975 / 108 images |
| Separate test split | 361 image pairs |
| Input size | 160 × 160 |
| Epochs | 8 |
| Batch size | 16 |
| Noisy-input PSNR | 25.313 dB |
| Denoised-output PSNR | 33.757 dB |
| PSNR change | +8.444 dB |
| Combined model parameters | 31,138 |
| Recorded model-only latency | 42.326 ms/frame |
| Recorded model-only throughput | 23.6 frames/second |

The saved notebook output identifies that run as **CPU execution** (`torch` reported a CPU build/device). The latency is model-only, excludes image file I/O and other application overhead, and can change with hardware and runtime conditions.

The denoising evaluation uses synthetic corruption generated from the clean thermal image. Therefore, the reported improvement does not by itself establish performance on real thermal-camera noise, improved object-detection accuracy, or improved fusion quality.

## Dependencies

The `requirements.txt` file lists the Python packages used by the notebook and its Gradio interface. Google Colab supplies the `google.colab` integration used to mount Drive; running this notebook unchanged outside Colab requires adapting the Drive-mounting code and data paths.

## Repository contents

```text
LightThermNet/
├── LightThermNet.ipynb  # Training, evaluation, inference helpers, Gradio prototype
├── requirements.txt     # Notebook and prototype dependencies
├── results/
│   └── README.md        # Results folder notes
├── README.md
└── .gitignore
```

## Limitations and future work

- Test on real sensor noise, in addition to simulated noise.
- Compare denoising against appropriate baseline methods.
- Evaluate fusion with suitable no-reference or task-specific measures and human inspection.
- Test whether enhancement benefits a separately trained thermal-object detector.
- Benchmark the full application on the intended deployment hardware.
