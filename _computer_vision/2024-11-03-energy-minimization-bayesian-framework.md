---
title: "Energy Minimization and Bayesian Framework"
excerpt: "Energy Minimization and an introduction to the Bayesian Framework"
collection: computer_vision
permalink: /computer-vision/energy-minimization-and-bayesian-framework/
---

# Energy Minimization and An introduction to the Bayesian Framework

## Modelling vs Estimation 
1. We want to find models, laws and/or properties of data

2. Finding latent parameters of given models and data (Direct/Inverse problems)

## What is an inverse problem? 
A direct problem is a problem directed towards a loss of information. All the information is in my observation. It starts with the causes and then calculates the effects
The inverse problem in contrast is the process of calculating from a set of observations the causal factors that produced them. These problems allows us to retrieve information about parameters that we cannot directly observe.
Thus, to determine them, our effects must be stored with a very high numerical accuracy. 

One example for an inverse problem would be **Object Reconstructions** where from a blurry image we try to get the original image. 

## Characterizing inverse problems
Definition (**Hadamard**): A problem is *ill-posed* if any of the following is true:
1. A solution (object) does not always exist
2. The solution (object) is not always unique
3. The solution (object) does not depend continuously on the data (observation)

### Curing ill-posedness
Define approximate solutions satisfying additional constrains (**a priori** information) 
This makes the mapping smoother and ensures that a solution always exists and is unique

Examples: Finite energy, positiveness, upper bounds, family of functions, smoothness...

### Regularization
Definition (A.N. Tikhonov)
A **regularized solution** is one from a family of approximate solutions parametrized via a positive *Regularization* parameter (typical math symbols)
- properties
1. When there is no noise, the family converges to the correct solution as the parameters goes to 0 
2. When there is noise, the approximate solution can be obtained for a non-zero paramater

Example: Suppose that the exact solution to an inverse problem is the function: 

$$
u(x) = \frac{1}{x^2}
$$

This function is not defined in x=0. A regularized solution could be one from the approximate function family

$$
u_\epsilon (x) = \frac {1}{x^2 + \epsilon}
$$

This satisifies the two privous properties. 

In general, given an optimiyation problem 

$$
\hat u = \text{ arg min}_u E[u]
$$

we can regularize it by changing it to
$$
\hat u = \text{ arg min}_u E[u] + \epsilon R[u]
$$

where R[u] is an energy term capturing the additional knowledge

