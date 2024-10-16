---
title: "Generalized Linear Models"
excerpt: "Introduction to GMLs in ML."
collection: machine_learning
permalink: /machine-learning/generalized-linear-models/
---


In our exploration of **Classification** and **UniBe/Machine Learning/Linear Regression|Linear Regression,** we've encountered methods like logistic regression and ordinary least squares regression. These are not isolated techniques but are part of a broader and more powerful framework known as **Generalized Linear Models (GLMs)**. GLMs unify various statistical models under a common umbrella, allowing us to model different types of data (binary, count, continuous) using a consistent approach. Understanding GLMs enhances our ability to select appropriate models for diverse machine learning tasks and deepens our comprehension of the underlying statistical principles.
## Exponential Family
At the heart of GLMs is the **Exponential Family** of probability distributions. A probability distribution belongs to the exponential family if it can be expressed in the following canonical form:

$$
\text{(Eq.1)} \quad p(y; \eta) = b(y) \exp\left( \eta^T T(y) - a(\eta) \right)
$$
**Where:**

$y$ is the random variable or response variable.
$\eta$ is the **natural parameter** (also known as the **canonical parameter**). Determines the shape or characteristics of the distribution.
$T(y)$ is the **sufficient statistic** (often $T(y) = y$).  Contains all the information needed from the data regarding the parameter $\eta$.
$a(\eta)$ is the **log-partition function**, ensuring the distribution sums or integrates to one. Acts as a normalizing constant to ensure the probabilities sum to one.
$b(y)$ is the **base measure**, often a function involving $y$ that ensures the distribution is properly scaled.

This form is powerful because it encompasses many common distributions (Gaussian, Bernoulli, Poisson, etc.) and provides a unified way to work with them.

For a given distribution, we say it's part of the Exponential family if there is a choice of $T$, $a$ and $b$ so that the Eq.1. becomes exactly the class of the given distribution. 

### Bernoulli distribution

The **Bernoulli distribution** models binary outcomes ($y \in \{0, 1\}$). Its probability mass function (PMF) is:

$$
p(y; \phi) = \phi^y (1 - \phi)^{1 - y}
$$

We aim to express this PMF in the form of Eq.1.

1. **Apply logarithms and exponentials:**

$$
\begin{align*}
p(y;\phi) &= \phi^y (1 - \phi)^{1-y} \\
&= \text{exp}(y·log(\phi) + (1-y)· log(1-\phi)) \\
&= \text{exp}\left(\left(log\left(\frac{\phi}{1-\phi}\right)\right)y+log(1-\phi)\right)
\end{align*}
$$

Thus, identifying the components:

   **Natural Parameter ($\eta$)**:

$$
     \eta = \ln \left( \frac{\phi}{1 - \phi} \right)
$$

This is the log-odds or logit function.

**Sufficient Statistic ($T(y)$)**:

$$
     T(y) = y
$$

**Log-Partition Function ($a(\eta)$)**:

$$
     a(\eta) = -\ln(1 - \phi) = \ln\left(1 + e^\eta\right)
$$

Since $\phi = \frac{e^\eta}{1 + e^\eta}$, we have $1 - \phi = \frac{1}{1 + e^\eta}$.

**Base Measure ($b(y)$)**:

$$
     b(y) = 1
$$

Thus, the Bernoulli distribution fits into the exponential family framework.

### Gaussian distribution

Consider the **Gaussian distribution** with mean $\mu$ and variance $\sigma^2$:

$$
p(y; \mu) = \frac{1}{\sqrt{2\pi \sigma^2}} \exp\left( -\frac{(y - \mu)^2}{2\sigma^2} \right)
$$

Since the value of $\sigma$ had no effect on our final choice of $\theta$ and $h_{\theta}(x)$, we choose an arbitrary value of $\sigma^2 = 1$ to simplify the derivation  

$$
\begin{align*}
p(y;\mu) &= \frac{1}{\sqrt{2\pi}}\text{exp}\left( -\frac{1}{2}(y-\mu)^2\right)\\ 
&= \frac{1}{\sqrt{2\pi}}\text{exp}\left(-\frac{1}{2}y^2\right) · \text{exp} \left( \mu y-\frac{1}{2}\mu^2\right)
\end{align*}
$$

Thus we see that Gaussian is in the exponential family with 

$$
\begin{align*}
\eta &= \mu \\
T(y) &= y \\
a(\eta) &= \mu^2 / 2 \\
&= \eta^2/2 \\
b(y) &= \frac{1}{\sqrt{2\pi}}\text{exp}\left(\frac{-y^2}{2} \right)
\end{align*}
$$

### Constructing GLMs

More generally, consider a classification or regression problem where we would like to predict the value of some random variable $y$ as a function of $x$. To derive a GLM for this problem, we will make the following **three assumption**s about the conditional distribution of $y$ given $x$ and about our model:

1. **Response Variable Distribution**
$p(y|x;\theta) \in \text{Exp. family}$. I.e., given $x$ and $θ$, the distribution of $y$ follows some exponential family distribution, with parameter $η$.

