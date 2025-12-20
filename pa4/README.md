# Federated Learning: Foundations and Heterogeneity Mitigation

This project provides an empirical study of **Federated Learning (FL)**, investigating the theoretical foundations of decentralized optimization and benchmarking state-of-the-art algorithms designed to handle non-IID data distributions. The research evaluates **FedAvg** and its extensions—including regularization, control-variate, and sharpness-aware methods—on the CIFAR-10 dataset.

---

## Assignment Overview

Federated Learning enables collaborative model training without exchanging raw data, preserving privacy in sensitive domains like healthcare and finance. This study addresses four critical areas:

- **Theoretical Foundations:** Proving and empirically demonstrating the equivalence of FedSGD and centralized SGD.

- **Hyperparameter Sensitivity:** Analyzing the impact of local training epochs () and client sampling fractions () on convergence.

- **Data Heterogeneity:** Simulating non-IID scenarios using **Dirichlet partitioning** to measure the impact of label skew on performance.

- **Mitigation Strategies:** Benchmarking advanced FL algorithms (FedProx, SCAFFOLD, FedGH, FedSAM) against a baseline to reduce client drift.

---

## Key Analysis and Findings

### 1. Theoretical Equivalence

The project proves that **FedSGD** is theoretically equivalent to **centralized SGD** when all clients participate and use full-batch gradients. Empirical tests on CIFAR-10 showed nearly perfect overlap in loss and accuracy trajectories, with minor deviations attributed solely to floating-point precision.

### 2. FedAvg Hyperparameter Dynamics

Increasing local computation reduces communication frequency but introduces **client drift**, where local models diverge from the global objective.

**Table 1: Impact of Local Epochs () on Performance and Drift**

| Local Epochs () | Final Accuracy | Final Client Drift |
|-----------------|----------------|--------------------|
| 1               | 0.6263         | 19.52              |
| 5               | 0.6984         | 44.89              |
| 10              | **0.7029**     | 45.05              |
| 20              | 0.6947         | 53.83              |

**Findings:**

- **Diminishing Returns:** Accuracy gains plateau after , while client drift continues to increase as clients optimize more independently.

- **Sampling Efficiency:** Reducing the client sampling fraction () significantly lowers communication costs with only a moderate impact on accuracy, making  a practical choice for bandwidth-constrained environments.

### 3. Impact of Data Heterogeneity

Data heterogeneity was simulated using a Dirichlet distribution concentration parameter (), where lower values represent higher label skew.

**Table 2: FedAvg Performance under Varying Heterogeneity ()**

|  | Heterogeneity Level | Test Acc. (%) | Gen. Gap (%) |
|---|---------------------|---------------|---------------|
| 100.00 | Near-IID          | **64.09**     | 35.87         |
| 0.50   | Moderate          | 54.28         | 41.28         |
| 0.05   | High Label Skew   | 41.04         | 51.12         |

**Findings:**

- **Performance Gap:** There is a >23% accuracy drop between IID and highly heterogeneous settings.

- **Generalization Failure:** In non-IID settings, the generalization gap exceeds 50%, indicating that clients overfit to their local skewed distributions while the aggregated model fails to generalize globally.

### 4. Mitigation Strategies Benchmark

The study compared several specialized algorithms designed to combat client drift in non-IID settings ().

**Table 3: Performance Summary of FL Mitigation Methods**

| Method      | Test Acc (%) | Test Loss | Gen. Gap (%) | Avg. Div. |
|-------------|--------------|-----------|--------------|-----------|
| **FedAvg**  | 40.55        | 1.88      | 54.03        | 0.00145   |
| **FedProx** | 44.01        | 1.81      | 51.65        | 0.00140   |
| **SCAFFOLD**| 27.44        | 2.03      | **41.82**    | **0.00042** |
| **FedSAM**  | **46.04**    | **1.62**  | 48.00        | 0.00146   |

**Findings:**

- **Top Performer:** **FedSAM** achieved the highest accuracy by seeking flatter minima, which enhances generalization under biased data.

- **Robust Regularization:** **FedProx** provided a reliable and efficient balance between stability and accuracy.

- **SCAFFOLD Paradox:** While SCAFFOLD achieved the lowest model divergence (successfully suppressing drift), it suffered from lower accuracy, highlighting extreme sensitivity to hyperparameter tuning in practical settings.

---

## Implementation Details

- **Model Architectures:** Evaluated using two CNN variants; Model 1 utilized **Group Normalization** to ensure deterministic performance for theoretical proofs, while Model 2 used **Batch Normalization** for standard experiments.

- **Optimization:** Local training utilized SGD with a learning rate of 0.01 and momentum of 0.9.

- **Heterogeneity Mitigation:** Specialized modules include proximal penalties (FedProx), control variates (SCAFFOLD), gradient orthogonalization (FedGH), and adversarial weight perturbations (FedSAM).

---
