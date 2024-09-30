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
