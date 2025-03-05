---
title: 'Likelihood Function simply explained'
date: 2025-02-27
permalink: /posts/2025/02/likelihood-function/
tags:
  - Bayes
  - Mathematics
---

It represents the probability of observing the given data, given a particular set of parameter values. It plays a crucial role in [Bayes](https://afloresep.github.io/posts/2025/2/intro-bayes/)

### Definition:
Given a dataset $D \{x_1, x_2, x_3 ..., x_n \}$ and a parameter ($\theta$), the likelihood function is: 
$L(\theta) = P(D \vert \theta$), which tells us the **probability of the observed data given a specific parameter**

It can also be written like this:

$$
L(θ; y, X) = p(y \vert  X; θ)
$$

Where:
- $L$ is the likelihood function
- $θ$ is the set of parameters we're trying to estimate
- $y$ is our observed dependent variable
- $X$ is our matrix of independent variables

If we have multiple observations, say a dataset with $n$ examples $(x_1, y_1), (x_2, y_2), \dots, (x_n, y_n)$ the likelihood function is the **product of the probabilities** of all the observed outcomes $y_i$​ given their corresponding inputs $x_i$​. This is because the observations are typically assumed to be independent (Independent and identically distributed) so we multiply the individual probabilities.


The likelihood function $L(\theta)$ is:

$$
L(\theta) = \prod_{i=1}^n p(y_i \vert  x_i; \theta)
$$

Substitute $p(y_i \vert  x_i; \theta)$ for each observation:

$$
L(\theta) = \prod_{i=1}^n \left[ (h_\theta(x_i))^{y_i} (1 - h_\theta(x_i))^{1 - y_i} \right]
$$

This gives the likelihood of the entire dataset, which is a function of the model parameters $\theta$. In other words, it tells us **how likely the entire set of observations** $\{y_1, y_2, \dots, y_n\}$ is given the inputs $\{x_1, x_2, \dots, x_n\}$ and the parameters $\theta$.


### Intuitive example of what the Likelihood function is

Let's say we have a coin that we will flip $n$ amount of times, with a probability of seeing heads of $\theta$ and a result (observed data -$y$-) of $k$ times seeing heads after flipping. 

Let's say the coin is balanced ($\theta = 0.5$) i.e. equal chance of getting heads or tails but we don't know (in other words, we have to guess which $\theta$ is the more likely given our observations). And our observations are that we flipped the coin 10 times, where we got 6 heads ($k$) and 4 tails ($n - k$). Since we don't know that the coin in balanced ($\theta$) we ask the question: *Given that I threw the coin 10 times and 60% of the time it was heads, which balance (i.e. probability of seeing heads) is more likely?* 
Well, intuitively the answer is $\theta = 0.6$. Since we saw 60% of times heads, it makes sense that the coins is balanced in such a way that it gives 60 percent of times a head. A likelihood function then is nothing but how this likelihood -*how well our parameter fits our observed data*- changes for different values of $\theta$. In this example $\theta = 0.6$ is what we call [[maximum likelihood]] estimation or **MLE**. 




### 3. Why Do We Maximize the Likelihood?

In logistic regression, the goal is to find the parameters $\theta$ that maximize the likelihood function, i.e., the values of $\theta$ that make the observed outcomes most probable. This process is called **[[maximum likelihood]] estimation (MLE)**.

### 4. The Log-Likelihood Function

Instead of working with the product in the likelihood function, it’s more convenient (and numerically stable) to work with the **logarithm** of the likelihood function. This is called the **log-likelihood** function, and it simplifies the math because the logarithm turns products into sums.

Taking the logarithm of the likelihood function:

$$
\log L(\theta) = \log \left( \prod_{i=1}^n \left[ (h_\theta(x_i))^{y_i} (1 - h_\theta(x_i))^{1 - y_i} \right] \right)
$$

By applying the logarithm, this becomes:

$$
\log L(\theta) = \sum_{i=1}^n \left[ y_i \log h_\theta(x_i) + (1 - y_i) \log (1 - h_\theta(x_i)) \right]
$$

This is the **log-likelihood function**.

- When $y_i = 1$, the term $y_i \log h_\theta(x_i)$ dominates (since $\log(1 - h_\theta(x_i))$ disappears because of the $1 - y_i$ term).
- When $y_i = 0$, the term $(1 - y_i) \log (1 - h_\theta(x_i))$ dominates.

Thus, the log-likelihood captures how well the model fits both the $y_i = 1$ and $y_i = 0$ cases across all data points.

### 5. Maximizing the Log-Likelihood

To find the best parameters $\theta$, we want to maximize the log-likelihood function. This is done using optimization techniques such as **gradient ascent** or, more commonly in practice, **gradient descent** (since it's easier to minimize the negative of the log-likelihood, often referred to as the **cost function**).

### Summary of the Log-Likelihood:

$$
\log L(\theta) = \sum_{i=1}^n \left[ y_i \log h_\theta(x_i) + (1 - y_i) \log (1 - h_\theta(x_i)) \right]
$$

- **$h_\theta(x_i)$** is the predicted probability that $y_i = 1$ based on the input $x_i$.
- The log-likelihood measures how well the model with parameters $\theta$ explains the observed outcomes.
- By maximizing the log-likelihood, we are finding the values of $\theta$ that make the observed data most probable under the model.

In logistic regression, this leads to finding the best-fitting logistic function that separates the classes $y = 0$ and $y = 1$.
