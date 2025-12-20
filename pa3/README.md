# Advanced Model Compression: Pruning, Quantization, and Knowledge Distillation

This repository explores various neural network compression methodologies aimed at optimizing deep learning models for deployment on resource-constrained hardware. Using the VGG architecture on CIFAR-10 and CIFAR-100 datasets, this study evaluates the trade-offs between model size, computational efficiency, and classification accuracy.

---

## Assignment Overview

The rapid growth of neural network complexity poses significant challenges for deployment on edge devices and smartphones due to high storage and computational overhead. For example, a standard VGG-11 network requires nearly 500 megabytes of storage and millions of parameters. This project implements and analyzes three primary families of compression:

- **Pruning:** Magnitude-based weight removal (unstructured) and channel-level filter removal (structured).

- **Quantization:** Reducing numerical precision from FP32 to bit-widths as low as INT4 using Post-Training Quantization (PTQ) and Quantization-Aware Training (QAT).

- **Knowledge Distillation (KD):** Transferring knowledge from a high-capacity "teacher" model (VGG-16/19) to a compact "student" model (VGG-11).

---

## Technical Analysis and Findings

### 1. Network Pruning

The experiments compared unstructured and structured pruning at matched compression ratios to understand their impact on hardware and accuracy.

**Key Findings:**

- **Sensitivity:** Early convolutional layers (e.g., `features.0`) were found to be highly sensitive to pruning, while deeper layers remained robust even at high sparsity.

- **Regularization:** Unstructured pruning acted as a regularizer, with accuracy recovering to 89.3% after fine-tuning—surpassing the 85.1% baseline.

- **Hardware Gains:** Structured pruning provided genuine speedups by reducing FLOPs by 65% and memory footprint by 71%, whereas unstructured pruning offered only logical sparsity without inference speedup on standard hardware.

| Model            | Accuracy (%) | Params | Size (MB) | Inf. Time (ms) | FLOPs (M) |
|------------------|--------------|--------|-----------|---------------|-----------|
| **Baseline**     | 85.09        | 9.76M  | 37.24     | 10.79         | 153.29    |
| **Unstructured** | 89.25        | 9.76M  | 37.24     | 12.19         | 153.29    |
| **Structured**   | 87.60        | 2.79M  | 10.64     | 7.64          | 53.24     |

### 2. Quantization and Scaling Laws

The study evaluated the performance of models as numerical precision decreased, comparing PTQ and QAT frameworks.

**Key Findings:**

- **QAT Superiority:** QAT consistently outperformed PTQ across all bit-widths, particularly at INT4 where it achieved a 14.4% accuracy gain over PTQ.

- **Outlier Impact:** INT4 quantization is highly sensitive to activation outliers. Implementing 99.9% percentile clipping improved INT4 accuracy from 18.55% to 51.05%.

- **Mixed Precision:** Protecting the first and last layers at FP16 while quantizing intermediate layers to INT4 (Simple Mixed) achieved 68.32% accuracy, effectively mitigating the sharp degradation seen in uniform INT4 configurations.

### 3. Knowledge Distillation

Six distillation methods were evaluated to determine what knowledge is most effectively transferred to student models.

**Key Findings:**

- **Method Performance:** Contrastive Representation Distillation (CRD) and Decoupled KD (DKD) were the most effective, providing ~3.7% accuracy improvements over the independent student.

- **Capacity Mismatch:** Larger teachers (VGG-19) did not necessarily result in better students than VGG-16. Higher teacher confidence and lower entropy in larger models can hinder knowledge transfer.

- **Attention Transfer:** While advanced KD methods improved classification, they did not necessarily involve copying the teacher's spatial attention, suggesting students develop architecture-specific focus strategies.

| Method                  | Test Accuracy (%) | Improvement (%) | KL Divergence |
|-------------------------|-------------------|-----------------|---------------|
| **Teacher (VGG-16)**    | 74.00             | —               | —             |
| **Independent Student** | 60.63             | 0.00            | 2.90          |
| **Decoupled KD**        | 64.31             | +3.68           | 2.45          |
| **CRD**                 | 64.32             | +3.69           | 2.22          |

---
