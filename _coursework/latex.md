---
title: "Image Classification with Convolutional Neural Networks"
excerpt: "A project implementing a CNN for image classification using PyTorch."
collection: computer_vision
---

## Image Classification with Convolutional Neural Networks

In this project, I implemented a Convolutional Neural Network (CNN) for image classification using PyTorch. The model was trained on the CIFAR-10 dataset and achieved an accuracy of 85% on the test set.

### Project Overview

- **Dataset**: CIFAR-10
- **Framework**: PyTorch
- **Model Architecture**: Custom CNN with 3 convolutional layers and 2 fully connected layers
- **Training**: 50 epochs using Adam optimizer

### Key Learnings

1. Importance of data augmentation in improving model generalization
2. Impact of learning rate scheduling on training stability
3. Techniques for visualizing and interpreting CNN features

### Mathematical Formulation

The convolution operation in a CNN can be expressed as:

$$(f * g)(t) = \int_{-\infty}^{\infty} f(\tau) g(t - \tau) d\tau$$

For a 2D image $I$ and a kernel $K$, the discrete convolution is:

$$(I * K)(i, j) = \sum_{m} \sum_{n} I(m, n)K(i-m, j-n)$$

The activation function we used is the Rectified Linear Unit (ReLU):

$$f(x) = \max(0, x)$$

### Loss Function

We used the cross-entropy loss function, defined as:

$$L = -\sum_{i} y_i \log(\hat{y}_i)$$

where $y_i$ is the true label and $\hat{y}_i$ is the predicted probability for class $i$.

### Future Work

- Implement transfer learning using pre-trained models like ResNet
- Explore more advanced architectures like Inception or DenseNet
- Apply the model to a real-world image classification task

[Link to GitHub Repository](#)