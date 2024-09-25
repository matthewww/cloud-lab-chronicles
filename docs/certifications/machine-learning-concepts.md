```mermaid
graph LR;
    A[Machine Learning] --> B[Supervised Learning]
    A --> C[Unsupervised Learning]
    A --> D[Semi-Supervised Learning]
    A --> E[Reinforcement Learning]
    
    %% Supervised Learning
    B --> B1[Classification]
    B --> B2[Regression]
    
    %% Unsupervised Learning
    C --> C1[Clustering]
    C --> C2[Dimensionality Reduction]
    
    %% Classification Algorithms
    B1 --> B1A[Logistic Regression]
    B1 --> B1B[Decision Trees]
    B1 --> B1C[SVM]
    B1 --> B1D[Neural Networks]
    
    %% Regression Algorithms
    B2 --> B2A[Linear Regression]
    B2 --> B2B[Polynomial Regression]
    B2 --> B2C[Ridge Regression]
    
    %% Clustering Algorithms
    C1 --> C1A[K-Means]
    C1 --> C1B[Hierarchical Clustering]
    C1 --> C1C[DBSCAN]
    
    %% Dimensionality Reduction Techniques
    C2 --> C2A[PCA]
    C2 --> C2B[t-SNE]

    F[Important Concepts] --> F1[Supervised Learning]
    F1 --> F1A[Feature Engineering]
    F1 --> F1B[Overfitting/Underfitting]
    F1 --> F1C[Cross-Validation]
    F1 --> F1D[Bias-Variance Tradeoff]
    F1 --> F1E[Regularization]

    F --> F2[Unsupervised Learning]
    F2 --> F2A[Clustering Evaluation: Silhouette Score]
    F2 --> F2B[Dimensionality Reduction Benefits]
    F2 --> F2C[Finding Hidden Patterns]
    
    F --> F3[Semi-Supervised Learning]
    F3 --> F3A[Use of Small Labeled Data]
    F3 --> F3B[Leveraging Unlabeled Data]
    F3 --> F3C[Practical Applications]

    F --> F4[Reinforcement Learning]
    F4 --> F4A[Reward/Penalty Systems]
    F4 --> F4B[Exploration vs Exploitation]
    F4 --> F4C[Markov Decision Processes: MDP]

    %% Evaluation Metrics as a sub-branch of Supervised Learning
    F1 --> G[Evaluation Metrics for Supervised Learning]
    G --> G1[Classification: Accuracy, F1-score, ROC-AUC]
    G --> G2[Regression: MSE, RMSE, R-squared]

```

* Despite its name, logistic regression is used for binary classification or multi-class classification problems, not for regression tasks.
