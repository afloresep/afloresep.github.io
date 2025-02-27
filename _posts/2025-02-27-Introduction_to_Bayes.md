---
title: 'Introduction to Bayes'
date: 2025-02-27
permalink: /posts/2025/02/Intro_Bayes/
tags:
  - Bayes
  - Mathematics
---

In very few words, Bayes is a framework for reasoning about **uncertainty** using **probability**. The core idea is that we **update** our beliefs about an hypothesis as we gather more and more data (evidence) . 

Now, Bayes' theorem and Bayesian inference are not the same. Bayes' theorem is *just* correct. What we do with it is what bifurcates the paths to [Frequentism](https://en.wikipedia.org/wiki/Frequentist_probability) and Bayesianism. However, for simplicity we will skip this part. If you want to know more about it, here's a [great post](https://www.lesswrong.com/posts/THdadTRy9d8w5ZhK2/bayes-rule-bayesian-inference) on it. 

## Bayes' Theorem

$$
P(\theta \vert D) = \frac{P(D\vert\theta) P (\theta)}{P(D)}
$$

This theorem provides our mechanism to **update** our beliefs based on observed data, where: 

- $P(\theta)$ is the **[[prior]]** - belief about $\theta$ before seeing any data
- $P(D \vert \theta)$  is the **likelihood** (how likely the data is given  $\theta$ )
- $P(D)$  is the **evidence** (probability of data under all possible values of  $\theta$ ).
- $P(\theta \vert D)$  is the **posterior** (updated belief after seeing data).

An intuitive example is testing whether someone suffers or not from a disease given a positive result on a test ($P(\theta \vert D)$).
Here the **prior** is the probability of having the disease -let's say 0.1% of the population-, the **likelihood** ($P(Positive \vert Disease)$ or ($P(D \vert \theta$ ))) i.e. the probability of testing positive if you have the disease, is 99%. The **evidence** here is the amount of people that tested positive -whether they actually had or not the disease.

#### Bayes is NOT intuitive 
Before going into the calculator, we can think of this **intuitively**. If I have a test that is **very** **good** at identifying whether someone suffers or not from cancer -again, our test is 99 times correct out of 100 tests-, and we asked a doctor: *Doctor, I got a positive result, do I have cancer?*, what should the Doctor answer? What would **you** answer? Well, there's been many studies on this and the most common answer for the doctor is: *Yes. You are very likely to have cancer*.

What is the actual answer? If you tested positive, there's only a **9%** chance that you *actually* have cancer. You can do the math. The reason is that, since the disease is so rare (0.1% of the population), the false positives (that 1%) dominate over the true positives.


### Likelihood Function
The **[Likelihood function](https://afloresep.github.io/posts/2025/02/likelihood-function/)** is a fundamental concept in statistical inference. In essence, represents how probable the observed *dependent* data ($y$) is for different values of a parameter ($\theta$). 
Formally, for a given set of points $D= \{x_1, x_2,..., x_n\}$ the likelihood function is:

$$
L(θ; y, X) = p(y | X; θ)
$$


Where:
- $L$ is the likelihood function
- $θ$ is the set of parameters we're trying to estimate
- $y$ is our observed dependent variable
- $X$ is our matrix of independent variables

#### Intuitive example of what the Likelihood function is

Let's say we have a coin that we flip $n$ amount of times, with a probability of seeing heads of $\theta$ and a result (this would be our observed data -$y$-) of $k$ times seeing heads after flipping. 

Let's say the coin is balanced ($\theta = 0.5$) i.e. equal chance of getting heads or tails but we don't know (in other words, we have to guess which $\theta$ is the more likely given our observations). And our observations are that we flipped the coin 10 times, where we got 6 heads ($k$) and 4 tails ($n - k$). Since we don't know that the coin in balanced ($\theta$) we ask the question: *Given that I threw the coin 10 times and 60% of the time it was heads, which balance (i.e. probability of seeing heads) is more likely?* 
Well, intuitively the answer is $\theta = 0.6$. Since we saw 60% of times heads, it makes sense that the coins is balanced in such a way that it gives 60 percent of times a head. A likelihood function then is nothing but how this likelihood -*how well our parameter fits our observed data*- changes for different values of $\theta$. In this example $\theta = 0.6$ is what we call maximum likelihood^1 estimation or **MLE**. 


Now, let's say we keep throwing our balanced coin and we reach 100 throws ($n$), only this time the number of heads ($k$) is 49. This makes sense, the more we throw the more we start to see the actual 'balance' of the coin. Now, what is more likely now, that our coin is unbalanced towards heads like before ($\theta = 0.60$) or that our coin is balanced ($\theta = 0.5$)? 

<img width="1438" alt="image" src="https://github.com/user-attachments/assets/bc773224-60b1-4030-b126-09867b8d62be" />

As we can see in the figure, our $MLE$ was 0.6 when we only had 10 flips and 60% of them were heads. As we increased the number of throws, now that $\theta = 0.6$ becomes much less likely. 

This process where we started with some ***prior*** knowledge -*thinking that our coin was balanced for example*- and after throwing it 10 times updating the ***posterior*** -that is the word for the probability updated from our prior after seeing some data- and finally updating it again after seeing 100 throws is what is known as **Bayesian Inference**. 

To summarize: 
- The **likelihood function** measures how well different parameter values explain the observed data.
- **MLE** finds the parameter value that maximizes the likelihood.
- **Bayesian inference** computes the full posterior distribution of the parameter, updating prior beliefs with observed data.


$$
P(\mu | D) \propto P(D | \mu) P(\mu)
$$

----
1. The principal of **maximum likelihood** says that we should choose $\theta$ (parameters) so as to make the data **as high probability as possible**. This means, we should choose $\theta$ to maximize the **likelihood** of observing our data ($y$) given our $x$'s. 

