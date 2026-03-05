# GCN vs GAT for Molecular Toxicity Prediction

This project compares two Graph Neural Network (GNN) architectures — **Graph Convolutional Networks (GCN)** and **Graph Attention Networks (GAT)** — on the **Tox21 molecular toxicity dataset**.

Molecules are naturally represented as graphs where:

- **Nodes = atoms**
- **Edges = chemical bonds**

The goal of this study is to evaluate whether **attention mechanisms improve predictive performance and interpretability** compared to traditional graph convolution methods.

---

# Overview

Graph Neural Networks have become the standard for modeling molecular data in drug discovery and computational chemistry.

This project evaluates two foundational GNN architectures:

**Graph Convolutional Network (GCN)**  
Uses fixed neighborhood aggregation weights.

**Graph Attention Network (GAT)**  
Uses learned attention weights to determine how much influence neighboring atoms have during message passing.

The study investigates whether attention-based aggregation improves prediction performance on toxicity tasks.

---

# Dataset

The models are trained on the **Tox21 dataset**, a benchmark dataset from **MoleculeNet**.

Key characteristics:

- **7,823 molecular graphs**
- **12 toxicity prediction tasks**
- **9 atom-level features**
- Average **18.6 atoms per molecule**
- Average **38.6 bonds per molecule**
- Approximately **17% missing labels**

Each molecule is represented as a graph where node features encode atomic properties such as atom type, degree, charge, and hybridization. :contentReference[oaicite:1]{index=1}

---

# Graph Structure

In molecular graphs:

- Atoms form nodes
- Bonds form edges
- Node degree corresponds to the number of chemical bonds

Typical degrees:

- Hydrogen: 1
- Oxygen: 2
- Carbon: 3–4

This structure is important because both GCN and GAT aggregate information from neighboring nodes during training.

---

# Methodology

Both models are trained under identical conditions to ensure a fair comparison.

### Training Setup

- Optimizer: **Adam**
- Learning rate: **0.001**
- Batch size: **64**
- Epochs: **50**
- Loss function: **Binary Cross Entropy with NaN masking**
- Evaluation metric: **AUROC**

AUROC is calculated for each toxicity task and then averaged across all tasks.

---

# Model Architectures

### Graph Convolutional Network (GCN)

Architecture:

```
GCNConv → ReLU  
GCNConv → ReLU  
Global Mean Pooling  
Linear Classification Layer
```

Hidden dimension: **64**

Parameter count:

```
5,580 parameters
```

---

### Graph Attention Network (GAT)

Architecture:

```
GATConv (4 attention heads) → ReLU  
GATConv → ReLU  
Global Mean Pooling  
Linear Classification Layer
```

Hidden dimension: **64**

Parameter count:

```
20,428 parameters
```

The attention mechanism allows the network to assign different importance to neighboring atoms during aggregation.

---

# Training Results

Both models were trained for **50 epochs**.

Final results:

| Model | Best AUROC | Final AUROC | Parameters |
|------|------------|------------|-----------|
| GCN | 0.7494 | 0.7492 | 5,580 |
| GAT | 0.7933 | 0.7897 | 20,428 |

Key finding:

**GAT consistently outperformed GCN**, suggesting that attention-based aggregation improves predictive performance on molecular toxicity tasks.

---

# Attention Analysis

The project also analyzes the distribution of attention weights in the GAT model.

Key observations:

- **Hierarchical attention patterns**
- Early layers aggregate broadly across neighbors
- Deeper layers focus on a smaller subset of important atoms

The attention distribution in deeper layers becomes **bimodal**, indicating that the network assigns near-binary importance to certain neighbors.

This behavior suggests that the model learns chemically meaningful patterns in molecular graphs.

---

# Limitations

Some limitations of the study include:

- Single train/test split (no cross-validation)
- Relatively shallow architectures (2 layers)
- No hyperparameter tuning
- Attention weights do not necessarily represent causal explanations

Despite these limitations, the results demonstrate the advantages of attention-based message passing for molecular prediction tasks.

---

# References

Kipf & Welling (2017) — *Semi-Supervised Classification with Graph Convolutional Networks*

Veličković et al. (2018) — *Graph Attention Networks*

Wu et al. (2018) — *MoleculeNet: A Benchmark for Molecular Machine Learning*
