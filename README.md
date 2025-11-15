# Song Clustering Analysis

A statistical pattern recognition project exploring unsupervised learning techniques for clustering songs based on audio features.

**Authors:** Juliana Alscher, Clea Demuynck, and J. Brandinger

## Overview

This project applies machine learning clustering algorithms to classify songs into distinct groups based on audio characteristics such as BPM, energy, danceability, loudness, valence, acousticness, and speechiness. We compare the performance of K-Means, MiniBatch K-Means (with various batch sizes), and Gaussian Mixture Models (GMM) to determine optimal clustering strategies.

## Dataset

The dataset (`song-data.csv`) contains **1,358 songs** with the following features:

- **title**: Song name
- **artist**: Artist name
- **top genre**: Specific genre classification
- **bpm**: Beats per minute (tempo)
- **Energy**: Energy level (0-100)
- **Dance**: Danceability score (0-100)
- **dB**: Loudness in decibels
- **val**: Valence/positivity (0-100)
- **acous**: Acousticness (0-100)
- **spch**: Speechiness (0-100)

After preprocessing and filtering genres with >10 occurrences, the working dataset contains **1,166 songs** across 11 major genre categories (pop, hip hop, rap, rock, metal, soul, reggae, dance, indie, house, r&b, edm).

## Methods

### Data Preprocessing
1. Removed unnecessary columns (index, added date, live, duration, popularity, year)
2. Cleaned missing values
3. Consolidated subgenres into major genre categories
4. Standardized features using `StandardScaler`
5. Split data: 70% training, 30% testing

### Clustering Algorithms

#### 1. **K-Means Clustering**
- Tested cluster numbers from 1 to 100
- Used elbow method and silhouette scores for optimization
- **Best result:** 95 clusters with cost of -427.81 (0.139s runtime)

#### 2. **MiniBatch K-Means**
Evaluated five different batch sizes to optimize speed/accuracy tradeoff:
- Batch size 49
- Batch size 100
- Batch size 256
- Batch size 512
- Batch size 1024

**Key Finding:** Batch size 49 provided the best balance, achieving a cost of 432.95 with 99 clusters in just 0.087 seconds - significantly faster than standard K-Means.

#### 3. **Gaussian Mixture Models (GMM)**
- Tested components from 1 to 100
- **Best result:** 8 components with log-likelihood of -6.87
- Provides probabilistic cluster assignments
- More interpretable cluster structure than K-Means with 95 clusters

## Key Results

### Performance Comparison

| Algorithm | Clusters/Components | Cost/Log-Likelihood | Runtime |
|-----------|-------------------|---------------------|---------|
| K-Means | 95 | -427.81 | 0.139s |
| MiniBatch (49) | 99 | -432.95 | 0.087s |
| MiniBatch (100) | 99 | -435.13 | 0.091s |
| MiniBatch (256) | 99 | -428.59 | 0.135s |
| GMM | 8 | -6.87 | N/A |

### Insights
- **Silhouette analysis** suggested 2 clusters as optimal, indicating that songs may naturally divide into two broad categories
- **Elbow method** showed diminishing returns after ~10-20 clusters
- **GMM with 8 components** provides more interpretable results while maintaining competitive log-likelihood
- **MiniBatch K-Means** significantly reduces computation time with minimal accuracy loss
- **PCA visualization** (2 components) reveals continuous rather than discrete cluster structure

### Model Comparison on PCA-reduced Data
- **K-Means (95 clusters):** Log-likelihood of -3.95
- **GMM (8 components):** Log-likelihood of -9.35

While K-Means shows better numerical performance, GMM provides more interpretable cluster assignments that may better represent the underlying genre structure.
