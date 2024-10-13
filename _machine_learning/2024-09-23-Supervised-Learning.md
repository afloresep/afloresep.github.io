---
title: "Linear Regression"
excerpt: "First part of the Machine Learning course. Introduction and Linear Regression"
collection: machine_learning
permalink: /machine-learning/linear-regression/
---

Supervised learning, also known as supervised machine learning, is a subcategory of machine learning and artificial intelligence. It is defined by its use of labeled data sets to train algorithms that to classify data or predict outcomes accurately.
For this section we will take as example predicting price for houses based on parameters such as number of bedrooms, area... 

### **Notation**
- $x$: Input data (e.g. Area). Can be vector, scalar... 
- $x^{(i)}$: X in index $i$. We don't use sub index as that is used to indicate the data on input data with more than one dimension. 
	For example. If input data is 2D vector: 

$$
x =  
\begin{bmatrix}
area \\
bedroom \\
\end{bmatrix}
 $$
 
  then $x_1$ will be $area$ and $x_2$ $bedrooms$ 
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

There is an alternative to batch gradient descent that also works very well.

Loop for $i=1$ to $m$,
			
$$
\theta_j := \theta_j - \alpha(y^i - h_\theta(x^i))x^i_j \ \ (for\ \ every \ \ j)  
$$
		
In this algorithm, we repeatedly run through the training set, and each time we encounter a training example, we update the parameters according to the gradient of the error with respect to that single training example only. This algorithm is called Stochastic Gradient Descent

Whereas batch gradient descent has to scan through the entire training set before taking a single step—a costly operation if m is large—stochastic gradient descent can start making progress right away, and continues to make progress with each example it looks at. Often, stochastic gradient descent gets θ “close” to the minimum much faster than batch gradient descent.

### Matrix derivatives
For a function $f : \mathbb{R}^{m \times n}\rightarrow \mathbb{R}$  mapping from *m-by-n* matrices to the real numbers, we define the derivate of **$f$ with respect to A** to be:

$$
\begin{equation}
    \nabla_A f(A) = \begin{bmatrix}
        \frac{\partial f}{\partial A_{11}} & \cdots & \frac{\partial f}{\partial A_{1d}} \\
        \vdots & \ddots & \vdots \\
        \frac{\partial f}{\partial A_{n1}} & \cdots & \frac{\partial f}{\partial A_{nd}} \\
    \end{bmatrix}
\end{equation}
$$


**Definition** $$\nabla_A f(A)$$ 
the gradient ($\nabla$) of the function ($f(A)$) with respect to all the entries of matrix ($A$)
{: .notice--info}

Thus, the gradient $\nabla_Af(A)$ is itself an *m*-by-*n* matrix, whose (*i, j*)-element is ${\partial f}/{\partial A_{ij}}$

We also introduce the **trace operator**, written “tr.” For an n-by-n (square) matrix A, the trace of A is defined to be the sum of its diagonal entries:

$$
trA = \sum^n_{i=1}A_{ii}
$$

The trace operator has the property that for two matrices A and B such that AB is square, we have that $trAB = trBA$ 

**Important definition** $trAB = trBA$
{: .notice--info}

Let's start by defining what matrix multiplication is for $A\in \mathbb{R}^{m\times n} \ , \ B\in \mathbb{R}^{n\times m}$:

$$
(AB)_{ik}  = \sum_{i=1}^m A_{ij}B_{jk}
$$

And according to our previous definition of *trace*:

$$
\begin{align*}
trAB &= \sum^m_{i=1}(AB)_{ii} = \sum^m_{i=1}  \sum_{i=1}^n A_{ij}B_{ji}
\\
trBA &= \sum^n_{i=1}(BA)_{ii} = \sum^n_{p=1}  \sum_{q=1}^m A_{pq}B_{qp}
\end{align*}
$$

Other important definitions are: 

$$tr A = trA^T$$

$$trAB = trBA$$

$$tr0ABC = trCAB = trBCA$$

$$tr(A+B) = trA + trB = traA = atrA)$$

An from this we state without proof some facts of matrix derivatives: 

$$\nabla_A trAB = B^T$$

$$\nabla_{A^T}f(A) = (\nabla_Af(A))^T$$

$$\nabla_AtrABA^TC = CAB + C^TAB^T$$

$$\nabla_A |A| = |A|(A^{-1})^T$$

The last equation (4) applies **only to non-singular square matrices A**, where $\lvert A \rvert $ denotes the determinant of A

