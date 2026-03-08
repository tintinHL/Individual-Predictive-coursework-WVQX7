# DeepFashion Clothing Category Classification

**Module:** MSIN0097 Predictive Analytics — Individual Coursework 2025–26
Candidate number: WVQX7

## Project Overview

End-to-end predictive system for 32-class clothing category classification using the DeepFashion Category and Attribute Prediction Benchmark. The project compares four generations of computer vision approaches (HOG+SVM → MLP → ResNet18 → ViT) under data-limited conditions (10,000 images).

**Champion model:** ResNet18 with Optuna-tuned hyperparameters — 45.9% accuracy, 0.404 Weighted F1.

## Repository Structure

```
├── deepfashion_subset_builder.ipynb   # Main notebook (all steps 1-6)
├── final_subset_representative.csv    # 10,000-image subset metadata
├── final_subset.csv                   # Initial subset (alphabetical)
├── requirements.txt                   # Python dependencies
├── README.md                          # This file
│
├── Anno_coarse/                       # DeepFashion annotation files
│   ├── list_attr_cloth.txt
│   ├── list_attr_img.txt
│   ├── list_category_cloth.txt
│   └── list_category_img.txt
│
├── Eval/
│   └── list_eval_partition.txt        # Train/val/test partition
│
├── data/
│   ├── subset_images_representative/  # 10,000 sample images
│   ├── hog_features/                  # Cached HOG/PCA features (.npy)
│   │   ├── X_train_hog.npy
│   │   ├── X_train_pca.npy
│   │   ├── y_train.npy
│   │   └── ...
│   └── subset_images_unbalanced_alphabetical/  # Initial subset (archived)
│
├── models/                            # Pre-trained checkpoints
│   ├── champion_resnet18.pt           # Optuna-tuned ResNet18
│   └── vit_b16.pt                     # ViT-B/16
│
├── img.zip                            # Full DeepFashion images (compressed)
│
├── interaction_log.md                 # Agent interaction log (1 March)
├── session_log_2026-03-02_00-56-17.md # Detailed session log
└── interaction_log_2026-03-08.md      # Agent interaction log (8 March)
```

## Setup & Installation

### Prerequisites
- Python 3.11+
- NVIDIA GPU with CUDA support (tested on RTX 3070, 8GB VRAM)
- ~4GB disk space for images and cached features

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Dataset Access

The DeepFashion Category and Attribute Prediction Benchmark is available at:
http://mmlab.ie.cuhk.edu.hk/projects/DeepFashion.html

The annotation files (`Anno_coarse/`, `Eval/`) and a representative 10,000-image subset are included in this repository. The full `img.zip` (2.6GB) contains all source images.

## How to Run

### Full Pipeline (requires GPU, ~2 hours)

1. Open `deepfashion_subset_builder.ipynb` in Jupyter or VS Code
2. Run all cells sequentially from top to bottom
3. The notebook follows Steps 1–6 as marked by section headings

### Quick Reproduce (without retraining)

Pre-trained model checkpoints are saved in `models/`. To skip training and run evaluation only:

1. Run cells up to and including Step 3 (data loading and splitting)
2. Skip Steps 4.1–4.7 (model training cells)
3. Load checkpoints manually:
   ```python
   champion = models.resnet18(pretrained=False)
   champion.fc = nn.Linear(512, 32)
   champion.load_state_dict(torch.load('models/champion_resnet18.pt'))
   champion.to(device)
   ```
4. Run Step 5 evaluation cells onwards

### Hardware Notes

- **GPU:** Tested on NVIDIA RTX 3070 (8GB VRAM). Batch sizes are set conservatively (32 for ResNet, 16 for ViT) to fit within VRAM.
- **RAM:** 16GB minimum. Close other applications before running Steps 4–5. DataLoaders use `num_workers=0` and `pin_memory=False` to avoid memory spikes.
- **CPU-only:** The notebook can run on CPU by changing `device = torch.device('cpu')`, but training will be significantly slower.

## Key Results

| Model | Accuracy | Macro F1 | Weighted F1 | Time |
|-------|----------|----------|-------------|------|
| SVM (Baseline) | 29.7% | 0.095 | 0.273 | ~43s |
| MLP | 35.3% | 0.123 | 0.330 | ~30s |
| ResNet18 (Champion) | 45.9% | 0.146 | 0.404 | ~8min |
| ViT-B/16 | 30.5% | 0.038 | 0.181 | ~15min |

## Agent Tooling

Two agent tools were used following a plan → delegate → verify → revise workflow:

- **Codex (VS Code):** Code scaffolding, pipeline generation, model architecture templates
- **Gemini (https://gemini.google.com/):** Bug fixing, structural improvements, report generation

Full interaction logs are available in the repository root. The decision register documenting accepted/modified/rejected agent contributions is in the report appendix.

## References

- Liu, Z. et al. (2016) DeepFashion: Powering Robust Clothes Recognition and Retrieval with Rich Annotations. CVPR.
- He, K. et al. (2016) Deep Residual Learning for Image Recognition. CVPR.
- Dosovitskiy, A. et al. (2021) An Image is Worth 16x16 Words. ICLR.
- Akiba, T. et al. (2019) Optuna: A Next-generation Hyperparameter Optimization Framework. KDD.
