# Swin Transformer

An reimplementation of the **Swin Transformer**, trained on **ImageNet-100** for image classification.

## The Paper

**Swin Transformer: Hierarchical Vision Transformer using Shifted Windows** (Liu et al., ICCV 2021)

This paper introduces a vision backbone that builds a **hierarchical** feature representation while keeping the Transformer's self-attention mechanism, by restricting attention to small local **windows** and shifting those windows between consecutive blocks.

![Swin Transformer architecture](https://raw.githubusercontent.com/microsoft/Swin-Transformer/main/figures/teaser.png)
*Image from the official [microsoft/Swin-Transformer](https://github.com/microsoft/Swin-Transformer) repository (Liu et al., 2021).*

**Improvements over the classic ViT :**
- **Hierarchical features instead of a single scale** : patches are progressively merged stage by stage, producing multi-scale feature maps usable for detection/segmentation, unlike ViT's fixed single-resolution output.
- **Local windowed attention instead of global attention** : self-attention is computed only within small windows (likes 7×7 patches), reducing complexity from quadratic to linear in image size, instead of ViT's full global attention over every patch.
- **Shifted windows** : every other block shifts the window grid, so information can still flow across window boundaries despite attention being local.
- **Relative position bias** : added directly inside attention, rather than ViT's absolute position embeddings added once at the input.

## Dataset I used : ImageNet-100

[ImageNet-100](https://www.kaggle.com/datasets/ambityga/imagenet100) is a 100-class subset of ImageNet-1K, with about 1,300 training images per class. 

I couldn't use ImageNet-1K due to a lack of computing resources.

It's downloaded via `kaggle`. The provided validation folder is split 90/10 into a validation set and a held-out test set. 

Images are resized to 224×224 and normalized with the standard ImageNet mean/std

Training images are additionally augmented with random horizontal flips and color jitter.

## Training Setup

| Hyperparameter | Value |
|---|---|
| Image size / patch size | 224×224 / 4 |
| Embedding dim (C) | 96 |
| Stage depths | [2, 2, 6, 2] |
| Attention heads per stage | [3, 6, 12, 24] |
| Window size | 7 |
| MLP ratio | 4 |
| Optimizer | AdamW |
| Learning rate | 5e-4 |
| Weight decay | 0.05 |
| LR schedule | 5-epoch linear warmup → cosine decay |
| Loss | Cross-entropy, label smoothing 0.1 |
| Batch size | 256 |
| Epochs (max) | 75, early stopping (patience 10) |

## Result

Due to limited computational resources, it was not feasible to conduct a hyperparameter search, compare alternative optimizers, or run architectural ablations 

My result :
**~70% accuracy on the held-out test set.**
