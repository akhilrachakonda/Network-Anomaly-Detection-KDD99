# Network Anomaly Detection using LSTM Autoencoders and Isolation Forest

This project compares classical and deep learning-based unsupervised anomaly detection techniques for identifying network intrusions using the KDDCup’99 dataset. The models include a baseline Isolation Forest, a simple LSTM Autoencoder, and a deeper LSTM Autoencoder designed to capture complex temporal dependencies in network traffic.

## 📌 Project Overview

- **Dataset:** [KDDCup’99](http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html)
- **Goal:** Detect network intrusions (attacks) using anomaly detection.
- **Approach:** 
  - Train models only on normal traffic (unsupervised setting).
  - Evaluate on both normal and attack samples.
  - Compare performance using reconstruction errors, ROC AUC, F1-score, precision, recall, and confusion matrix.

## 🧠 Models Implemented

### ✅ Isolation Forest
- Classical anomaly detection method based on tree ensembles.
- Fast, interpretable, and efficient for structured tabular data.
- Trained using only normal instances.
- Thresholding via contamination (5%).

### 🔁 Simple LSTM Autoencoder
- Single-layer LSTM encoder-decoder architecture.
- Designed to reconstruct normal sequences and detect anomalies via high reconstruction error.
- Architecture:
  - LSTM (64 units) → RepeatVector → LSTM (64 units) → TimeDistributed Dense.
- Optimized using MSE loss and Adam optimizer.
- Threshold set to 95th percentile of training reconstruction MSE.

### 🔁 Deep LSTM Autoencoder
- Multi-layer encoder-decoder with increased representational capacity.
- Two LSTM layers (128, 64) in encoder and decoder.
- Better suited for capturing complex, abstract temporal patterns in network traffic.
- Includes early stopping to prevent overfitting.

## 🛠️ Methodology

- **Data Preprocessing**
  - Label encoding for `protocol_type`, `service`, and `flag`.
  - Binary classification: `normal` = 0, `attack` = 1.
  - Features normalized using `StandardScaler`.
  - Training performed only on normal traffic.

- **Training**
  - LSTM models trained on reshaped 3D sequences.
  - Reconstruction error (MSE) used for anomaly scoring.
  - Isolation Forest used directly on preprocessed 2D data.

- **Thresholding**
  - LSTM models: 95th percentile of reconstruction error.
  - Isolation Forest: Scoring based on contamination.

- **Evaluation Metrics**
  - ROC AUC
  - F1-Score
  - Precision & Recall
  - Confusion Matrix
  - ROC Curves and Error Distribution Histograms

## 📊 Results Summary

| Model               | ROC AUC | F1 Score | Precision | Recall | Accuracy |
|--------------------|---------|----------|-----------|--------|----------|
| Simple LSTM AE     | 0.9553  | 0.8620   | 0.9360    | 0.8131 | 0.8620   |
| Deep LSTM AE       | 0.9589  | 0.8822   | 0.9590    | 0.8167 | 0.8758   |
| Isolation Forest   | 0.9397  | 0.7771   | 0.9747    | 0.6461 | 0.7890   |

✅ Deep LSTM outperformed others across all key metrics.  
🧩 Isolation Forest showed high precision but struggled with recall.

## 📂 Repository Structure

```bash
.
├── Simple-LSTM.ipynb        # Simple LSTM Autoencoder implementation
├── Deep-LSTM.ipynb          # Deep LSTM Autoencoder implementation
├── Isolatedforest.ipynb     # Isolation Forest implementation
├── README.md                # This file
