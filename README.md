# Liver and Tumor Segmentation from CT Scans

A two-stage deep learning pipeline for liver and tumor segmentation from abdominal CT scans, comparing DenseUNet169 and EfficientNet-based U-Net architectures.

## Overview

This project segments the liver and liver tumors from CT scan slices in two stages — liver region segmentation followed by tumor segmentation within the liver — using encoder-decoder architectures built on pretrained CNN backbones.

## Datasets

- **[3D-IRCADb-01](https://www.ircad.fr/research/data-sets/liver-segmentation-3d-ircadb-01/)** — 20-patient 3D CT dataset with liver and tumor annotations
- **[LiTS (Liver Tumor Segmentation)](https://academictorrents.com/details/27772adef6f563a1ecc0ae19a528b956e6c803ce)** — larger-scale liver/tumor CT dataset used for extended training

## Models

| Model | Architecture Summary | Notebook |
|---|---|---|
| DenseUNet169 | U-Net-style decoder built on a DenseNet169 encoder pretrained on ImageNet, trained with a combined Dice + weighted cross-entropy loss; multiple training configurations were explored (class weighting, learning rate, deep supervision) | [`notebooks/denseunet169_ircadb.ipynb`](./notebooks/denseunet169_ircadb.ipynb), [`notebooks/denseunet169_lits.ipynb`](./notebooks/denseunet169_lits.ipynb) |
| EfficientNet-based U-Net | U-Net decoder built on an EfficientNet-B0 encoder (via `segmentation_models_pytorch`), pretrained on ImageNet and fine-tuned with combined Dice + cross-entropy loss, with a small hyperparameter search over learning rate and loss weighting | [`notebooks/efficientnet_unet.ipynb`](./notebooks/efficientnet_unet.ipynb) |

A dedicated preprocessing notebook handles DICOM extraction, volume loading, and conversion to model-ready 2D slices/masks: [`notebooks/preprocessing.ipynb`](./notebooks/preprocessing.ipynb).

## Libraries & Tools

- **PyTorch / torchvision** — model definition and training
- **segmentation-models-pytorch** — EfficientNet-based U-Net implementation
- **Albumentations** — data augmentation
- **pydicom** — reading and inspecting medical DICOM files
- **scikit-image, trimesh, plotly** — 3D reconstruction and visualization
- **OpenCV, Matplotlib, NumPy, tqdm** — general image processing and utilities

## Results

Test-set Dice scores (best configuration per model):

| Model | Dataset | Liver Dice | Tumor Dice |
|---|---|---|---|
| DenseUNet169 | 3D-IRCADb-01 | 83.9% | 17.7% |
| DenseUNet169 | LiTS | 91.3% | 51.0% |
| EfficientNet-based U-Net | 3D-IRCADb-01 | 88.6% | 52.2% |

## Paper

This work has been accepted for publication (ACM Digital Library): [https://doi.org/10.1145/3812734.3813722](https://doi.org/10.1145/3812734.3813722)

## Repository structure

```
.
├── notebooks/
│   ├── preprocessing.ipynb
│   ├── denseunet169_ircadb.ipynb
│   ├── denseunet169_lits.ipynb
│   └── efficientnet_unet.ipynb
└── README.md
```

## Getting started

1. Clone the repo:
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   cd <repo-name>
   ```
2. Download the datasets ([3D-IRCADb-01](https://www.ircad.fr/research/data-sets/liver-segmentation-3d-ircadb-01/) and/or [LiTS](https://academictorrents.com/details/27772adef6f563a1ecc0ae19a528b956e6c803ce)) and update the data paths at the top of each notebook.
3. Run `preprocessing.ipynb` first to prepare 2D slices and masks, then run any of the model notebooks.

## Team

- Malik Jallal
- Lujain Yahya Toma
- Areen Al-Akaleek
- Motasem Alwedyan
- Abdullah Al-Amaren
