---
title: 'Cost Function'
date: 2025-03-05
permalink: /posts/2025/03/cross-function/
tags:
  - Machine Learning 
  - Mathematics
---

### Definition
In **machine learning (ML)**, a **cost function** (also called a loss function) is a measure of **how well the model’s predictions match the actual outcomes or targets**. The cost function gives a numerical value, the **cost** or **loss** that **quantifies** the **error** (or "cost") between the **predicted** values and the **true** values  The goal of training a machine learning model is to minimize this cost function by adjusting the model’s parameters.

#### Loss function
There's multiple way we can compute the loss function - $\mathcal{L} (y_i, \hat y_i$)- where $y_i$ is the **actual** output and $\hat y_i$ is the **predicted** output. One example could be using [Cross-entropy](https://afloresep.github.io/posts/2025/03/cross-entropy) as Loss function 

$$
\text{Loss} = -\sum_i^V \left[ y_t^{(i)} \log(\hat{y}_t^{(i)}) \right]
$$

$V$ is the number of possible classes (in language modeling, the size of your vocabulary),
$\mathbf{y} = (y^{(1)}, \dots, y^{(V)}$) is the **true** distribution of classes (often a **one-hot** vector,
 $\hat{\mathbf{y}} = (\hat{y}^{(1)}, \dots, \hat{y}^{(V)})$ is the **predicted** probability distribution from your model (often via a [softmax function](https://afloresep.github.io/posts/2025/03/softmax/)).
#### Loss over $n$ samples 
$J(\theta)$  is the overall loss function, often called the **cost function**, which aggregates the loss across all training samples.

$$
J(\theta) = \frac1{n}\sum^n_{i=1}\text{Loss}(\hat y_i, y_i)
$$
$n$ is the number of data points.
$\hat{y}$​ is the prediction for the $i$ data point.
$y_i​$ is the true label for the $i$-th data point.

Here, we are **averaging** across **multiple** data samples. Each $\text{Loss}(\hat{y}_i, y_i)$ is computed per data sample, summed across all samples and then averaged. 


The **Loss function** measures the error for a single data point. The form of this loss function varies based on the problem type.

### Inputs & Outputs
- **Input**: The cost function takes the predictions made by the model and the actual ground truth values.
- **Output**: It returns a single number representing the total error or loss.ç

### Types
Can be many. 
- Mean Squared Error or Least squares
$$
\begin{equation}
    J(\theta) = \frac 1 2 \sum_{i=1}^n \left( h_\theta(x^{(i)}) - y^{(i)} \right)^2\text{.}
\end{equation}
$$

### How to Choose a Cost Function

1. **Regression**:
    - **MSE**: Use when larger errors need more emphasis.
    - **MAE**: Use when all errors are equally important.
    - **Huber Loss**: Use when you need robustness to outliers.
2. **Classification**:
    - **Binary Cross-Entropy**: Use for binary classification.
    - **Categorical Cross-Entropy**: Use for multi-class classification.
    - **Weighted Cross-Entropy**: Use when dealing with imbalanced classes.
3. **Custom and Domain-Specific**:
    - Consider custom loss functions when you have specific requirements (e.g., different costs for false positives/negatives).
4. **Regularization**:
    - Add L1 or L2 regularization when you want to control overfitting by constraining model complexity.
