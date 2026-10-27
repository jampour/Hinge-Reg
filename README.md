# Hinge-Reg

**Loss-Level Structural Regularization for Vision Transformer-Based Handwritten Text Recognition**


This repository currently serves as the canonical reference for the paper and will be updated with the full implementation (loss module and reference backbone) once the review process is complete.

If you need access to the code before acceptance (e.g., for review or reproducibility checks), please contact the authors.


## What is HingeReg?

HingeReg is a modular, architecture-agnostic loss-level regularization framework for ViT-based handwritten text recognition (HTR). It augments the CTC objective with three structural terms, all of which add zero inference cost and no learnable parameters at inference:

- **Perceptual Term** — a hinge-squared penalty on attention to distant patches in early layers.
- **Relational Term** — horizontal smoothness on late-layer representations via a one-dimensional Gaussian target.
- **Feedback Term** — Barlow-Twins-style cross-layer consistency with stop-gradient on the late branch.

The framework requires only quantities already computed during the forward pass (attention maps, per-layer features, patch coordinates, and an optional padding mask), and can therefore be applied to any ViT backbone without architectural modification.

---

## What the Paper Asks

Rather than proposing another architectural prior or a single locality loss, this work asks:

> *Why, when, and under what conditions does loss-level structural regularization work for ViT-based HTR?*

The evidence points to three findings:

1. **Which term matters.** The Perceptual Term accounts for the majority of the gain; the Relational and Feedback Terms are subordinate in isolation but contribute a small further improvement in combination.
2. **When terms must be introduced.** Under the tested optimization setting, curriculum scheduling is a *necessary* condition: without a short warm-up, the same terms degrade performance on every seed and both datasets.
3. **On which backbones it helps.** The effect is modulated by the backbone's built-in locality prior. On a plain ViT the gain is large and consistent; on HTR-VT, whose ResNet-18 stem already encodes locality, the effect is small relative to seed-to-seed variability.

---

## Results (Summary)

### Main results — plain ViT, 10 seeds

| Dataset | Method | CER (%) | WER (%) |
|---|---|---|---|
| MPHD (Persian) | CTC-only | 41.67 ± 3.56 | 93.54 ± 3.19 |
| MPHD (Persian) | **HingeReg** | **30.89 ± 3.37** | **79.94 ± 4.87** |
| READ2016 (Early Modern German) | CTC-only | 15.93 ± 2.30 | 58.15 ± 5.14 |
| READ2016 (Early Modern German) | **HingeReg** | **14.58 ± 1.48** | **55.04 ± 3.41** |


### Ablation (3 seeds per configuration)

- **Perceptual Term alone:** ~90% of the full gain.
- **Relational and Feedback Terms alone:** neutral or slightly harmful.
- **Full configuration without curriculum:** *worse than CTC-only* on every seed and both datasets.

Full tables and ablations are in the paper.

---

## Planned Release

The repository will include:

- `losses/` — the complete HingeReg loss module (Perceptual, Relational, Feedback), architecture-agnostic.
- `models/` — the reference plain-ViT backbone used in the paper.
- `configs/` — configuration files for the two paper settings (plain ViT and HTR-VT).
- `checkpoints/` — links to trained checkpoints for the canonical seeds.

The loss module will be usable as a drop-in component for any ViT backbone that exposes attention maps and per-layer features.

---

## Citation

```bibtex
@article{jampour2026hingereg,
  title   = {HingeReg: Loss-Level Structural Regularization for Vision Transformer-Based Handwritten Text Recognition},
  author  = {Jampour, Mahdi and Fedeli, Alba},
  journal = {under review},
  year    = {2026}
}
