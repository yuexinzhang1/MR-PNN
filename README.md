# MR-PNN: A Knowledge-Driven Continual Learning Network for Alzheimer's Disease Diagnosis
MR-PNN is a sophisticated deep learning framework designed for Alzheimer's Disease (AD) diagnosis by integrating and learning from multiple-omics data. The model leverages genetic, phenotypic, and structural MRI (sMRI) data through a novel architecture that combines multi-scale attention, adaptive expert systems, and progressive knowledge transfer to achieve robust and accurate predictions.
# 🧠 Core Architecture
The MR-PNN framework is composed of four main components working in concert:
Multiple-Omics Associations Matrix: The foundational data structure.
Multi-scale Attention Learning Module: For fusing heterogeneous inputs.
MR-SMoE Module: For adaptive, input-dependent computation.
PNN Module: For continual learning and knowledge transfer across tasks.
1. Multiple-Omics Associations
This component constructs the integrated data representation used by the network.
Genetic Data: 77 top AD-related SNPs selected via Recursive Feature Elimination (RFE).
sMRI Data: A 34x6 matrix representing 34 brain Regions of Interest (ROIs), each with 6 structural features (e.g., volume, thickness).
Associations Matrix: A 24x6 matrix that links 24 key AD-related sMRI features with their corresponding genetic and phenotypic factors. Each of the 24 features is paired with:
Polygenic Risk Score (PRS)
Age
Sex
Age- and sex-matched population mean
Age- and sex-matched population variance
The PRS is calculated from large-scale GWAS results on the UK Biobank (UKB) dataset using the formula:
PRS = Σ(ES_i * C_i)
where ES_i is the effect size of SNP i and C_i is the subject's allele count.
2. Multi-scale Attention Learning Module
This module fuses the three distinct input modalities and learns their complex interdependencies.
Inputs:
Genetic features (77 SNPs)
Multiple-omics associations matrix (24x6)
sMRI feature matrix (34x6)
Process:
Each modality is independently encoded into a common latent space using a Multi-Layer Perceptron (MLP).
A custom multi-scale cross-attention mechanism is applied. This mechanism uses convolutional filters with different kernel sizes (3, 5, 7) to extract patterns at various scales before computing attention.
The attended features are concatenated and passed through another MLP to produce the final fused representation Y_F.
3. MR-SMoE Module (Multi-Route Sparse Mixture of Experts)
This module enables the network to adapt its computation dynamically based on the input.
Experts: A set of N specialized neural networks (E_i), each designed to handle specific data patterns.
Router: A gating network that analyzes the fused input Y_F and selects the top-k most relevant experts.
Output: The final output Y_M is a weighted sum of the outputs from the selected experts, allowing for highly flexible and efficient processing.
4. PNN Module (Progressive Neural Network)
This module addresses the challenge of limited data by enabling continual learning from related tasks without forgetting previous knowledge.
Architecture: Organized as a series of vertical "columns," where each column c_t is dedicated to a specific task t.
Knowledge Transfer: Each new column can access and integrate intermediate features from all previously learned columns through trainable adapter layers (f_A).
Catastrophic Forgetting Prevention: When training on a new task t, the parameters of all previous columns (k < t) are frozen (their gradients are set to zero). This ensures that knowledge from past tasks is preserved while the model adapts to the new one.
# 📊 Input Data Requirements
To use this model, you will need to prepare the following data for each subject:
Genetic Data: Genotype data for the 77 pre-selected AD-related SNPs.
Phenotypic Data: Age and sex.
sMRI Data: Structural measurements (e.g., volume, cortical thickness) for the 34 specified ROIs.
# 🚀 Getting Started
Data Preprocessing: Process your genetic, sMRI, and phenotypic data to match the required formats (77 SNPs, 34x6 sMRI matrix).
Construct Associations: Calculate the PRS and build the 24x6 multiple-omics associations matrix.
Model Initialization: Initialize the MR-PNN architecture with the components described above.
Training: Train the model sequentially on your tasks, leveraging the PNN module for continual learning.
