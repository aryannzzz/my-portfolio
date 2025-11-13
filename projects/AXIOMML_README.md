# AxiomML — Educational ML Framework from Scratch

**Timeline:** August 2025 – Present  
**Impact:** Used by 50+ students  
**Tech Stack:** Python, NumPy (pure implementation)

## Overview

Built a complete machine learning framework from scratch using only NumPy, achieving 99.9% accuracy parity with Scikit-learn while providing full mathematical transparency for educational purposes.

## Implemented Algorithms

### 1. **Supervised Learning**

#### Support Vector Machines (SVM)
- Linear and kernel-based SVMs
- SMO (Sequential Minimal Optimization) solver
- Multi-class classification strategies
- Custom kernel functions

#### Random Forests
- Decision tree implementation from scratch
- Bootstrap aggregating (bagging)
- Feature importance calculations
- Out-of-bag error estimation

### 2. **Unsupervised Learning**

#### K-Means Clustering
- Lloyd's algorithm implementation
- K-means++ initialization
- Elbow method for optimal K
- Cluster analysis tools

#### Principal Component Analysis (PCA)
- Eigenvalue decomposition
- Singular Value Decomposition (SVD)
- Dimensionality reduction
- Explained variance analysis

## Key Features

### Mathematical Transparency
- Every algorithm implemented with clear mathematical foundations
- Step-by-step computation visibility
- Educational comments and documentation
- No black-box operations

### Scikit-learn Compatible API
- Familiar `.fit()`, `.predict()`, `.transform()` interface
- Drop-in replacement for learning purposes
- Consistent parameter naming
- Standardized return formats

### Validation & Testing
- Comprehensive test suite
- 99.9% accuracy parity with Scikit-learn
- Performance benchmarking
- Edge case handling

## Technical Implementation

```python
# Example: Clean, educational implementation
class SVM:
    def __init__(self, kernel='linear', C=1.0):
        self.kernel = kernel
        self.C = C
        
    def fit(self, X, y):
        # Clear SMO algorithm implementation
        # Every step documented and transparent
        
    def predict(self, X):
        # Explicit decision function calculation
```

## Educational Impact

### Used By 50+ Students
- Teaching tool for ML internals
- Understanding algorithm mechanics
- Debugging and experimentation
- Research foundation

### Learning Outcomes
- Deep understanding of ML algorithms
- Appreciation of optimization techniques
- Insight into computational complexity
- Foundation for advanced research

## Modular Architecture

```
axiomml/
├── supervised/
│   ├── svm.py
│   ├── random_forest.py
│   └── decision_tree.py
├── unsupervised/
│   ├── kmeans.py
│   └── pca.py
├── utils/
│   ├── kernels.py
│   ├── metrics.py
│   └── preprocessing.py
└── tests/
    └── test_all.py
```

## Performance Validation

- **Accuracy Parity:** 99.9% match with Scikit-learn
- **Interpretability:** Full algorithm visibility
- **Educational Value:** Clear, documented implementations

## Use Cases

1. **Learning:** Understanding ML algorithm internals
2. **Teaching:** Classroom demonstrations
3. **Research:** Building custom ML pipelines
4. **Experimentation:** Algorithm modifications and testing

## Future Enhancements

- Additional algorithms (Gradient Boosting, Neural Networks)
- GPU acceleration with CuPy
- More extensive documentation
- Interactive tutorials

---

**Repository:** Open source educational toolkit  
**Users:** 50+ students across institutes  
**Purpose:** Teaching ML fundamentals through transparent implementations