2. **Modelling the Expected Value**
Given $x$, our goal is to predict the expected value of $T(y)$ given $x$. In most of our examples, we will have $T(y) = y$, so this means we would like the prediction $h(x)$ output by our learned hypothesis h to satisfy $h(x) = E[y|x]$.

3. **Linear Relationship of Natural Parameter**
The natural parameter $\eta$ and the inputs $x$ are related linearly: $\eta = \theta^Tx$. Hence the name, Generalized **Linear** models. 


### The Link Function

The relationship between the expected value $h_\theta(x)$ and the linear predictor $\theta^T x$ is mediated by a **link function** $g$:

$$
g(h_\theta(x)) = \theta^T x
$$

Alternatively, the expected value is related to the linear predictor via the **inverse link function** $g^{-1}$:

$$
h_\theta(x) = g^{-1}(\theta^T x)
$$

The choice of the link function depends on the distribution of $y$ and ensures that the predicted values are within the valid range.

### Examples of GLMs
#### Ordinary least squares as GLMs
To show that least-squares is a special case of the GLM family, consider the setting where the target variable $y$ is continuous, and we model the conditional distribution of $y$ given $x$ as Gaussian $\mathcal{N}(\mu, \sigma^2)$.

Given assumption 2: 

$$
h_\theta(x) = E[y|x]
$$
and since $y|x;\theta \backsim \mathcal{N}(\mu, \sigma^2)$ and so its expected value is given by $\mu$ then:

$$
\begin{align*}
h_\theta(x) &= \mu \\
&= \eta \\
&= \theta^Tx
\end{align*}
$$

The rest of the equalities follows from Assumption 3 and our earlier derivation showing that $\mu =  \eta$ in the formulation of the Gaussian as exponential family distribution)


#### Logistic regression as GLMs
We now consider logistic regression. Here we are interested in **binary classification**, so $y \in$ {0,1}. Given that $y$ is binary-valued, it therefore seems natural to choose the Bernoulli family of distributions to model the conditional distribution of $y$ given $x$. In our formulation of the Bernoulli distribution as an exponential family distribution, we had

$$
\phi = \frac{1}{1+ e^{-\eta}}
$$

So if $y/x$;\theta \backsim \text{Bernoulli}(\phi)$, then by the first assumption and following a similar derivation as the one for ordinary least squares, we get: 

$$
\begin{align*}
h_\theta(x) &= E[y|x;\theta] \\
&= \phi \\
&= 1/(1+e^{-\eta}) \\
&= 1/(1+e^{-\theta^Tx})
\end{align*}
$$


### Example: Poisson Regression

**Context**: Modeling count data ($y \in \{0, 1, 2, \ldots\}$).

**Poisson Distribution**:

$$
p(y; \lambda) = \frac{\lambda^y e^{-\lambda}}{y!}
$$

**Expressing in Exponential Family Form**:

1. **Rewriting**:

   $$
   p(y; \lambda) = \exp\left( y \ln \lambda - \lambda - \ln y! \right)
   $$

2. **Identifying Components**:

   **Natural Parameter**: $\eta = \ln \lambda$
   **Sufficient Statistic**: $T(y) = y$
   **Log-Partition Function**: $a(\eta) = \lambda = e^\eta$
   **Base Measure**: $b(y) = \frac{1}{y!}$

**Expected Value**:

$$
E[y] = a'(\eta) = e^\eta = \lambda
$$

**Link Function**:

Canonical link: $\eta = \ln E[y]$
Inverse link: $E[y] = e^{\theta^T x}$

**Conclusion**: Poisson regression is a GLM with a log link function.

### Parameter Estimation via Maximum Likelihood

The parameters $\theta$ in a GLM are estimated by maximizing the likelihood function:

$$
L(\theta) = \prod_{i=1}^n p(y^{(i)} | x^{(i)}; \theta)
$$

**Log-Likelihood**:

$$
\ell(\theta) = \sum_{i=1}^n \left( \eta^{(i)} T(y^{(i)}) - a(\eta^{(i)}) + \ln b(y^{(i)}) \right)
$$

**Gradient and Hessian**:

- **Gradient**:

  $$
  \nabla \ell(\theta) = \sum_{i=1}^n \left( T(y^{(i)}) - a'(\eta^{(i)}) \right) x^{(i)}
  $$

- **Hessian**:

  $$
  H(\theta) = -\sum_{i=1}^n a''(\eta^{(i)}) x^{(i)} x^{(i)T}
  $$
  
**Iteratively Reweighted Least Squares (IRLS)**:

An efficient algorithm for maximizing $\ell(\theta)$:

1. **Initialize** $\theta^{(0)}$.

2. **Iterate** until convergence:

Compute weights:
 $$
     w^{(i)} = a''(\eta^{(i)})
     $$

   Update $\theta$:     

$$
     \theta^{(t+1)} = \left( X^T W X \right)^{-1} X^T W z
$$

   Where:

 $X$ is the design matrix.
 $W$ is a diagonal matrix of weights $w^{(i)}$.
 $z$ is the adjusted dependent variable.

