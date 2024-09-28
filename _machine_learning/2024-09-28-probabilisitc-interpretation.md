---
title: "Probabilistic Interpretation"
excerpt: "Modelling of Probability Density Functions"
collection: machine_learning
permalink: /machine-learning/probabilistic-interpretation/
---

### Main Assumptions
- **Modelling of data through their parametric probability density functions.** 
This assumption means we represent our data using mathematical functions that describe the probability of observing different values. These functions have parameters that shape their form. For example, we might use a Gaussian (normal) distribution, where the parameters are the mean and standard deviation.

- Data is independent and identically distributed (IID assumption). We collect samples X and Y, there is an assumption is that we collected independently from one and other, and each sample correspond from the same distribution.
- Maximum likelihood approach approach to identify the parameters from data

#### Modelling of Probability Density Functions
Let us assume that we observe the following data following a linear regression just like we did on the Linear regression part. Then it would by described by: 

$$
y^{(i)} = \theta^Tx^{(i)} + \epsilon^{(i)}
$$

where $y$ is the target observations, $x$ are the input/feature observations, \theta are unknown parameters (weights), and $\epsilon$ are **noise samples**. In other words, the imperfections are captured by a random variable called $\epsilon$. The index $(·)^{i}$ indicates the sample number, where $i = 1,...,m$. 
We assume that $\epsilon^{(i)}$ are Independent and identically distributed

#### How does IID help us? 
Example #1
Let's assume that the $\epsilon^{(i)}$ are distributed IID according to a **Gaussian distribution** (a.k.a. Normal distribution) with mean zero and some **variance** $\sigma^2$. 
A way of writing  just this is "$\epsilon{(i)} ~\mathcal{N}(0, \sigma^2)$"

<img width="519" alt="image" src="https://github.com/user-attachments/assets/15368b19-e1fc-4546-bf71-f18632f31a8f">{: .align-center


The **density** of $\epsilon^{(i)}$ is given by:

$$
p(\epsilon^{(i)}) = \frac{1}{\sqrt{2\pi \sigma^2}} \exp\left(-\frac{(\epsilon^{(i)})^2}{2\sigma^2}\right)
$$

where:
- $\epsilon^{(i)}$ is the noise term,
- $\sigma^2$ is the variance (with $\sigma$ being the standard deviation),

In other words, this formula describes the **likelihood** of observing **a particular value of the error $\epsilon$** following a normal distribution ($\mathcal{N}$) where the peak of the curve is at zero, and the spread is governed by $\sigma$ (standard deviation). 

Here, $\epsilon^{(i)}$ represents the error term (or noise) and is assumed to be normally distributed with mean 0 and variance.- Because $y^{(i)}$ is modeled as the sum of the linear predictor $\theta^T x^{(i)}$ (which is deterministic given $x^{(i)}$) and the normally distributed error term $\epsilon^{(i)}$, the distribution of $y^{(i)}$, conditional on $x^{(i)}$, will **also follow a normal distribution** centered around $\theta^T x^{(i)}$

$$
     p(y^{(i)} | x^{(i)}; \theta) = \frac{1}{\sqrt{2\pi \sigma}} \exp\left( -\frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2} \right)
     
$$

$p(y^{(i)} | x^{(i)}; \theta)$: indicates that this is the distribution of $y^{(i)}$ given $x^{(i)}$ and parameterized by $θ$
We should not condition on $\theta\ since is not a random variable. 

If we apply the posterior probability in all our samples we have

$$
p(y^{(i)}, ..., y^{(m)} | x^{1}, ..., x^{m}; \theta)
$$

Now, this could be a very complex formula, so here is where the Independent and identically distributed assumption simplifies. 

$$
p(y^{(i)}, ..., y^{(m)} | x^{1}, ..., x^{m}; \theta) = \prod^m_{i=1}p(y^{(i)}|x^{(i)};\theta)
$$

Under the IID assumption, the **joint** probability of observing all the data ($y$) given ($x$) and parameterized by $\theta$ is the **product** ($\prod$) of the individual probabilities, $p(y^{(i)}|x^{(i)};\theta)$, (since the data points are independent). 
{: .notice--success}

Now, if we see this function as $y$ and $x$ being fixed and $\theta$ unknown, we call it the **likelihood function** ($L(\theta$)). This function indicates **how likely are we to observe our data $y$, given our predictors $x$'s and $\theta$** and is equal to $p(y|X;\theta)$ which is a PDF.

$$
L(\theta) = L(\theta;X, \vec y) = p(\vec y | X;\theta) = \prod^m_{i=1}p(y^{(i)}|x^{(i)};\theta)
$$


Now, what is a reasonable way of choosing our best guess of the parameters $θ$?
The principal of maximum likelihood says that we should choose $\theta$ so as to make the data **as high probability as possible**. This means, we should choose $\theta$ to maximize the **likelihood** of observing our data ($y$) given our $x$'s.  

A more manageable form of the likelihood function is the *log likelihood* 

$$
\begin{align*}
L(\theta) &=  \prod^m_{i=1}p(y^{(i)}|x^{(i)};\theta)
\\
log \ \ L(\theta) &=  log \prod^m_{i=1}p(y^{(i)}|x^{(i)};\theta)
\\ 
&= log \prod_{i=1}^n\frac{1}{\sqrt{2\pi \sigma}} \exp\left( -\frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2} \right)
\\ 
&= \sum^m_{i=1}log \ \frac{1}{\sqrt{2\pi \sigma}} \exp\left( -\frac{(y^{(i)} - \theta^T x^{(i)})^2}{2\sigma^2} \right)
\\ 
&= n \ \ log\frac{1}{\sigma^2}·\frac{1}{2}\sum^n_{i=1}(y^{(i)}-\theta^Tx^{(i)})^2
\\
\end{align*}
$$

We can recognize here the $J(\theta)$ from our least-squares cost function

$$
\frac{1}{2}\sum^n_{i=1}(y^{(i)}-\theta^Tx^{(i)})^2
$$

Which means that 

**Key Concept** The **maximum likelihood** of our linear model (i.e. maximizing $L(\theta)$) is equivalent to **least squares** (i.e. minimizing $J(\theta))$
{: .notice--info}

To summarize: Under the previous probabilistic assumptions on the data, least-squares regression corresponds to finding the maximum likelihood estimate of θ. This is thus one set of assumptions under which least-squares regression can be justified as a very natural method that’s just doing maximum likelihood estimation.


