---
title: "Interest Points at different scales"
excerpt: "Locating interest points at different scales, rotation..."
collection: computer_vision
permalink: /computer-vision/interest-points/
---
Interest points are defined as interesting pieces of information to solve an important problem in computer vision which is **correspondence**.
Correspondence is in essence matching points, patches, edges or regions across images  (e.g. finding the same object from different points of view).

### Interest points

Interest points = Keypoints = features

**Definition**: They are points that are distinctive (i.e., can be easily distinguished from other texture) and repeatable (i.e., they can be identified even when the camera view changes- so with position, rotation and scale changes).

They have many applications:

- Tracking
- Recognition
- 3D reconstruction

1. Find a set of distinctive key points from both images (e.g. eye of the cat)

2. Define a region around each keypoint. (square)

3. Extract and normalize the region content. We have to be able to find a way to make them align to some canonical orientation. That way we'll look much similar

4. Compute a local descriptor from the normalized region. ($f_A$ and $f_B$)

5. Math local descriptors ($d(f_A, f_B) < T$


One very old example is the Harris Detector

--- 
## Harris Detector

Widely used for **corner** detection in computer vision. Corners, as distinctive image features, are particularly important because they provide stable points that can be tracked between images. See Locating and Describing Interest Points

To fully understand the Harris corner detection, we'll break down the math behind it and touch on concepts like eigenvalues and eigenvectors.

### 1. **Second Moment Matrix (Auto-correlation Matrix)**

At the heart of the Harris detector is the **second moment matrix**, also called the **structure tensor**. This matrix contains information about local image gradients (changes in pixel intensities) and captures how the intensity changes in a small neighborhood around a pixel.

The matrix is formed from the partial derivatives of the image intensities with respect to $x$ and $y$:

![image](https://github.com/user-attachments/assets/5f0a1a99-9f5d-4279-86b7-f4c123474afe)


$I_x$​ is the partial derivative of the image in the x-direction (horizontal edge information).
$I_y$​ is the partial derivative in the y-direction (vertical edge information).

The second moment matrix is denoted as:

$$
\mathbf{M} = \begin{bmatrix} I_x^2 & I_x I_y \\ I_x I_y & I_y^2 \end{bmatrix}
$$

To obtain this matrix over a neighborhood (not just at a single pixel), we smooth the gradients by applying a Gaussian filter. In other words, the matrix $\mathbf{M}$ gets smoothed by the Gaussian filter $g(\sigma_I)$ to produce a final matrix $\mu(\sigma_I,\sigma_D)$. This matrix contains the weighted sum of the gradient information over a neighborhood, allowing us to assess the **cornerness** of a region.

$$
\mu(\sigma_I,\sigma_D) = g(\sigma_I)*\begin{bmatrix}  I_x^2(\sigma_D) & I_x I_y (\sigma_D) \\  I_x I_y(\sigma_D) &   I_y^2 (\sigma_D)\end{bmatrix}
$$

**$Ix(σ_D)$ and $I_xy(\sigma_D)$** These are the partial derivatives of the image in the $x$- and $y$-directions, but they are computed after **smoothing** the image using a Gaussian filter of scale $\sigma_D$.
The notation $I_x(\sigma_D)$ means the derivative was computed on an image that has been smoothed using a Gaussian filter with standard deviation $\sigma_D$ often referred to as **derivative smoothing**.

**Gaussian filter $g(\sigma_I)$**: After the derivatives are computed, we apply another Gaussian filter of standard deviation $\sigma_I$, which is often referred to as **integration scale**. This filter smooths the second moment matrix (i.e., the matrix of squared gradients) over the local neighborhood of each pixel.

#### Why two Different Scales: $\sigma_D$ and $\sigma_I$? 
$\sigma_D$ This scale controls how finely the **gradients** are computed. A smaller $\sigma_D$ will pick up finer details of the image, making the derivatives more sensitive to small, fine edges or textures. A larger $\sigma_D$​ will blur the image more, resulting in coarser derivatives.

$\sigma_I$: This scale controls how much **smoothing** is applied to the matrix of squared gradients $\mathbf{M}$
 A larger $\sigma_I$​ will average the gradient information over a larger neighborhood, making the feature detection (i.e., corner detection) more robust to noise, but possibly less responsive to fine detail.


This distinction is important because the image structure (edges, corners, textures) can appear at different scales. For instance, you might want to compute derivatives on a finely detailed image ($\sigma_D$) but smooth the results over a larger area ($\sigma_I$​) to avoid being too sensitive to noise or local irregularities.
{: .notice--info}

### 2. **Eigenvalues and Eigenvectors**

To understand why we use eigenvalues, let's first explain what they are.

- **Eigenvectors** are directions in which a linear transformation acts by only scaling them (i.e., their direction doesn’t change).
- **Eigenvalues** are the scaling factors corresponding to those eigenvectors.

The eigenvalues of the matrix $\mu(\sigma_I, \sigma_D)$ (which is essentially a smoothed version of the second moment matrix) represent the **intensity variations** in the $x$- and $y$-directions after both derivative computation and smoothing. These eigenvalues are essential for distinguishing between **corners**, **edges**, and **flat regions**:

- If both eigenvalues are large, this indicates a corner (significant intensity change in both directions).
- If one eigenvalue is large and the other is small, this indicates an edge (intensity changes mainly in one direction).
- If both eigenvalues are small, this indicates a flat region (little or no intensity change in any direction).

So, the eigenvalues give us a measure of how the image gradients behave in a neighborhood. For corner detection, we are interested in areas where the intensity changes significantly in **both** directions, which corresponds to **both eigenvalues being large.**

### 3. **Cornerness Function**

The Harris detector doesn't explicitly compute the eigenvalues but instead uses an approximation based on the **determinant** and **trace** of the matrix $\mathbf{M}$ to determine corner strength.

$$
\begin{align*}
har &=det[\mu(\sigma_I,\sigma_D)]−α⋅ [trace(\mu(\sigma_I,\sigma_D))^2]
\\&= g(I^2_x)g(I^2_y) - [g(I_xI_y)]^2 - \alpha[g(I^2_x) + g(I^2_y)]^2 
\end{align*}
$$


#### Harris Detector: Mathematics

1. We want **large eigenvalues, and small ratio**. The eigenvalues **need to be strong** in both axes. We can capture that with this function with the a function that combines the determinant and the trace 
2. We know:

$$
\begin{align*}
det\ \ M &= \lambda_1 \lambda_2 \\ trace \ \ M=&\lambda_1 + \lambda_2
\end{align*}
$$

4. Harris detector defined via

$$
det \ \ M - k·trace^2 (M) > t
$$

k: empirical constant, k= 0.04-0,06
t: threshold 

### Automatic Scale selection
In many computer vision tasks, such as feature detection, it's important to make the method **scale-invariant**—meaning the features we detect in an image should be identifiable regardless of the scale (or zoom level) of the image. The question of **scalability** and **scale selection** comes up when discussing how to detect corresponding features (like corners, blobs, or other keypoints) in images taken from different distances or perspectives.

#### The Problem: Scale-Invariance in Feature Detection

When you take pictures of the same object from different distances, the features (like corners, edges, and blobs) may appear at different **scales**. For example, a corner of a building in a wide shot might appear small, but the same corner in a zoomed-in shot will appear larger. If we simply apply feature detectors like the Harris detector at a fixed scale (i.e., using a fixed window size), we might miss features in one image that are clearly visible in another.

To address this, we need a method to:
- **Detect features at multiple scales** (so that the features can be recognized whether they appear large or small).
- **Automatically select the appropriate scale** at which a feature is most prominent, allowing us to match corresponding features across images taken at different zoom levels.

This is where **automatic scale selection** comes into play.
#### Solution: Calculating Features at Multiple Scales

A simple but effective way to detect features at different scales is to **compute features at several scales** and then select the optimal one. To do this, we introduce a scale parameter $σ$, which controls the size of the window or patch we use to examine the image.

!Pasted image 20241006194325.png

#### Scale-Sensitive Function $f(I(x,σ))$
The next step is to come up with a function $f(I(x,σ))$ that can detect features **in both the original image and its zoomed-in version**, and that is sensitive to scale. This **function should provide a maximum response** when it detects a feature at the correct scale and then decay as the scale moves away from this optimum.

![image](https://github.com/user-attachments/assets/55d13cbf-d8bb-446b-a3a3-417483fa8b82)


#### What function ($f$) should we use? 

A commonly used function that fits this criterion is the **Difference of Gaussians (DoG)**, which is an approximation of the Laplacian of Gaussian (LoG). The Difference of Gaussians is computed by subtracting two Gaussian-blurred versions of the image with slightly different values of $\sigma$

$$
DoG(x,y,σ)=L(x,y,kσ)−L(x,y,σ)
$$

where $k$ is a constant that determines how different the two scales are (typically k≈1.6).

For each point in the image, we compute the DoG over multiple scales and track how the value changes as we vary σ\sigmaσ. The scale $\sigma_{\text{max}}$​ at which the DoG reaches its **maximum** gives us the optimal scale for that feature. This is key for **automatic scale selection**: the feature is most prominent at this scale, and we can use it to describe the feature in a **scale-invariant** way.

By identifying the local maxima of this function in both position and scale, **we get a list of $(x, y, \sigma)$-coordinates and the corresponding scale for each feature detected.**

### Summary

1. **Automatic scale selection** is essential for detecting features in images at different zoom levels or resolutions.
2. We calculate features at multiple scales by constructing a **scale-space representation** of the image, using Gaussian smoothing with different scales σ\sigmaσ.
3. The **Difference of Gaussians (DoG)** is used as the function f(I(x,σ))f(I(x, \sigma))f(I(x,σ)), which detects features (blobs) and provides a scale-invariant description.
4. By finding local maxima of the DoG function in **position-scale space**, we can identify features and their corresponding scales in both the original and zoomed-in images.
5. This approach allows us to match corresponding features across images, regardless of the scale at which they appear.
---

### Orientation Normalization

In addition to scale, we also need to account for **rotation** to ensure that features are consistently detected regardless of how the object is oriented in the image. The idea is to **normalize the orientation** of the detected features.

Here's the process:

1. **Gradient Calculation**: After detecting a feature and selecting the appropriate scale, we compute the **image gradients** (changes in intensity) within a window around the feature. This gives us information about how intensity changes in different directions.
   
2. **Dominant Orientation**: From the gradients, we create a **histogram of gradient orientations** within the window. The direction with the highest number of gradient responses (the dominant orientation) is chosen as the main orientation for the feature.

3. **Alignment**: Once the dominant orientation is identified, the feature is **rotated** so that this direction points upward (or is standardized). This ensures that the feature will have the same orientation across images, regardless of how the object is rotated.

The **SIFT descriptor** uses this method to normalize the orientation, ensuring that features can be matched even if the object has been rotated between images. This rotation invariance, combined with scale invariance, makes the SIFT descriptor highly robust for feature matching across various transformations.

--- 

Self-Check Questions

- What is an interest point?
- Describe the interest point matching procedure
- How does the Harris point detector work?
- How can we "uniquely" identify the scale of a feature?
- How can we "uniquely" identify the rotation of a feature?
