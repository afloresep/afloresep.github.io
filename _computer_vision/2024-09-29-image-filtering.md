---
title: "Image Filtering"
excerpt: "Image filtering covering convolutions, denoising and more"
collection: computer_vision
permalink: /computer-vision/image-filtering/
---

## Image denoising
Sensors are subject to noise. By analysing the sensor we can model the noise distribution. For example, a typical model for noise is: $x = x_0 +n$ where $x$ is the image we took plus another component, $n$, which is the noise described by $\mathcal{N}(0,\sigma^2$) and $x_0$ is the noise-free image. 
### How can we remove zero-mean noise from an image? 
Ideas: 
 - [Best] Capture many images from the same viewpoint and then average them (similar to using a very long exposure with a small gain) 
 - [Simplest] Averaging between nearby pixels; Here you have to assume the same intensity. When averaging the pixels, since noise is described by $\mathcal{N}(0,\sigma^2$) then the average of the noise -the more pixels we get the better- should be 0. However, and the reason why we don't do this -averaging every pixel- is because you will also be averaging $x_0$ and this will cause the image to go blurry if the intensity in all the $x_0$ are different. Thus, we only average between nearby pixels
 - [Advanced] Learn the statistic of noise-free texture and use them to separate the noise. 
#### Denoising through averaging
If only one image is available, then we can consider spatial averaging. We can provide an estimate for the pixel value at the cross by using the average of the values within the *window.* In simple terms, we create a *window* that takes a certain amount of pixels -9- and then compute the average by multiplying every element -pixel- by 1/9 and adding them up. 
We want to do this at every location. This operation is called **convolution**

