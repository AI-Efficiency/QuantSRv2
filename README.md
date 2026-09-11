# QuantSR+: Pushing the Limit of Quantized Image Super-Resolution Networks

**IEEE TPAMI · 2026 · Early Access**

Haotong Qin, Xudong Ma, Xianglong Liu, Jie Luo, Jinyang Guo, Michele Magno, Yulun Zhang

[Published paper](https://doi.org/10.1109/TPAMI.2026.3697683) | [arXiv](https://arxiv.org/abs/2605.22351) | [Citation](#citation)

**QuantSR+ improves low-bit image super-resolution through Redistribution-driven Bit Determination (RBD), Quantized Slimmable Architecture (QSA), and Slimming-guided Function-localized Distillation (SFD).** This repository is named `QuantSRv2`; the paper and method name is **QuantSR+**. It extends [QuantSR](https://github.com/htqin/QuantSR) with operator, architecture, and distillation designs.

## Published results and scope

Selected SwinIR-S results at ×4, W2A2, from Table 2 of the linked manuscript. PSNR is in dB; higher is better. The QuantSR+/ODM comparison uses the same 300K training iterations described in Section 4.1.

| Method | Set5 PSNR | Urban100 PSNR |
| --- | --- | --- |
| 2DQuant | 29.53 | 23.84 |
| QuantSR | 31.53 | 25.26 |
| ODM | 31.67 | 25.36 |
| QuantSR+ | 31.70 | 25.65 |

QuantSR+ improves Urban100 PSNR by **0.29 dB over ODM** under this setting. This comparison does not imply that every metric or dataset improves by the same amount.

For SRResNet ×4 with 16 residual blocks, Table 4 reports **89.4% storage reduction and 87.9% theoretical operation reduction** at W2A2 with FP32 head/tail, using a 3×256×256 input for complexity accounting. The separate hardware experiment uses ZCU104 (ZU7EV FPGA), a 3×64×64 LR input, and a 3×256×256 HR output. The FP32-head/tail path reports 2.13 s / 0.47 FPS; the fully integer path with INT8 head/tail reports 24 ms / 41.7 FPS. These deployment paths, precisions, and input sizes must not be conflated with the theoretical reduction ratios.

### What this paper supports

- RBD improves representation in forward and backward quantized operators (Section 3.2; Table 5).
- QSA learns from a larger architecture and supports slimming under efficiency constraints (Section 3.3; Table 5).
- SFD aligns block-level functions during distillation to improve optimization (Section 3.4; Table 5).
- Accuracy gains are demonstrated on SRResNet and SwinIR-S; diffusion-based StableSR is evaluated separately with perceptual metrics (Tables 1–3).
- Actual acceleration depends on the complete deployment path, including head/tail precision and PS/PL placement (Table 4 and its caption).

## Implementation

![QuantSR+ framework](./imgs/overview.png)

This is quantization-aware training with SR data and distillation, rather than data-free post-training quantization. The commands below are the repository's supplied 4-bit evaluation entry points; set dataset and checkpoint paths in the YAML files before running. Published results above have not been rerun in this documentation update.

## Dependencies

```bash
# Go to the default directory
pip install -r requirements.txt
python setup.py develop
```

## Execution

```bash
# We provide script to test our 4-bit QuantSR+(Conv) and QuantSR+(Transformer)
python3 basicsr/test.py -opt options/test/test_QuantSR_plus_C_4bit_x4.yml
python3 basicsr/test.py -opt options/test/test_QuantSR_plus_T_4bit_x4.yml
```

## Citation

Please cite the published paper below. Open paper versions are linked at the top of this README.

```bibtex
@article{qin2026quantsrplus,
  title = {{QuantSR+}: Pushing the Limit of Quantized Image Super-Resolution Networks},
  author = {Haotong Qin and Xudong Ma and Xianglong Liu and Jie Luo and Jinyang Guo and Michele Magno and Yulun Zhang},
  journal = {IEEE Transactions on Pattern Analysis and Machine Intelligence},
  year = {2026},
  doi = {10.1109/TPAMI.2026.3697683},
  url = {https://doi.org/10.1109/TPAMI.2026.3697683}
}
```
