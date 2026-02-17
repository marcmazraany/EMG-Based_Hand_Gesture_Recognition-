# Real-Time EMG Gesture Recognition System

A real-time hand gesture recognition system built using surface electromyography (sEMG) signals.  
This project focuses on designing a robust machine learning pipeline capable of generalizing to unseen users while maintaining real-time feasibility.

---

## Overview

Electromyography (EMG)-based gesture recognition enables intuitive human-machine interaction with applications in:

- Prosthetic control  
- Virtual and augmented reality  
- Assistive technologies  
- Human-computer interfaces  

EMG signals are inherently noisy, user-dependent, and highly variable. The goal of this project was to design a reliable end-to-end system including signal preprocessing, feature engineering, model selection, and rigorous cross-validation to ensure real-world applicability.

**Final Performance (Unseen Subjects):**
- Accuracy: 90.24%
- F1-Score: 90.04%

---

## Dataset

- 36 participants  
- 8 distinct hand gestures  
- 8 EMG channels  
- Leave-One-Subject-Out cross-validation  

The leave-one-out setup ensures strict separation between training and testing subjects, simulating deployment on new users.

---

## System Pipeline

### 1. Signal Preprocessing

To ensure high-quality signal representation, the following steps were applied:

- Bandpass filtering  
- Notch filtering  
- Full-wave rectification  
- Normalization  

These steps reduce noise, remove powerline interference, and standardize signal amplitudes across subjects.

---

### 2. Windowing Strategy

Time-series segmentation was a critical design decision.

- Window size: **200 samples**
- Step sizes tested:
  - 100 samples (50% overlap)
  - 200 samples (no overlap)

Larger windows (e.g., 1000 samples) degraded performance due to reduced temporal resolution and mixed gesture signals within a single window.

Overlapping windows improved robustness but were carefully evaluated to prevent data leakage.

---

### 3. Feature Engineering

From each window:

- ~14 time-domain features per channel  
- 8 channels → 112 total features  

Extracted features included:

- Mean  
- Standard deviation  
- Energy  
- Zero-crossing rate  
- Additional statistical descriptors  

To improve efficiency and generalization:

- Mutual Information ranking  
- Recursive Feature Elimination (RFE)  

The top 20 most informative features were selected for final evaluation.

---

## Models Evaluated

The following classifiers were implemented and compared:

- Random Forest  
- Support Vector Machine (SVM)  
- K-Nearest Neighbors (KNN)  
- Decision Tree  

---

## Results Summary

| Setup | Classifier | Accuracy | F1-Score |
|-------|------------|----------|----------|
| Overlapping Windows | Random Forest | 93.60% | 93.58% |
| No Overlap | Random Forest | 92.56% | 92.47% |
| Leave-One-Out (Full Features) | RF | 88.92% | 88.54% |
| Leave-One-Out (Top 20 Features) | RF | **90.24%** | **90.04%** |

### Key Observations

- Random Forest consistently delivered the strongest performance.
- KNN performed competitively, especially on smaller or cleaner subsets.
- Decision Trees were more sensitive to data variability.
- Feature selection improved both training efficiency and generalization.

Feature reduction improved computational cost while increasing accuracy, highlighting the importance of dimensionality control in EMG classification.

---

## Engineering Insights

This project emphasizes:

- The impact of window size on temporal modeling  
- Risks of overlap-induced data leakage  
- The importance of feature selection for generalization  
- The strength of robust classical ML models for structured EMG features  

Random Forest with the top 20 selected features was chosen as the final configuration.

---

## Future Improvements

- Deep learning architectures (1D CNN, CNN-LSTM)  
- Real-time latency benchmarking  
- Embedded deployment optimization  
- Integration with wearable EMG hardware  

---

## Tech Stack

- Python  
- NumPy  
- Scikit-learn  
- Signal processing libraries  
