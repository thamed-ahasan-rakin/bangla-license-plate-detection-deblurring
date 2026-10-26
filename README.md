# Bangla License Plate Detection and Deblurring

A research portfolio project exploring deep-learning methods for locating Bangla vehicle license plates and restoring plate crops affected by blur.

> **Reproducibility note:** This repository is a cleaned research-code starter. The notebooks need local dataset paths and compatible PyTorch accelerator builds before they can run. Datasets and trained weights are intentionally not included.

## Research overview

The project investigates two complementary tasks:

1. **Oriented license-plate localization:** YOLOv8m-OBB is trained to locate tilted or rotated license plates using four-corner oriented bounding-box annotations.
2. **Image restoration:** a modified DeblurGAN-v2-style model uses Line-Curve blocks that combine horizontal and vertical strip convolutions with deformable convolution. The training objective includes pixel, edge, perceptual, and adversarial terms.

A data-preparation notebook rectifies OBB crops and generates clean, motion-blurred, and defocus-blurred plate images. The notebooks are organized by workflow stage; use them as separate experiments unless you have also prepared and tested an end-to-end inference script.

## Key Research Contributions

This project investigates a multi-stage deep-learning framework for detecting and restoring distorted Bangla vehicle license plates.

### 1. Self-Developed Dataset
We independently collected and prepared a dataset for our research on Bangla license plate detection and image restoration. The data preparation workflow includes plate annotation and preprocessing for detection and deblurring experiments.

Dataset link : https://app.roboflow.com/thamed-ahasan-rakin/bengali-vehicle-images-made-for-yolov8m-obb/browse?queryText=&pageSize=100&startingIndex=0&browseQuery=true

### 2. Oriented License Plate Detection
We trained and evaluated a YOLOv8m-OBB model to localize Bangla license plates using oriented bounding-box annotations. This approach is designed to support plate localization when plates appear tilted or rotated, enabling more appropriate cropping for subsequent restoration.

### 3. Modified DeblurGAN Architecture
We developed a modified DeblurGAN-style architecture incorporating Line-Curve blocks that combine horizontal and vertical strip convolutions with deformable convolution. The restoration framework uses pixel, edge, perceptual, and adversarial loss components to investigate the restoration of blurred license plate images.

### 4. Multi-Stage Research Workflow
The project explores complementary detection and restoration stages, from oriented license plate localization to deblurring. The notebooks document the individual experimental workflows; fully integrated inference and performance claims are included only where implemented and evaluated.

### Reproducibility and Evaluation
Evaluation results, dataset statistics, hardware details, and experimental configurations will be reported based on verified runs. Dataset access is subject to applicable privacy, licensing, and research permissions.

## Repository structure

```text
.
├── README.md
├── requirements-common.txt
├── .gitignore
├── notebooks/
│   ├── 01_prepare_plate_dataset.ipynb
│   ├── 02_train_yolov8m_obb.ipynb
│   └── 03_train_modified_deblurgan.ipynb
└── results/
    └── qualitative/   # Add a few approved before/after examples here
```

## Notebooks

### 1. `01_train_yolov8m_obb.ipynb`

- Checks image/label pairs and annotation validity.
- Creates reproducible train/validation/test splits (80/10/10 by default).
- Trains YOLOv8m-OBB and evaluates the held-out test split.
- Reports precision, recall, mAP@0.5, and mAP@0.5:0.95.

### 2. `02_prepare_plate_dataset.ipynb`

- Generates aligned clean, motion-blurred, and defocus-blurred image sets.
- Uses PyTorch XPU for the Intel GPU path described in the notebook.

 

### 3. `03_train_modified_deblurgan.ipynb`

- Trains a generator with Line-Curve blocks for plate-image restoration.
- Uses L1 pixel loss, Sobel edge loss, VGG19 perceptual loss, and adversarial loss.
- Evaluates restoration with PSNR, SSIM, and MAE.
- The notebook currently selects `torch.device("cuda")`; it therefore expects a compatible CUDA-enabled PyTorch setup unless you deliberately adapt and validate the accelerator code.


## Environment and setup

Python 3.11 is the version described in the project notebooks. Create separate virtual environments if you need both Intel XPU training and NVIDIA CUDA deblurring. Do **not** install XPU-only and CUDA-only PyTorch builds into the same environment.

1. Install Python and Git, then clone this repository.
2. Create/activate a virtual environment.
3. Install the common packages:

   ```bash
   pip install -r requirements-common.txt
   ```

4. Install the PyTorch build that matches the accelerator for the notebook you want to run. Follow the current official PyTorch installation instructions for Intel XPU or NVIDIA CUDA. The first two notebooks target Intel XPU; the deblurring notebook currently targets CUDA.
5. In the detection environment, install Ultralytics after installing the correct PyTorch build: `pip install ultralytics`.
6. Open the repository folder in VS Code, install the Jupyter extension, and select the same virtual environment as the notebook kernel.
7. Edit the dataset/project paths in the configuration cells. The path values in these cleaned copies are placeholders.
8. Make sure you have the dataset in the expected layout before running any training cell. The raw dataset is not included in this repository.

The common packages are listed in `requirements-common.txt`. Accelerator-specific PyTorch installation is intentionally kept separate to avoid accidentally installing the wrong build.

## Data and trained weights

Dataset images, generated blur sets, training checkpoints (`.pt` / `.pth`), and machine-specific outputs are excluded from Git. Follow the dataset's own license and privacy restrictions. Do not publish real license-plate images unless you have permission and a lawful basis to share them; use synthetic, blurred, or otherwise approved examples for the public demo.

## Results

Add only results produced by your actual final run. Recommended reporting:

- **Detection:** precision, recall, mAP@0.5, mAP@0.5:0.95 on the held-out test set.
- **Deblurring:** PSNR, SSIM, and MAE on a clearly identified validation/test split.
- **Qualitative comparison:** a small approved panel showing blurred input, restored output, and clean reference.

Include the hardware, data split, and checkpoint/configuration used for each reported result. Do not report validation performance as test performance, and do not insert figures or metric values until you verify the final run.

## Limitations

- Dataset paths and local data are user-configured and are not bundled here.
- The notebooks target different accelerator backends (Intel XPU for preparation/detection; CUDA for the current deblurring code).
- Deblurring can introduce visual details that are not guaranteed to match the original plate. Restored images should not be treated as verified identity evidence without human review.
- This repository presents the implemented notebook workflows; describe OCR or end-to-end recognition as implemented only if you add and evaluate that code.

## Credits and research integrity

This is based on a team research project. Before making the repository public, confirm with your teammates and supervisor that the code and any results are approved for release. Add the agreed author list, each person's actual contributions, dataset acknowledgements, and relevant citations here. Do not publish student IDs, private data, or the full thesis PDF unless everyone with the relevant rights has approved it.
