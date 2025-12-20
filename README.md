# Advanced Topics in Machine Learning (ATML):

**Course material and programming assignments for EE-5102 / CS-6304 – Advanced Topics in Machine Learning (Fall 2025, LUMS), covering deep representation learning, interpretability, efficiency, federated systems, domain adaptation, and reinforcement learning for LLMs.**

This repository serves as a comprehensive investigation into the frontiers of modern machine learning. Through a series of hypothesis-driven experiments, it explores how deep learning systems generalize, fail, and can be optimized for real-world deployment across diverse environments.

---

## 🏛️ Core Research Pillars

### 1. Investigating Inductive Biases

This module explores how architectural designs impose "built-in" assumptions. We contrast the **local receptive fields** of CNNs with the **global self-attention** of Vision Transformers (ViTs) and the **multimodal alignment** of CLIP.

* **Shape vs. Texture:** Probing why CNNs are "texture-hungry" while ViTs and CLIP align closer to human-shaped perception.
* **Generative Trade-offs:** Evaluating the latent manifold smoothness of VAEs versus the high-fidelity outputs of GANs.
* **Key Insight:** ViTs handle spatial disruptions (occlusions/permutations) significantly better than CNNs by leveraging global context.

### 2. Robustness & Domain Adaptation

Models often fail when test data deviates from the training distribution. This section implements strategies for **Domain Adaptation (DA)** and **Domain Generalization (DG)** using the PACS dataset.

* **Invariant Learning:** Using **IRM** and **Group DRO** to minimize reliance on "spurious correlations" (e.g., specific backgrounds).
* **Optimization Geometry:** Leveraging **Sharpness-Aware Minimization (SAM)** to find "flatter" minima that generalize better to unseen styles like Sketch or Art.
* **Prompt Tuning:** Adapting CLIP via **CoOp** to specialize on new domains while maintaining open-set flexibility.

### 3. Advanced Model Compression

To deploy models on edge devices, we must reduce their footprint without sacrificing accuracy. This module benchmarks optimization techniques on VGG architectures:

* **Pruning:** Comparing **unstructured** (weight-level) vs. **structured** (channel-level) pruning for hardware-level speedups.
* **Quantization:** Analyzing the performance "cliff" of INT4 precision and how **Quantization-Aware Training (QAT)** recovers accuracy.
* **Knowledge Distillation:** Using **CRD** and **DKD** to transfer "dark knowledge" from high-capacity teachers to compact students.

### 4. Federated Learning & Heterogeneity

This section addresses the challenges of training models on decentralized, "Non-IID" (label-skewed) data without exchanging raw information.

* **Client Drift:** Measuring how local data skew (simulated via **Dirichlet partitioning**) causes local models to diverge from the global objective.
* **Mitigation Algorithms:** Benchmarking **FedProx**, **SCAFFOLD**, and **FedSAM** to stabilize global aggregation.
* **Key Finding:** Seeking flatter minima (FedSAM) is more effective at handling data heterogeneity than standard regularization penalties.

### 5. LLM Alignment & Interpretability

The final module explores how we control Large Language Models (LLMs) and investigate their internal representations.

* **Alignment Regimes:** A head-to-head comparison of **DPO**, **PPO**, and **GRPO** for reducing verbosity bias and reward hacking.
* **Decoding Dynamics:** Benchmarking how strategies like **Top-P (Nucleus) Sampling** balance diversity with coherence.
* **Mechanistic Interpretability:** Using **Universal Sparse Autoencoders (USAEs)** to identify shared features between ResNet and ViT, testing the Platonic Representation Hypothesis.

---
