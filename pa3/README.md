# Robustness Under Distribution Shift: Domain Adaptation and Generalization

This repository explores **Domain Adaptation (DA)** and **Domain Generalization (DG)** to address the challenge of domain shift, where models encounter test-time data distributions that differ significantly from their training distributions. By implementing state-of-the-art alignment, invariant learning, and prompt-tuning techniques, we evaluate strategies to mitigate **spurious correlations** and improve performance on completely unseen domains.

## Research Objectives

This study addresses three fundamental research questions:

* 
**Unsupervised Adaptation:** How can models effectively adapt to new domains without target labels by balancing domain alignment with task-specific performance? 


* 
**Robust Generalization:** Which training strategies enable models to generalize across diverse source domains and maintain reliability on unseen target domains? 


* 
**Multimodal Transferability:** How can vision-language models (CLIP) be adapted for open-set environments, and what trade-offs exist between specialization and generalization? 



---

## 🔬 Experimental Analysis

### 1. Unsupervised Domain Adaptation (UDA)

We evaluated the **Art Painting → Cartoon (A→C)** transfer scenario on the PACS dataset using a ResNet-18 backbone.

* 
**Alignment Methods:** We compared statistical alignment (**DAN** via MK-MMD), adversarial alignment (**DANN** via gradient reversal), and class-aware conditional alignment (**CDAN**).


* 
**Self-Training:** We implemented **Pseudo-labeling** with a 0.9 confidence threshold to leverage target data structure without true labels.


* 
**Concept Shift:** Methods were stress-tested under severe label shift by selectively removing target classes and oversampling others.



### 2. Domain Generalization via Invariant & Robust Learning

Using a **leave-one-domain-out** protocol (holding out the **Sketch** domain), we explored three distinct mechanisms for cross-domain robustness:

* 
**Invariant Risk Minimization (IRM):** Optimized for stable feature representations across environments by penalizing gradient variance.


* 
**Group DRO:** Employed distributionally robust optimization to minimize the worst-case loss across source domains through adaptive reweighting.


* 
**Sharpness-Aware Minimization (SAM):** Optimized the loss landscape geometry to find flatter minima, reducing sensitivity to domain-specific perturbations.



### 3. Prompt Tuning with CLIP

We investigated **Context Optimization (CoOp)** as a lightweight alternative to full model fine-tuning.

* 
**Zero-Shot vs. Linear Probing:** Evaluated the out-of-the-box generalization of CLIP versus tuning a task-specific classifier head.


* 
**Gradient Alignment:** Analyzed gradient conflicts between diverse domains and applied **GradCos** to re-align opposing gradient directions.


* 
**Open-Set Analysis:** Measured the impact of prompt tuning on identifying unseen classes using **Maximum Softmax Probability (MSP)**.



---

## 📊 Core Findings

### Model Performance Summary (PACS Dataset)

| Method | Source Avg (%) | Target (Sketch) Accuracy (%) | Generalization Gap (%) |
| --- | --- | --- | --- |
| **ERM (Baseline)** | 95.30% 

 | 61.59% 

 | 33.71% 

 |
| **IRM** | 93.61% 

 | 63.96% 

 | <br>**29.65%** 

 |
| **Group DRO** | 93.87% 

 | 65.59% 

 | 28.28% |
| **SAM** | <br>**95.20%** 

 | <br>**67.70%** 

 | 27.50% |
| **CDAN (UDA)** | 87.32% 

 | 77.61%* (A→C) 

 | — |

### Key Takeaways

1. 
**Geometric vs. Explicit Regularization:** **SAM** achieved the best target accuracy (67.7%) by finding flatter minima, proving that geometry-aware optimization can enhance robustness without the discriminative trade-offs required by IRM or Group DRO.


2. 
**The Invariance Trade-off:** **IRM** reduced spurious correlation reliance (domain silhouette improved from 0.0471 to 0.0183) but sacrificed 1.69% of source performance to enforce invariance.


3. 
**Fragility of Global Alignment:** While **CDAN** excelled under standard conditions (0.3441 silhouette score), all UDA alignment strategies proved brittle under **concept shift**, failing when the target label distribution diverged from the source.


4. 
**Overconfidence in Prompt Tuning:** While **CoOp** improved in-domain CLIP performance, it reduced flexibility in open-set scenarios; the average MSP for unseen classes dropped sharply to 0.785, making it harder to detect truly unknown samples.


5. 
**Domain Difficulty as a Signal:** **Group DRO** identified **Art Painting** as the most challenging domain (final weight: 0.3747); focusing on this difficult domain during training led to superior transfer to the Sketch domain.



---