### The Convolution
The convolution implements the application of the same linear filter (**also called kernel**) everywhere in a signal (e.g. an image). 
![Image without padding. The filter can now be applied to the edges of the image, and the resulting output is smaller than the input.](https://content.codecademy.com/courses/deeplearning-with-tensorflow/image-classification/convolution.gif)
Mathematically can be written as: 

$$
s[m, n] = (x * w)[m,n] = \sum_{i,j}x[m -i, n-j]w[i,j]
$$

The output (filtered input) $s$ at position $[m,n]$ is defined as the **convolution** ($*$) of input $x$ with the kernel $w$. 
One property of the operation is that is **symmetric**, which means that changing $x$ and $w$ will lead to the same result, and **linear**, which means we can write it as a matrix product. 

$$
s[m, n] = \sum_{i,j}w[m -i, n-j]x[i,j]
$$


#### Properties of convolutions
- **Linearity** Convolution is a linear operation, which means it satisfies two key properties (additivity and homogeneity)
- **Shift-invariance** This property, also known as translation invariance, means that if we shift (translate) the input, the output is shifted by the same amount. Mathematically:

$$
y(t) = h(t) * x(t), \rightarrow  \ y(t - τ) = h(t) * x(t - τ)
$$

In the context of image processing, this means that **the result of applying a filter to a shifted image is the same as shifting the filtered original image**. This property is particularly useful in image recognition tasks, where the position of an object in the image shouldn't affect its detection.
- **Theoretical result** 
#### Output size
Depending on the assumption beyond the images boundaries, we can obtain different output sizes (Padding)

#### Kernel 
-What are edge effects on the kernel? What's the effect on the image after applying the convolution? 

To eliminate edge effects, weight contribution of neighbourhood pixels according to their closeness to the center. This way we get a *"fuzzy circle"*

A natural choice is to use a **Gaussian kernel** or Gaussian blob. The Gaussian kernel is a type of kernel that is used to perform a concept called Gaussian blur. This concept is used to reduce the noise in the image and make the edges of the image smoother.

The idea is that we don't want to change the overall intensity of the image so we want all the weights to add up to one

![image](https://github.com/user-attachments/assets/241c40a7-6c88-46ab-941a-032ad7a53bfd)
*From left to right: Visual representation of Gaussian blob> fuzzy blob>mathematical representation* 

Very important to note that the Gaussian kernel is used to focus on hyperparameter tuning as by in depth understanding of the parameters we can get the best results. The parameters are:
- Kernel size: Determines the range the smoothing will be applied to
- Standard deviation/variance ($\sigma$): Determines the extent of smoothing that will be applied to the image
- Kernel width: Choose a gaussian filter that has half-width to about 3$\sigma$.

##### Separable Kernel
A *separable* kernel can be expressed as the product of two 1D vectors. For example:

$$
\begin{bmatrix}
1 & 2 & 1 \\
2 & 4 & 2 \\
1 & 2 & 1
\end{bmatrix}
= \begin{bmatrix}
1 \\
2 \\
1
\end{bmatrix}
\begin{bmatrix}
1 & 2 & 1
\end{bmatrix}
$$

Mathematically, the 2D Gaussian function can be written as the product of two 1D functions, one depending on \(x\) and the other on \(y\):

$$
\begin{align*}
G_{\sigma}(x, y) &= \frac{1}{{2\pi \sigma^2}} \exp\left(-\frac{x^2 + y^2}{2\sigma^2}\right) \\
&= \left( \frac{1}{{\sqrt{2\pi} \sigma}} \exp\left(-\frac{x^2}{2\sigma^2}\right) \right) \left( \frac{1}{{\sqrt{2\pi} \sigma}} \exp\left(-\frac{y^2}{2\sigma^2}\right) \right)
\end{align*}
$$

This is not merely a mathematical trick; separability allows for significant computational optimizations in convolution. Specifically:
- You can perform the convolution in two steps: first along the $x$-axis and then along the $y$-axis.
- This approach enables parallel processing of the two steps, potentially reducing computation time.

##### Why is separability useful?
When a 2D kernel is separable, a 2D convolution can be broken down into two 1D convolutions (one across the rows and another across the columns). This reduces the computational complexity from $O(n^2m^2)$ for a direct 2D convolution on an $n \times n$ image with an $m \times m$ kernel, to $O(n^2m)$, making the process much more efficient.

#### Noise
Noise refers to **unwanted or random variations in pixel values that degrade the quality of an image**. Several types of noise can affect an image:

- **Salt and pepper noise**: random occurrences of black and white pixels.
- **Impulse noise**: random occurrences of white pixels.
- **Gaussian noise**: variations in intensity that follow a Gaussian (normal) distribution.

Gaussian noise is a good model for various types of random noise, as it simulates the summation of many independent factors affecting the image. It is particularly useful for images with small standard deviations. In practice, we often assume that the noise has a mean of zero and is independent of the image data. Applying Gaussian smoothing with larger standard deviations can suppress noise, but it also has the side effect of **blurring the image**.
##### Reducing salt-and-pepper noise
The **Gaussian filter** is not well-suited for removing **salt and pepper noise** because it operates by applying a weighted average to the pixel values in a neighbourhood, giving more weight to the center pixels. While this makes it effective at smoothing out **Gaussian noise** (where pixel intensity values vary gradually), it struggles with **salt and pepper noise**, which consists of sharp, extreme pixel values (**outliers**)—random occurrences of black (0 intensity) or white (maximum intensity) pixels—scattered throughout the image.

One alternative, is to use a median filter . 

The **median filter** is a widely used technique for reducing noise in images, particularly effective for removing **salt and pepper noise** (random black and white pixels). It operates by replacing each pixel's value with the **median value** of the neighboring pixels in a defined window (usually square-shaped). Unlike other filters, such as the mean filter, the median filter **preserves edges while smoothing out noise.**

 **How the Median Filter Works:**
1. **Sliding window**: The filter uses a sliding window (e.g., 3x3, 5x5) that moves pixel by pixel across the image.
2. **Neighbor selection**: For each pixel, the values of its neighbouring pixels (inside the window) are collected.
3. **Sorting and median calculation**: The values within the window are sorted in ascending order, and the median (middle) value is found.
4. **Replacement**: The original pixel value is replaced with the median value

##### Linearity of Median Filter
Median filter is not linear. 

Proof: 
To prove that the **median filter** is not linear, we need to demonstrate that it violates one or both of the key properties of **linearity**

##### 1. Additivity Violation (Counterexample)

Let's consider a simple 1D example where the median filter operates on a window of size 3.
- Let $I_1 = [1, 2, 100]$
- Let $I_2 = [3, 4, 5]$

Now, apply the median filter to each image separately.

For $I_1$: The median of $[1, 2, 100]$ is 2
For $I_2: The median of $[3, 4, 5]$ is 4.

So,
$F(I_1) = 2, \quad F(I_2) = 4$
Now, consider the sum of the two images $I_1 + I_2 = [4, 6, 105]$.
Apply the median filter to the sum: The median of [4, 6, 105] is 6.

So,
$F(I_1 + I_2) = 6$

Now check whether the additivity property holds:

$F(I_1 + I_2) \neq F(I_1) + F(I_2)$

Since $6 \neq 2 + 4$ the median filter violates additivity.

#### Self-Check Questions
- How can zero-mean noise be removed? 
- How do convolutional filters work? Write the mathematical expression of the convolution between an image and a kernel
- Why is a Gaussian filter a good choice for a denoising kernel?
- Why is a separable filter a good choice for a convolution?
- How does the median filter work? Is it a linear filter? Why? 
