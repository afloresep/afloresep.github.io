---
title: "Fundamental definitions"
excerpt: "First part of the Applied Optimization course."
collection: applied_optimization
permalink: /applied-optimization/fundamental-definitions/
---

## Fundamental definitions 

A mathematical optimization problem, or just optimization problem, has the form: 


> [!NOTE]
> Definition of a optimization problem
> minimize $f_0(x)$
> subject to $f_i(x) \leq b_i \,, \ i =1,...,m$

Where:
- (vector) $x = (x1, ...,x_n)$ is the optimization variable of the problem
- (function) $f_0 : R^n \rightarrow R$ is the objective function
- (function) $f_i : R^n \rightarrow R, \ \ i = 1,...,m$ are the (inequality) constraint functions
- (constants) $b_1,...,b_m$ are the limits, or bounds, for the constraints

A vector $x^{\*}$ is called optimal, or a solution of the problem, if it has the smallest objective value among all vectors that satisfy the constraints. In other words, solution $x^\*$ is a point of the feasible set where $f_0$ gets minimal:  $x^\* \in F \ \ and \ \ f_0(x^\*) \leq f_0(z), \ \ \forall z \in F$

### Classes of Optimization Problems

#### Linear Program (LP)
As an important example, the optimization problem (1.1) is called a linear program if the objective and constraint functions $f_0, . . . , f_m$ are linear. i.e. satisfy: 
- all $f_i$ and $hi$ are affine functions (linear + constant)
- linear function satisfies $f(\alpha a + \beta b) = \alpha f(a) + \beta f(b), \ \ \forall \alpha , \beta \in R$
- linear function has form $f(x) = a^T x = \sum ^n_{j=1} a_j x_j$
- affine function has form $f(x) = a^T x + b= \sum ^n_{j=1} a_j x_j +b$

#### Non-Linear Program (NLP)
- $f_i$ and $h_j$ can be arbitrarily non-linear

#### Convex Optimization Problem 
- $f_i$ are convex (this includes objective functions and inequality functions) and $h_j$ are affine. 
- convex function satisfies:

$$
f(\alpha a + \beta b) \leq \alpha f(a) + \beta f(b), \ \ \forall \alpha + \beta = 1, \alpha \geq 0, \beta \geq 0
$$

It can be seen as generalization of linear function, since it satisfies a similar function as the linear program (additivity) but instead of just = we have $\leq$

### Applications
The optimization problem is an abstraction of the problem of making the best possible choice of a vector in $R^n$ from a set of candidate choices. The variable $x$ represents the choice made; the constraints $f_i(x) \leq b_i$ represent firm requirements or specifications that limit the possible choices, and the objective value $f_0(x)$ represents the cost of choosing $x$. 

#### Example - Portfolio Optimization
Task: Find the best way to invest some capital in a set of $n$ assets
Variables ($x_i$): investment in the $i^{th}$ asset 
Constraints: Limit on budget, non-negative investment, minimum acceptable value of ROI...
Objectives: maximize expected return or minimize risk 

General optimization problems are very difficult to solve, today only is possible for small problems ($n, m \leq 50$). Methods typically involve some compromise, e.g. very long computation time or not always finding the solution
However, exceptions that be solved efficiently and reliably are: 
- Linear least-squares problems
- Linear programming problems
- Convex optimization problems 

#### LINEAR LEAST-SQUARES (LLS) 
minimize $||A_x - b||^2_2$ . Which can be seen as $({l^2 norm})^2$  (removing square root of **L2-Norm**) 
(matrix) $A_x \in R^{k\times n}$ 
(vector) $b \in R^{k}$

Analytic solution: $x* = (A^TA)^{-1} A^T b$ (Invert the matrix A transpose A multiplied by A transpose b) 
However, in practice we would never do that. Instead we would solve the corresponding linear system: 

$$
x* = (A^TA)^{-1} A^T b \ \ \rightarrow \ \ A^T Ax^* = A^Tb
$$

The computational time is proportional to $n^2 k$.
Many problems can be modelled by "tuning" with weights, regularization...

#### Finding the Analytic solution

$$
f_0(x) = ||Ax - b||_2^2 = (Ax - b)^2_1 + (Ax -b)^2_2 + ... + (Ax - b)^2_n
$$

(Definition of L2-Norm)
Which can also be written as: 

$$
f_0(x) = ||Ax - b||_2^2 =(A_x - b)^T (Ax - b)
$$

$$
\begin{align} 
f_0(x) &= [(Ax - b)^T (Ax - b)] = [(A^T x^T - b^T) (Ax - b)] \\ &= [x^T A^T A x - x^T A^T b - b^T A x + b^T b] \\&= x^TA^TAx - 2x^TA^Tb + b^Tb
\end{align} 
$$


> [!NOTE]
> Property of matrix transposition
>For any matrices $P$ and $Q$, $(PQ)^T = Q^T P^T$
>In this example: 
>$(A_x - b)^T = (A^T x^T - b^T)$
>For any matrix $M$, $M$ and $M^T$ are equal when $M$ is 1x1 (a scalar).
>$(x^T A^T b)^T = b^T (A^T)^T x = b^T A x$


Now we apply the derivative and setting this equal to zero

$$
\begin{align} \nabla_x f_0(x) &= \nabla_x [x^TA^TAx - 2x^TA^Tb + b^Tb] \end{align}
$$

$\nabla (x^T A^T Ax)$ = $2A^T A x$
To find the derivative, we need to use the rules of matrix calculus.


> [!NOTE]
> Key Rule for solving this part:  
> $\frac {\partial}{\partial x}(x^T B x) =  (B + B^T)x$


In our case, $B = A^T A$. So we have:

$$
\frac {\partial}{\partial x}(A^TAx + (A^TA)^Tx)
$$

Now, let's look at $(A^T A)^T:$

$$
(A^T A)^T = A^T (A^T)^T = A^T A
$$

- This is because $A^TA$ is symmetric.
- Substituting this back in: 

$$ 
\frac {\partial}{\partial x}(A^TAx + (A^TA)^Tx) = (A^T A + A^T A)x = 2A^T A x
$$

$\nabla (2x^TA^Tb$ ) = $2(\nabla A^Tb(x^T)) = 2A^Tb$
$\nabla b^Tb$ = 0 (scalar)
Therefore, we have: 

$$
\begin{align}
\frac {\partial}{\partial x} f_0(x) &= \nabla_x [x^TA^TAx - 2x^TA^Tb + b^Tb]  = 0 \\ &= 2A^TAx - 2A^Tb = 0
\\ &= A^T Ax = A^Tb 
\\ &= (A^TA)^{-1} A^Tb
\end{align}
$$
