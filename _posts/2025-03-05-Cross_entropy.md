---
title: 'Cross entropy'
date: 2025-03-05
permalink: /posts/2025/03/cross-entropy/
tags:
  - Machine Learning 
  - Mathematics
---

### What is Cross-Entropy?
Cross-entropy is one of many possible [cost function](https://afloresep.github.io/posts/2025/03/cost-function) that we might consider choosing for our model and is typically used in **Categorical Classification**. This means, a model that aims to predict whether our input data is one in a finite size possible classes.

For example, if we want to predict the next letter in a word, cross-entropy could be use to measure the loss between the letter predicted -which has to be one of 28 letters in our vocabulary, these are our possible classes- and the actual letter that proceeds.

**Why entropy?**

Well, this comes from an **information theory** perspective and is not so much related to the physic concept of disorder. Entropy in this context is more related to measuring the average amount of **information** _or bits_, this can be seen as uncertainty as well, inherent in a probability distribution. For more information on this topic outside ML see [this](https://en.wikipedia.org/wiki/Cross-entropy) 

*This refers exclusively to categorical cross-entropy i.e. Cross entropy for discrete probability distributions of $p$ and $q$ with the same support*

### Formal definition
Formally, the cross entropy is used to measure the difference between the probability distribution of $p$  or the **true probability distribution** (the actual distribution of data or outcomes) and $q$, the **predicted probability distribution** (for example, from a model)

#### Cross-entropy as Loss function
Here, the categorical cross-entropy as loss function $\mathcal{L}$ is

$$
\mathcal{L}(\hat{\mathbf{y}}, \mathbf{y}) = - \sum_{i=1}^{V} y^{(i)} \log(\hat{y}^{(i)})
$$

Where: 

$\mathbf{y} \in \mathbb{R}^V$ is a one-hot vector representing the true class (of $V$ possible classes/tokens).

$\hat{\mathbf{y}} \in \mathbb{R}^V$ is the predicted probability distribution (often obtained via a [softmax](https://afloresep.github.io/posts/2025/03/softmax/) layer).

For example, if we are trying to predict the next letter in a word or a phrase, $V$ would be the alphabet, hence $\mathbf{y} \in \mathbb{R}^{26}$, and if our predicted next letter is _'c'_ then our vector $\hat y$ would be

$$
\hat{y} = \begin{bmatrix} 0 \\ 0 \\ 1 \\ \vdots \\ 0 \\ 0 \end{bmatrix}
$$

There's a reason why we would one to use a one-hot vector representation of our distribution classes that will be explained in the section [[#Intuitive explanation of the mathematical expression]]
#### Cross-entropy as Cost function
As we have seen in other post, the Cross entropy serves as a [[cost function]] which can be defined as:

$$
J(\theta) = \frac1{n}\sum^n_{i=1}\mathcal{L}(\hat y_i, y_i)
$$

Where $J(\theta)$ represents the averaged ($1/n$ ) sum ($\sum_i^n$) of all computed Losses ($\mathcal{L}$) between the predicted value ($\hat y$) and the actual value ($y$) for a set of $n$ samples.

If we integrate the $\mathcal{L}$ that we saw previously into our Cost Function then we get: 

$$
J(\theta) = \frac{1}{n} \sum_{j=1}^{n} \Bigl( -\sum_{i=1}^{V} y_j^{(i)} \log(\hat{y}_j^{(i)}) \Bigr)
$$

Again, this represents the average loss across multiple samples

### Intuitive explanation of the mathematical expression and relation to the log-likelihoood

I always try to understand the why of a mathematical expression.  In the case of our  . 
Let's do it in parts for our cost function ($J(\theta)$):

$$
J(\theta) = \frac{1}{n} \sum_{j=1}^{n} \Bigl( -\sum_{i=1}^{V} y_j^{(i)} \log(\hat{y}_j^{(i)}) \Bigr)
$$

## 1. $\frac{1}{n} \sum_{j=1}^{n} (·)$
We already explained this part, the loss function $\mathcal{L}$ is calculated between two points. For our cost function we compute the loss function for every point in our dataset of size $n$. So all this does is sum every loss for points $j$ in $n$ and average the values $1/n$ 

## 2. $-\sum_{i=1}^{V} (·)$
We sum because we want to integrate the loss in **all** classes ($i \in V$) for every point ($j \in n$) in our dataset. 
As we will see later, in cases where the **true label** is represented as one-hot encoded vector it really does not make a difference to sum or not -as the product will be 0 for those clases different from the true label-, but for soft targets this formula is applicable. Therefore, this is the **generalized** mathematical **expression**. 

**Why using the negative sign?** 

Taking a step back. Why are we doing this for? Well, because we want to find the best weights or **parameters** for our model that is trying to classify our input data. 
In order to do so, we see the [[likelihood function]] as a function of how different values for these **parameters** ($\theta$) explain our observed data ($y$). 

Okay. So which $\theta$ do we choose? Obviously, we want to **maximize** this log-likelihood function (we use log because it's easier mathematically). Why? Well, because we want to get the parameters that are *more* likely to explain our observed data. However, from a mathematical  optimization perspective, it's easier to deal with **minimization** problems rather than **maximization** problems, so instead of trying to optimize the **maximization** our log-likelihood, we try to **minimize** our cost function. For this reason, we  look at our **cost function** as the **negative** log-likelihood, we are essentially converting log-likelihood it into a minimization task. 

**Important concept**
In essence, the cost function is a reformulation of the log-likelihood that fits into a minimization framework. As the cost decreases, the log-likelihood increases. 
{: .notice--info}

The **maximum likelihood** of our model, which is to say maximize $L(\theta)$ is the equivalent to minimizing $J(\theta))$
{: .notice--info}

## 3. $y_j^{(i)} \log(\hat{y}_j^{(i)})$
This part acts Like an “Indicator”

- If $y^{(i)} = 1$, then $-\sum_i y^{(i)} \log(\hat{y}^{(i)})$ **picks out** $-\log(\hat{y}^{(i)})$ for that specific $i$.

- If $y^{(i)} = 0$, the term $0 \times \log(\hat{y}^{(i)})$ contributes nothing.

Hence for the single-correct-class scenario, we only penalize the log probability of the correct class. It effectively focuses the loss on “the probability you assigned to the ground truth token.”