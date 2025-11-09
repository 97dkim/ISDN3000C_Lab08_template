# ISDN3000C Lab08 — Food-101 Baseline

Short README for the Lab08 workspace. This repo contains Food-101 data (subset or full), notebooks, and pretrained model artifacts used for the final challenge.

Contents
- `Lab08 (1).ipynb` — main notebook (analysis / training experiments).
- `data/food-101/` — dataset (images, meta, splits).

Quick setup (Windows PowerShell)

1. Create and activate a virtual environment (PowerShell):

```powershell
python -m venv .venv; .\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

2. Open `Lab08 (1).ipynb` in Jupyter / VS Code and run the cells. The notebook expects the dataset at `data/food-101/`.

Baseline training notes
- Baseline approach: Transfer learning with a pretrained ResNet18 backbone (ImageNet weights). Replace final classifier with a small head and train in two stages (linear probe, then fine-tune last block).
- Input size: 224×224, ImageNet normalization.
- Recommended training recipe (high level):
  1. Freeze backbone, train head for ~5–10 epochs (LR ~1e-3).
  2. Unfreeze last block and train with a lower LR (~1e-4) for ~10–20 epochs.

Augmentations (recommended)
- RandomResizedCrop(224, scale=(0.8,1.0))
- RandomHorizontalFlip(p=0.5)
- RandomRotation(degrees=15)
- ColorJitter(brightness=0.2, contrast=0.2, saturation=0.2, hue=0.02)
- RandomAffine(degrees=0, translate=(0.05,0.05), scale=(0.95,1.05), shear=2)
- RandomErasing(p=0.25) (optional)

Evaluation
- Use deterministic preprocessing for validation/testing: Resize(256) -> CenterCrop(224) -> ToTensor() -> Normalize(ImageNet mean/std).
- Keep test/validation sets untouched by training augmentations.

