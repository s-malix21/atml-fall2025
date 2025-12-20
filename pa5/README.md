# LLM Alignment and Mechanistic Interpretability: Decoding, Fine-Tuning, and Universal Representations

This project conducts a multi-faceted evaluation of Large Language Model (LLM) alignment and the internal representation geometry of neural networks. The study investigates how decoding strategies and reinforcement learning-based alignment (PPO, DPO, GRPO) affect model behavior, while utilizing Universal Sparse Autoencoders (USAEs) to test the Platonic Representation Hypothesis across distinct computer vision architectures.

---

## Assignment Overview

The research is divided into three primary investigation areas:

1. **Decoding Dynamics:** A comparative analysis of deterministic (Greedy, Beam Search) and stochastic (Top-K, Top-P) decoding strategies to evaluate the trade-offs between generation quality, diversity, and computational latency.

2. **Alignment Regimes:** An empirical study of alignment techniques—Direct Preference Optimization (DPO), Proximal Policy Optimization (PPO), and Group Relative Policy Optimization (GRPO)—focusing on their effectiveness in mitigating verbosity bias and reward hacking.

3. **Universal Latent Spaces:** The application of Universal Sparse Autoencoders to identify monosemantic, shared features between ResNet and Vision Transformer (ViT) architectures, quantifying the "Alignment Tax" required to map diverse geometries into a shared bottleneck.

---

## Methodology and Technical Analysis

### 1. Decoding Strategy Evaluation

The study utilized the **SmolLM2-135M-Instruct** model to benchmark decoding performance across 15 diverse instructional prompts.

- **Deterministic vs. Stochastic:** Deterministic methods prioritized structural grammatical safety, while sampling-based methods (Top-K/Top-P) introduced variance in quality but avoided the repetitive loops characteristic of maximization-based strategies.

- **Temperature Ablation:** Experiments across  demonstrated that lower temperatures favored coherence, while higher values led to sequence collapse in smaller models.

### 2. LLM Alignment Techniques

Models were aligned using the **Orca DPO Pairs** dataset and evaluated for catastrophic forgetting and length compliance.

- **PPO and GRPO:** These RL-based schemes utilize reward models to provide scalar feedback. GRPO, in particular, eliminates the need for a learned critic by leveraging group-wise relative comparisons.

- **DPO:** A simpler approach that eliminates the reward model by directly optimizing the policy on human preference pairs, significantly reducing computational overhead.

### 3. Mechanistic Interpretability via USAEs

A USAE was trained to map activations from the final blocks of **ResNet-18** and **ViT-B-16** into a shared dictionary space ().

- **Universality Metrics:** Alignment was quantified via cross-model reconstruction scores and Normalized Firing Entropy (), distinguishing between universal features and model-specific artifacts.

---

## Key Findings

### Decoding Performance and Stability

- **Efficiency vs. Quality:** Beam Search () achieved the highest quality (4.11 reward) but was the slowest method (5.3 tokens/sec), whereas Top-P offered a superior balance of coherence and diversity at higher speeds (>21 tokens/sec).

- **Length Bias:** Top-K sampling exhibited extreme instability (CV: 3.27) and a near-perfect correlation () between output length and reward, suggesting it "gamed" evaluation by generating longer text.

### Alignment and Verbosity Bias

| Model            | Mean Tokens | Skewness | KL Divergence |
|------------------|-------------|----------|---------------|
| **Base (SFT)**   | 217.58      | -1.69    | —             |
| **DPO**          | 179.17      | -0.74    | 0.0163        |
| **GRPO**         | 211.67      | -1.42    | 0.0006        |
| **PPO (Sparse)** | 207.92      | -1.38    | 0.0015        |

- **Length Control:** DPO was the most effective at enforcing strict length constraints and respecting explicit word limits, whereas RL-based methods (PPO/GRPO) showed a persistent bias toward maximal elaboration.

- **Catastrophic Forgetting:** GRPO achieved the lowest KL divergence, indicating it retained the original model's behavior more effectively than DPO under constrained training schedules.

### The Cost of Universality (USAEs)

- **Alignment Tax:** Enforcing a shared latent space incurred an 11.4% drop in reconstruction fidelity compared to independent SAEs, illustrating that while models learn the same concepts, they differ fundamentally in implementation.

- **Bimodal Universality:** Firing entropy was strictly bimodal; features were either universal "bridges" representing coherent visual patterns (e.g., rigid objects like trucks) or model-specific artifacts that appeared as noise to the other architecture.

- **Functional Importance:** A "Universal Lobotomy" experiment confirmed that a small subset of high-entropy features carries the bulk of the cross-model translational load.
