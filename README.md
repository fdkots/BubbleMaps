# Bubble-Map-Augmented Graph Neural Networks for Scam Wallet Detection in the Solana Ecosystem

**Project Proposal**
**Author:** Faidon Kotsakis

---

## 1. Motivation and Problem Definition
The Solana blockchain’s throughput and low fees make it attractive for decentralized applications, but also for malicious actors. Scam wallets often exhibit characteristic behaviors: rapid accumulation of tokens, multi-hop routing, interactions with malicious program accounts, sudden bursts of activity, and short-lived transaction clusters. These behaviors are naturally represented in a transaction graph. Graph Neural Networks (GNNs) provide a powerful tool for modeling such graph-structured data.

Bubble maps, widely used for visual on-chain analytics, represent wallets as bubbles whose size, position, and color encode balances, embedding geometry, and cluster membership. Although bubble maps are used informally by analysts, they have not been mathematically formalized as machine-learning input features. This project aims to bridge this gap.

**Goal:** Create a GNN-based scam wallet classifier enhanced with bubble-map-derived metrics to identify malicious wallet behavior on Solana.

## 2. Related Work
Prior studies have investigated blockchain fraud detection, including graph-based classification approaches and illicit transaction tracing. Significant progress has been made with GNNs for transaction classification, particularly on other blockchains such as Bitcoin or Ethereum. Research has also explored node embeddings, link prediction, and temporal anomaly detection. However, no existing academic work integrates bubble-map spatial metrics or evaluates them as quantitative features in fraud detection.

Our project complements prior work by providing a novel geometric feature set derived from bubble-map layouts and applying them within a Solana-specific GNN model.

## 3. Problem Definition
Let the Solana network be a directed weighted graph:
$G = (V, E, w)$

where $V$ represents wallet addresses, $E$ represents token or SOL transfers, and $w(u, v)$ denotes the transferred amount.

Each wallet $v \in V$ has a feature vector consisting of structural, temporal, and bubble-map metrics:
$h^{(0)}_v = [x^{graph}_v, x^{temporal}_v, x^{bubble}_v]$

The goal is binary node classification using a GNN model:
$y_v = f(v) = \begin{cases} 1 & \text{if wallet is a scam} \\ 0 & \text{otherwise} \end{cases}$

$h^{(k+1)}_v = \sigma \left( W_k \cdot \text{AGG} \{ h^{(k)}_u : u \in \mathcal{N}(v) \} \right)$

## 4. Methodology

### 4.1 Data Collection
We will extract Solana transaction data from publicly available datasets and community-verified scam wallet lists. The dataset will include:
- Token-transfer graphs (SPL tokens and SOL)
- Known scam wallets (rug pulls, phishing, drainer wallets)
- Benign wallets sampled from regular user activity

**Expected graph size:** 100k–300k nodes.

### 4.2 Feature Engineering
**Graph Features:**
- In-degree / out-degree
- Total SOL flow in/out
- Program-call diversity
- PageRank
- Token diversity

**Temporal Features:**
- Slot-level burstiness
- Sudden activity spikes
- Accumulation speed

**Bubble-Map Features (Novel Contribution):**
Wallet embeddings are computed using Node2Vec followed by UMAP projection into 2D space. From this we define:
- Bubble radius (balance-based)
- Bubble growth rate
- Spatial outlier score
- Cluster compactness
- Dominance ratio within embedding clusters
- Local bubble density

### 4.3 GNN Architectures
We will evaluate multiple network architectures:
- Graph Convolutional Network (GCN)
- GraphSAGE
- Graph Attention Network (GAT)

**Model settings:**
- 2–3 layers
- Hidden dimensions 32–128
- ReLU activations
- Dropout 0.2–0.5
- Adam optimizer

**Training loss:**
$\mathcal{L} = - \sum_{v \in V_{train}} y_v \log(\hat{y}_v)$

## 5. Evaluation
We will evaluate the model using:
- AUC-ROC
- Precision, Recall, F1 score
- Precision@K (useful for early scam detection)
- Confusion matrix

### 5.1 Ablation Studies
We test the contribution of each feature type:
- GNN with graph-only features
- Bubble-map-only features
- Combined feature model

### 5.2 Case Studies
We will analyze:
- Rug-pull timelines and bubble expansions
- Clusters involved in malicious NFT mints
- Multi-hop laundering flows

## 6. Expected Contributions
- First integration of bubble-map geometric metrics into a GNN-based Solana fraud detection system
- Novel quantitative formalization of bubble-map behavior
- Demonstration that bubble features improve predictive accuracy
- Insightful visual and empirical analysis of scam wallet cluster dynamics on Solana

## 7. References
References will include foundational papers on GNNs, blockchain fraud detection, graph embeddings, and Solana-specific network documentation.