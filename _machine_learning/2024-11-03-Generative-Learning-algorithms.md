---
title: "Generative Learning Algorithms"
excerpt: "Introduction to Generative Learning Algorithms in ML."
collection: machine_learning
permalink: /machine-learning/generative-learning-algorithms/
---

# Generative Learning algorithms

So far, we've mainly been talking about learning algorithms that model the conditional distribution of $y$ given $x$. For instance, logistic regression modeled $p(y/x;\theta)$ as $h_\theta(x) = g(\theta^Tx)$ where $g$ is the sigmoid function. 

Consider a classification problem in which we want to learn to distinguish between elephants (y = 1) and dogs (y = 0), based on some features of an animal. Given a training set, an algorithm like logistic regression or the perceptron algorithm (basically) tries to find a straight line that separates the elephants and dogs. Then, to classify a new animal as either an elephant or a dog, it checks on which side of the decision boundary it falls, and makes its prediction accordingly

### GLM approach

In a generative learning algorithm, the focus is on modeling how the data is generated, or what the data looks like, for each possible class. This is done by learning the joint probability distribution $p(x,y)$ where:

- $x$ represents the features (like the characteristics of an animal: size, color, etc.).
- $y$ represents the label (e.g., whether the animal is a dog or an elephant).

Algorithms that try to learn $p(y/x)$ directly (such as logistic regression), or algorithms that try to learn mappings directly from the space of inputs $\mathcal{X}$ to the labels $\{0,1\}$, (such as the perceptron algorithm) are called **discriminative learning algorithms.** 

Instead, these algorithms are called **generative** learning algorithms.
The idea is to model how likely a certain set of features $x$ is for each class (dog or elephant). You would first model:

- $p(x \mid y = 0)$: The distribution of features for dogs.
- $p(x \mid y = 1)$: The distribution of features for elephants.

### Bayes' Theorem
Then we can use **Bayes' theorem** to predict which class $y$ a new observation belongs to, given the features $x$.

$$
p(y \mid x) = \frac{p(x\mid y)p(y)}{p(x)}
$$
Where: 

- $p(y∣x)$ is the **posterior probability**: the updated probability of $y$ given the observed data $x$.
- $p(x∣y)$ is the **likelihood**: the probability of observing the features $x$ given that the class is $y$ (e.g., dog or elephant).
- $p(y)$ is the **prior**: the overall probability of a particular class (e.g., how common are dogs vs. elephants).
- $p(x)$ is the marginal likelihood: the overall probability of observing the features $x$, summed over all possible values of $y$.

Here, the denominator is given by $p(x) = p(x\mid y = 1)p(y = 1) + p(x\mid y = 0)p(y = 0)$ (you should be able to verify that this is true from the standard properties of probabilities), and thus can also be expressed in terms of the quantities $p(x|y)$ and $p(y)$  that we’ve learned. 

$$
\text{arg max}_y p(y \mid x) = \text{arg max} \frac {p(x \mid y) p(y)}{p(x)}
$$
Actually, if were calculating $p(y \mid x)$ in order to make a prediction, then we don’t actually need to calculate the denominator, since $p(x)$ is a scalar is only changing the magnitude (height of the posterior) but we're taking the argument. So

$$
\text{arg max}_y p(y \mid x) = \text{arg max} p(x \mid y) p(y)
$$

## Gaussian discriminant analysis

The first generative learning algorithm that we’ll look at is Gaussian discriminant analysis (GDA). In this model, we’ll assume that $p(x/y)$ is distributed according to a multivariate normal distribution. Lets talk briefly about the properties of multivariate normal distributions before moving on to the GDA model itself.

### The multivariate normal distribution