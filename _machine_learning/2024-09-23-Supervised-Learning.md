---
title: "Supervised Learning"
excerpt: "First part of the Machine Learning course."
collection: machine_learning
permalink: /machine-learning/supervised-learning/
---

Supervised learning, also known as supervised machine learning, is a subcategory of machine learning and artificial intelligence. It is defined by its use of labeled data sets to train algorithms that to classify data or predict outcomes accurately.
For this section we will take as example predicting price for houses based on parameters such as number of bedrooms, area... 

### **Notation**
- $x$: Input data (e.g. Area). Can be vector, scalar... 
- $x^{(i)}$: X in index $i$. We don't use sub index as that is used to indicate the data on input data with more than one dimension. 
	For example. If input data is 2D vector: $x = \begin{bmatrix} area \\ bedrooms\\\end{bmatrix}$ then $x_1$ will be $area$ and $x_2$ $bedrooms$ 
- $y$: Output data (e.g. Price). 
The goal of this course and ML in general is to find a function $h$ with **parameters** weights $\theta$ that predicts $y$. 

$$
h_\theta (x) = y
$$

### Linear Regression
Following with the example of house and prices, if we assume that the function that predicts the price is a **linear function** then: 

$$
x \in R^2 , \ \ x = \begin{bmatrix} x_1 \\ x_2 \end{bmatrix}
$$

$$
h_\theta (x) = \theta_0 + \theta_1x_1 + \theta_2x_2
\,,\ \theta = \begin{bmatrix} \theta_0 \\ \theta_1\\ \theta_2 \end{bmatrix}
$$

Which can also be defined as the **inner product** of both vectors. 

$$
    h(x) = \sum_{i=0}^d \theta_i x_i = \theta^\top x\text{,}
$$

Now, given a training set, how do we pick, or learn, the parameters $\theta$?
One reasonable method seems to be to make $h(x)$ close to $y$, at least for the training examples we have. To formalize this, we will define a function that measures, for each value of the $\theta$'s, **how close the $h(x^{(i)})$'s are to the corresponding $y^{(i)}$'s.** 
We define the cost function, in this example the least-squares cost function, that gives rise to the **Ordinary Least Squares** regression model: 

$$
\begin{equation}
    J(\theta) = \frac 1 2 \sum_{i=1}^n \left( h_\theta(x^{(i)}) - y^{(i)} \right)^2\text{.}
\end{equation}
$$

### Least-Means-Square algorithm
We want to choose $θ$ so as to minimize $J(θ)$ (the cost function). To do so, lets use a search algorithm that starts with some “initial guess” for $θ$, and that repeatedly changes $θ$ to make $J(θ)$ smaller, until hopefully we converge to a value of $θ$ that minimizes $J(θ).$

For instance let's consider only one single sample (n=1). We do the gradient (derivates) of $J(\theta)$

$$ 
\nabla J(\theta) =  \nabla \frac {1}{2}(\theta^Tx^{1} -y^1 )^2 
$$

$$
\nabla J(\theta) = \frac {2}{2}(\theta^Tx^{1} -y^1 )x^1
$$

So now, the algorithms updates $\theta$ for all values of $j = 0, . . . , n.)$.

$$
\theta_j := \theta_j - \alpha(\theta^Tx^{1} -y^1 )x^1
$$

Here, $α$ is called the **learning rate**. This is a very natural algorithm that repeatedly takes a step in the direction of steepest decrease of $J$

We’d derived the LMS rule for when there was only a single training example. There are two ways to modify this method for a training set of more than one example. 
- The first is replace it **repeating until convergence**: 

$$
\theta_j := \theta_j - \alpha \sum^m_{i=1}(\theta^Tx^{i} -y^i )x^i
$$

This method looks at every example in the entire training set on every step, and is call is called **batched gradient descent**

- There is an alternative to batch gradient descent that also works very well.
	Loop
		for $i=1$ to $m$,

$$
		\theta_j := \theta_j - \alpha(y^i - h_\theta(x^i))x^i_j \ \ (for\ \ every \ \ j)
$$

In this algorithm, we repeatedly run through the training set, and each time we encounter a training example, we update the parameters according to the gradient of the error with respect to that single training example only. This algorithm is called Stochastic Gradient Descent

Whereas batch gradient descent has to scan through the entire training set before taking a single step—a costly operation if m is large—stochastic gradient descent can start making progress right away, and continues to make progress with each example it looks at. Often, stochastic gradient descent gets θ “close” to the minimum much faster than batch gradient descent.

