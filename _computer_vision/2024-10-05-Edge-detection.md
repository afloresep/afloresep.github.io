---
title: "Edge detection"
excerpt: "Edge detection. partial derivatives and gradient of an image."
collection: computer_vision
permalink: /computer-vision/edge-detection/
---

The goal in edge detection is to identify sudden changes (discontinuities) in an image. Intuitively, most semantic and shape information from the images can be encoded in the edges. Edges are more compact than pixels. 

**What causes an edge?** 
They are caused by a variety of factors like depth discontinuity, surface normal discontinuity, surface color or illumination discontinuity. 

![image](https://github.com/user-attachments/assets/8cee4649-9c70-4cb1-86e0-62eb5885ea8d)

If we take an image that has very simple vertical edges and we map the image intensity function (along a horizontal scanline) we can measure the edges. Mathematically, we could use derivates to measure these changes (in intensity). 

For a 2D function$(f(x,y)$) the partial derivate is: 

$$
\frac{\partial f(x,y)}{\partial x} = \lim_{\mathcal{E} \to 0} \frac {f(x + \mathcal{E}, y)-f(x,y)}{\mathcal{E}}
$$

*This comes from the formal definition of partial derivatives. It will be explained at the bottom of this page.*

For discrete data (an image), you can't quite measure the value of intensity for any location. You only have the value at some discrete locations. Thus, we can approximate using finite differences i.e. replacing $\mathcal{E}$ for the smallest shift that you can have in an image which is **one pixel**: 

$$
\frac{\partial f(x,y)}{\partial x} \approx \lim_{\mathcal{E} \to 0} \frac {f(x + 1, y)-f(x,y)}{1}
$$

To implement the above as a convolution, what would be the associated filter? 
 - Roberts Cross Filter
The **Roberts Cross filter** is one of the simplest edge detection methods. It uses two 2x2 kernels to approximate the gradients in the diagonal directions (45° and 135°).

- **Roberts filter for $x$-direction** (diagonal from top left to bottom right):
  
$$
  G_x = \begin{bmatrix}
  1 & 0 \\
  0 & -1
  \end{bmatrix}
$$

- **Roberts filter for $y$-direction** (diagonal from top right to bottom left):
  
$$
  G_y = \begin{bmatrix}
  0 & 1 \\
  -1 & 0
  \end{bmatrix}
  $$

The Roberts filter is used for detecting diagonal edges but tends to be more sensitive to noise since the convolution is performed using only two neighboring pixels.

- Prewitt Filter
The **Prewitt filter** is a 3x3 filter that approximates the first derivative of the image intensity function using a simple averaging process. It is more robust to noise than the Roberts filter due to its use of more neighboring pixels.

- **Prewitt filter for $x$-direction** (horizontal edges):
  
  $$
  G_x = \begin{bmatrix}
  -1 & 0 & 1 \\
  -1 & 0 & 1 \\
  -1 & 0 & 1
  \end{bmatrix}
  $$
  
  This filter emphasizes vertical changes (horizontal edges) by computing differences between left and right neighboring pixels.

- **Prewitt filter for $y$-direction** (vertical edges):
  
  $$
  G_y = \begin{bmatrix}
  -1 & -1 & -1 \\
  0 & 0 & 0 \\
  1 & 1 & 1
  \end{bmatrix}
  $$
  
  This filter emphasizes horizontal changes (vertical edges) by computing differences between top and bottom neighboring pixels.

The Prewitt filter is better at handling noise than Roberts, but its simplicity makes it less sensitive to finer details than other filters.

- Sobel Filter
The **Sobel filter** is an extension of the Prewitt filter, with an added emphasis on the central pixels. It uses a 3x3 kernel similar to the Prewitt filter but adds a weighting factor of 2 to the central row or column. This makes the Sobel filter better at detecting edges because it gives more weight to the pixels closer to the center of the convolution window.

- **Sobel filter for $x$-direction** (horizontal edges):
  
$$
  G_x = \begin{bmatrix}
  -1 & 0 & 1 \\
  -2 & 0 & 2 \\
  -1 & 0 & 1
  \end{bmatrix}
  $$
  
  This detects vertical changes, highlighting horizontal edges, but with greater emphasis on the center row.

- **Sobel filter for $y$-direction** (vertical edges):
  
  $$
  G_y = \begin{bmatrix}
  -1 & -2 & -1 \\
  0 & 0 & 0 \\
  1 & 2 & 1
  \end{bmatrix}
  $$
  
  This detects horizontal changes, highlighting vertical edges, with greater emphasis on the center column.

The Sobel filter is often preferred in practice because its weighting reduces noise while still accurately detecting edges.

### **Summary of Differences**

- **Roberts Filter**:
  - Smallest filter (2x2), computes diagonal differences.
  - Sensitive to noise due to small kernel size.
  - Detects diagonal edges.
  
- **Prewitt Filter**:
  - Uses a 3x3 kernel to compute differences.
  - Simpler and faster than Sobel but less accurate.
  - Emphasizes edges either horizontally or vertically without weighting.

- **Sobel Filter**:
  - Uses a 3x3 kernel like Prewitt but with additional weighting.
  - More robust to noise and produces stronger edge responses.
  - The most commonly used filter for basic edge detection.


### Image Gradient
So now, all these derivatives can be captured by the gradient of the image:

$$
\Delta f  = [\frac{\partial f}{\partial x}, \frac{\partial f} {\partial y}]
$$

Therefore, if changes are only in the $x$-axis the derivative of $y$ is zero and vice versa and for a gradual change is simple a vector with both derivatives. 
![image](https://github.com/user-attachments/assets/830c87b8-024d-4c23-a1ed-cc81f7151c55)

**Notice** The gradients points in the direction of the most rapid increase in intensity 
{: .notice--info}

#### How does this direction relate to the direction of the edge? 
- **Its orthogonal!**
Thus, the **edge/gradient direction** is given by 

$$
\theta = tan^{-1} (\frac{\partial f}{\partial y}/ \frac{\partial f} {\partial x})
$$

And the edge **strength** is given by the L2-Norm

$$
||\Delta f|| = \sqrt {(\frac{\partial f}{\partial x})^2 + (\frac{\partial f} {\partial x})^2}
$$

### Effects of noise
If we have noise in our single despite the fact that we have a very large step (intensity) if the noise is sharp enough and we compute the derivative, we can't actually see where the edge is. 

For example, let's consider a single row or column of the image. 

$f(x)$ with noise would seem like this:

![image](https://github.com/user-attachments/assets/49d9d11e-ede5-4d88-81a0-031fb8c91622)

However, when we compute the partial derivative: 

![image](https://github.com/user-attachments/assets/2be75ef5-214d-4556-9421-54176235c37b)

The solution for this is the same as we saw before when dealing with noise: **denoising** or in other words, smooth first. 

To denoise a **1D** signal, we could use a **gaussian** filter. *Notice how the change is not as sharp, although good enough.*

![image](https://github.com/user-attachments/assets/dfbe0908-0db2-43ce-9e0b-4e3f7f8346ec)

To find edges now, we have to look for **peaks** in $d/dx (f*g)$ 

 We can choose also the size of each of these filters. Smoothed derivative **removes noise**, but also blurs edge. Also finds edges at different **scales**
 
### Review: Smoothing vs. derivative filters
Smoothing filters (denoising filters). Basically "average" things out. 
- Gaussian: remove "high-frequency" components; "low-pass" filter 
- Can the values of a smoothing filter be negative? No. 
- What should the values sum to? Should be equal to 1. Since in case there's no noise the output should be the same as it was (nothing is 'denoised')

Derivative filters
- Derivatives of Gaussian
- Can the values of a derivative filter be negative? Yes. 
- The output should be equal to 0. Since in case there's no change in intensity the output -derivative- should be equal to 0.  

### Self-Check Questions
- What is an edge? 
- How are the edge direction and strength defined mathematically? 
- How does one compute an edge map of a noisy image?


#### Partial Derivative explained
In single-variable calculus, the derivate of a function$f(x)$with respect to$x$is defined as the rate at which$f(x)$changes as$x$changes. The derivative is given by the limit. 

$$
\frac {df}{dx} = \lim_{\Delta x \to 0} \frac{f(x + \Delta x) - f(x)}{\Delta x}
$$

This measures how much the function changes in the$y$-axis as we make a small change$\Delta x$in the$x$-axis 

The formula for the **partial derivative of 2D function** is rooted in the concept of how we measure the rate of change of a function with respect to **one of its variables**, **while keeping the other variable constant.**

In the case of a function$f(x, y)$of two variables, the partial derivative$\frac{\partial f}{\partial x}$measures how the function$f(x, y)$changes as we change$x$, while keeping $y$constant.

Thus, the formula for the partial derivative with respect to$x$looks similar to the single-variable case, but now we’re holding$y$fixed:

$$
\frac{\partial f(x,y)}{\partial x} = \lim_{\mathcal{E} \to 0} \frac{f(x + \mathcal{E}, y) - f(x, y)}{\mathcal{E}}
$$

Here:
-$f(x, y)$is the original value of the function at point$(x, y)$.
-$f(x + \mathcal{E}, y)$is the value of the function at a slightly displaced point$(x + \mathcal{E}, y)$, where we’ve increased$x$by a small amount$\mathcal{E}$.
-$\mathcal{E}$is a small change in$x$(the same idea as$\Delta x$in single-variable calculus).

##### 3. **Why the Limit?**
The limit $\lim_{\mathcal{E} \to 0}$is there because we want to measure the *instantaneous* rate of change of $f(x, y)$with respect to $x$. In other words, we’re interested in how the function changes for an infinitesimally small change in $x$.

If you just look at the difference $f(x + \mathcal{E}, y) - f(x, y)$, it gives you the change in $f$over a finite step $\mathcal{E}$. But to get the exact slope (rate of change) at the point $(x, y)$, we need to take the step size $\mathcal{E}$to be arbitrarily small — hence, we take the limit as $\mathcal{E}$approaches 0.