#### Matrix derivatives for Least squares
Armed with the tools of matrix derivatives, let us now proceed to find in closed-form the value of $θ$ that minimizes $J(θ)$. We begin by re-writing $J$ in matrix-vectorial notation.

Giving a training set, define the design matrix X to be the m-by-n matrix (actually m-by-n + 1, if we include the intercept term) that contains the training examples’ input values (area, number of beds, bathrooms...) in its rows:

$$
\begin{equation}
X = 
\begin{bmatrix} 
-(x^{(1)})^T- \\ 
-(x^{(2)})^T- \\ 
\vdots \\ 
-(x^{(m)})^T-
\end{bmatrix}
\end{equation}
$$

Also, let $y$ be the *m*-dimensional vector containing all the target values from the training set: 

$$
\begin{equation}
y =  
\begin{bmatrix} 
y^{(1)} \\ 
\vec y^{(2)} \\
: \\ 
y^{(m)}
\end{bmatrix}
\end{equation}
$$

We remember that our error (how far the predictions were from the actual value) was
$(h_\theta(x) - \vec y)$ and $h_\theta(x) = \sum_{i=1}^d \theta_i x_i = \theta^\top x$ (revisit inner product)

So now with matrices we can write $(h_\theta(x) -y)$ as $X \theta - \vec y$; 

$$\begin{align*}
    \  X \theta - \vec y &= \begin{bmatrix}
        (x^{(1)})^\top \theta \\
        \vdots \\
        (x^{(m)})^\top \theta
    \end{bmatrix}
    -
    \begin{bmatrix}
        y^{(1)} \\    
        \vdots \\
        y^{(m)}    
    \end{bmatrix}\\
    &= \begin{bmatrix}
        h_\theta(x^{(1)}) - y^{(1)} \\
        \vdots \\
        h_\theta(x^{(m)}) - y^{(m)}
    \end{bmatrix}\text{.}
\end{align*}
$$

And therefore our cost function $J(\theta)$: 

$$
J(\theta) = \frac {1}2\sum_{i=1}^m(h_\theta(x^{(i)})-y^{(i)})^2 
$$

now can be written as :

$$
J(\theta) = \frac {1}2\sum_{i=1}^m(X \theta -y^{(i)})^2 
$$


If we simplify $X \theta -y^{(i)}$ as $z$ is easier to see that using the following property: 

$$
z^Tz = \sum_i z_i^2
$$

then 

$$
\frac {1}{2}(X\theta - \vec y )^T(X\theta - \vec y)
$$

Finally, ==to minimize $J$==, let's find its derivative with respect to $\theta$ ($\nabla_\theta J(\theta)$) 

$$
\begin{align*}
\nabla_\theta J(\theta) &= \nabla_\theta \frac {1}{2}(X\theta - \vec y )^T(X\theta - \vec y) \\
&=  \frac {1}{2}\nabla_\theta (X\theta - \vec y )^T(X\theta - \vec y)
\end{align*}
$$

When we think about it $(X\theta - \vec y )^T(X\theta - \vec y)$ is just a **SCALAR**, therefore taking the $tr a = a$ equation we may say:

$$
\begin{align*}
\nabla_\theta J(\theta) &= \frac {1}{2}\nabla_\theta \ \ tr(X\theta - \vec y )^T(X\theta - \vec y) 
\\ &=  \frac {1}{2}\nabla_\theta \ \ tr (\theta^TX^TX\theta - \theta^TX^T\vec y - \vec y^TX\theta + \vec y^T\vec y)
\\
\end{align*}
$$

Notice:
- $\vec y^T\vec y$ doesn't have any $\theta$ so we can take it out
- $\theta^TX^T\vec y - \vec y^TX\theta = 2\theta^TX^T\vec y$

Hence:

$$
\begin{align*}
\nabla_\theta J(\theta) &= \frac {1}{2}\nabla_\theta \ \ tr (\theta^TX^TX\theta - 2( \theta^TX^T\vec y))
\\
\end{align*}
$$

Now we use the fact that $trA = trAT$, and Equation (5) with $AT = θ$, $B = B^T = X^TX$, and $C = I$, and Equation ($\nabla_A trAB = B^T$)., we get:

$$
X^TX\theta - X^T\vec y 
$$

To minimize *J*, we set its derivatives to zero, and obtain the **normal equations**

$$
X^TX\theta = X^T\vec y
$$


