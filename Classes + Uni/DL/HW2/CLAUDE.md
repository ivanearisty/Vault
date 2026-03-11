# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Deep Learning HW2 (CS-GY 6953, Spring 2026). Modern CNNs and Object Detection.
**Environment: Google Colab with CUDA GPU** (Mac locally, no CUDA — run notebook on Colab).
Total GPU time budget: ≤ 90 minutes across both problems.

## Homework Structure

| Problem | Topic | Points | Type |
|---------|-------|--------|------|
| 1 | Convolution Filter Design and Analysis | 20 | Theory |
| 2 | IoU and Generalized IoU | 10 | Theory |
| 3 | ConvNeXt Ablation Study (PatchCamelyon) | 25 | Coding (PyTorch, Colab GPU) |
| 4 | FCOS vs RetinaNet on NWPU VHR-10 | 25 | Coding (PyTorch, Colab GPU) |

## Directory Structure

```
HW2/
├── Theoretical/
│   ├── Problem 1.md    # Convolution filters: edge detection, blur, sharpening, noise
│   └── Problem 2.md    # IoU non-differentiability, GIoU analysis
├── HW2 Starter.ipynb   # Colab notebook for Problems 3 & 4
├── Homework 2.pdf       # Assignment PDF
└── CLAUDE.md
```

## Problem Details

### Problem 1: Convolution Filters (20 pts, Theory)
- (a) Design horizontal + vertical 3×3 edge detection filters, test on 5×5 image (6 pts)
- (b) Compare box blur vs weighted blur, edge preservation (4 pts)
- (c) Derive sharpening filter via unsharp masking with α=1, analyze large α (6 pts)
- (d) Linear filter vs median filter for noise reduction (4 pts)

### Problem 2: IoU and GIoU (10 pts, Theory)
- (a) Explicit IoU formula, identify non-differentiable operations (5 pts)
- (b) GIoU: why useful gradients when no overlap, range, negative values (5 pts)

### Problem 3: ConvNeXt Ablation (25 pts, Colab GPU)
Fine-tune ConvNeXt-Tiny on PatchCamelyon (binary histopathology). 5 ablations:
1. LayerNorm → BatchNorm2d
2. GELU → ReLU
3. 7×7 depthwise conv → 3×3 standard conv
4. Inverted bottleneck → standard bottleneck
5. Remove stochastic depth

Config: AdamW, lr=2e-4, weight_decay=0.05, 3 epochs, 20% subset, batch_size=64

### Problem 4: FCOS vs RetinaNet (25 pts, Colab GPU)
Fine-tune both on NWPU VHR-10 aerial imagery, 5 epochs. Fill 3 gaps:
1. Optimizer with differential LR (backbone 1e-4, head 1e-2)
2. Loss forward pass (sum loss dict)
3. Post-processing (score thresh 0.3, NMS IoU 0.5)

## Per-Student Seed

```python
import hashlib
seed = int(hashlib.sha256(b"YOUR_NETID_HERE").hexdigest(), 16) % 10000
```

## User's Math/LaTeX Style (Obsidian)

For theory solutions, use this style:
- Display math: `$$...$$`
- Multi-line derivations: `\begin{gather}...\end{gather}` wrapped in `$$`
- Line breaks: `\\` (single) or `\\ \\` (paragraph break)
- Headers: `## Problem X` → `### (i)` → subsections
- Matrices: `\begin{pmatrix}...\end{pmatrix}`
- Final answers: `\boxed{result}`
- Bold for conclusions: `**linearly independent**`

## Running

This homework requires CUDA. Run the notebook on Google Colab with a GPU runtime.

```bash
# Local dependencies (for editing only)
pip install torch torchvision torchgeo pycocotools numpy matplotlib pandas
```

## Key Libraries

- `torchvision.models.convnext_tiny` — Problem 3 base model
- `torchvision.models.detection.fcos_resnet50_fpn` — Problem 4 anchor-free detector
- `torchvision.models.detection.retinanet_resnet50_fpn` — Problem 4 anchor-based detector
- `torchvision.transforms.v2` + `tv_tensors` — Problem 4 detection augmentation
- `torchgeo.datasets.VHR10` — Problem 4 dataset
