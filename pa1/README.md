

# Investigating Inductive Biases in Vision Models

This assignment explores how architectural designs and training objectives impose "inductive biases"—the built-in assumptions models use to generalize beyond their training data. By conducting hypothesis-driven experiments across **discriminative**, **generative**, and **contrastive** frameworks, we evaluate how these biases affect robustness to out-of-distribution (OOD) shifts, such as style changes, spatial disruptions, and domain transfers.

## Research Objectives

The study is framed by several key research questions:

* 
**Shape vs. Texture:** Do CNNs and ViTs rely on different visual cues for recognition? 


* 
**Architectural Robustness:** How do convolutions vs. self-attention handle translations and occlusions? 


* 
**Generative Trade-offs:** How do VAEs and GANs differ in capturing data manifolds versus realism? 


* 
**Multimodal Synergy:** Does alignment with natural language (CLIP) shift visual bias toward human-like perception? 



---

## 🔬 Experimental Analysis

### 1. Discriminative Models: CNN (ResNet-50) vs. ViT (ViT-S/16)

We analyzed how local receptive fields (CNN) compare to global self-attention (ViT) when processed on CIFAR-10.

* 
**Semantic Biases:** Models were tested on grayscale and "cue-conflict" images where texture (e.g., elephant skin) was applied to a different shape (e.g., a cat).


* 
**Spatial Invariance:** We measured consistency across pixel shifts (translation), patch shuffling (permutation), and square masking (occlusion).


* 
**Domain Generalization:** Performance was evaluated on the PACS dataset (Photo, Art, Cartoon, Sketch) to observe how models handle extreme stylistic shifts.



### 2. Generative Models: VAE vs. GAN

We compared a Variational Autoencoder (VAE) using KL-annealing against a Deep Convolutional GAN (DCGAN) on the CIFAR-10 distribution.

* 
**Latent Manifolds:** We performed latent space interpolations to check for "smoothness"—the ability of the model to transition logically between two data points.


* 
**Fidelity vs. Diversity:** Quantitative metrics (Inception Score and FID) were used to measure the sharpness of samples against the variety of classes generated.



### 3. Contrastive Models: CLIP (ViT-B/32)

We utilized a pre-trained CLIP model to examine the effect of massive image-text alignment on zero-shot generalization.

* 
**Zero-Shot Transfer:** Tested CLIP’s ability to classify CIFAR-10 and PACS-Sketch images without any task-specific fine-tuning.


* 
**Multimodal Bias:** Conducted shape vs. texture tests to see if language supervision guides the model toward more abstract, semantic features.



---

## 📊 Core Findings

| Metric/Observation | ResNet-50 | ViT-S/16 | CLIP (Zero-Shot) |
| --- | --- | --- | --- |
| **Shape Bias** | 36.00% 

 | 60.60% 

 | <br>**76.7%** 

 |
| **Grayscale Accuracy** | 15.80% 

 | 35.90% 

 | — |
| **Translation Consistency** | <br>**98.00%** 

 | 96.83% 

 | — |
| **Sketch Domain Acc.** | 54.11% 

 | 37.57% 

 | <br>**~85%** 

 |

### Key Takeaways

1. 
**Texture vs. Shape:** CNNs exhibit a strong dependency on texture and color (73.31% drop on grayscale). ViTs and CLIP are significantly more shape-oriented, allowing for better robustness to stylistic corruption.


2. 
**Architectural Trade-offs:** CNNs possess inherent translational equivariance. However, ViTs handle patch permutations (48.8% vs 19.2%) and occlusions (89.8% vs 81.0%) better, leveraging global context where CNNs' local receptive fields fail.


3. 
**Generative Biases:** GANs produce high-fidelity, sharp images (FID: 86.98) but suffer from lower diversity. VAEs prioritize distribution coverage (diversity) but produce blurrier reconstructions due to the pixel-wise MSE objective.


4. 
**Language as a Regularizer:** CLIP’s multimodal training acts as a powerful semantic regularizer. It achieves human-like shape bias and generalizes to sketches far better than supervised models, indicating that training data variety can sometimes overshadow architectural constraints.



---
