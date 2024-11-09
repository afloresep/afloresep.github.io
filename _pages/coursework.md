---
layout: archive  
permalink: /coursework/  
author_profile: true  
---

This is the coursework section. Here, you'll find my collection of notes, articles, and reflections on various topics related to **Machine Learning (ML)**, **Artificial Intelligence (AI)**, and **Computer Vision (CV)** and **DevOps**. These resources are aimed at documenting my learning journey, especially during my PhD, and are meant to serve as a study guide for anyone delving into these fields as well. 

## Computer Vision
I will be documenting my progress in **HS2024 - Computer Vision**, a course offered by Universität Bern, which covers key concepts in CV. My notes dive into various topics that form the backbone of the field. These notes will _hopefully_ form a comprehensive resource for anyone interested in the technical foundations and applications of computer vision.
#### Image Formation
1. **[Projection Models](https://afloresep.github.io/computer-vision/Projection-Models/):**  Explanation of camera models and the mathematics behind projecting 3D objects onto 2D images. It covers the essentials of camera calibration, perspective projection, and the pinhole camera model.
2. **[Building a real camera](https://afloresep.github.io/computer-vision/building-cameras/)**: Covers lens camera, depth of field, lens flaws, bay array, desmosaicing and concept of color Moiré
3. **[Event Based Camera](https://afloresep.github.io/computer-vision/event-based-camera)**

#### Image processing
1. **[Image filtering](https://afloresep.github.io/computer-vision/image-filtering/)**: Image denoising, convolution, gaussian kernel...
2. **[Edge detection](https://afloresep.github.io/computer-vision/edge-detection/):** Noise detection, gaussian filters, separability of a convolution, edge detection and partial derivative filters.
3. **[Locating and Describing interest points](https://afloresep.github.io/computer-vision/interest-points/):** Locating Interest Points at Different Scales, cornerness function, Harris detector, Difference of Gaussians...

#### Preprocessing
1. **[Fitting](https://afloresep.github.io/computer-vision/fitting/)**: Fitting points to line. Total Least Squares and Random Sample Consensus (RANSAC) method for line fitting. 
2. **[Image Alignment](https://afloresep.github.io/computer-vision/image-alignment/)**: robust methods for aligning images by detecting and matching distinctive features, using transformations like affine and homography. 


## Machine Learning
I will be documenting my progress in **HS2024 - Machine Learning**, a course offered by Universität Bern, which covers key concepts in Machine Learning. Most of these notes are the same as the **CS229** course from Standford with extended explanations and notes. 

Supervised Learning
- [Linear Regression](https://afloresep.github.io/machine-learning/linear-regression) & [Probabilistc interpretation](https://afloresep.github.io/machine-learning/probabilistic-interpretation/)
- [Classification](https://afloresep.github.io/machine-learning/classification)
- [Generalized Linear Models](https://afloresep.github.io/machine-learning/generalized-linear-models)
- [Generative Learniing Algorithms](https://afloresep.github.io/machine-learning/generative-learning-algorithms/): Gaussian Discriminant analysis model & Naive Bayes Classifier


## Docker

During my work at UniBe I came to realize the importance of DevOps skills in modern software development. While machine learning and algorithms are critical components of my work, understanding DevOps has proven to be just as essential. For me, the true excitement of Computer Science lies in the ability to build complete solutions from the ground up and then deploy them for others to use. This is why I'm diving into Docker through this fantastic course on [Udemy](https://www.udemy.com/course/docker-mastery/?couponCode=KEEPLEARNING).

### Course outline: 
1. [Introduction to Docker](https://afloresep.github.io/docker/docker-introduction/)
2. [Docker Containers](https://afloresep.github.io/docker/docker-containers/)
3. [Docker Network](https://afloresep.github.io/docker/docker-network/)
4. [Docker Images](https://afloresep.github.io/docker/docker-images/)


## Applied Optimization
I will be documenting my progress in **HS2024 - Applied Optimization**, a course offered by Universität Bern.
The course consists of am applied introduction, covering a broad range of practically important topics, as for instance: Mathematical modeling of real-world problems, theory of convexity, Lagrange dualism, algorithms for unconstrained and constrained optimization with inequalities (e.g. gradient descent, Newton’s method, trust-region methods, active set approaches, interior point methods, …).

Hopefully by the end of the course I will be able to: 
- Understand which classes of optimization problems are easy/hard to solve.
- Model or re-formulate problems in a way that they become easier (e.g. convex).
- Understand the fundamental ideas behind unconstrained, constrained and mixed-integer optimization.
- Implement and use various optimization algorithms (programming exercises are in C++).
- Understand and tune the parameters and output statistics that are exposed by optimization packages.

### Course outline: 
1. [Introduction and Fundamental Definitions](https://afloresep.github.io/applied-optimization/fundamental-definitions)

## TMAP
A significant portion of my PhD work revolves around **TMAP**, a versatile tool in my research. To deepen my understanding of this tool, I’ve dedicated some time to exploring its concepts and implementation. The key topics involved, such as **Locality-Sensitive Hashing (LSH)** and **Min-Hashing**, are critical components of the larger framework.

I've written a detailed blog post that offers a foundational introduction to **TMAP** and breaks down its core mechanisms. Below are links to some of the key concepts discussed within the post:

1. **[TMAP Overview](https://afloresep.github.io/TMAP/):**  
   This post provides a comprehensive introduction to TMAP, explaining its role, functionality, and how it fits into the broader landscape of data mapping and visualization.

   - **[Min-Hashing](https://afloresep.github.io/posts/2024/09/MinHashing/):**  
     A dive into Min-Hashing, explaining how it efficiently estimates the similarity between datasets using hashed subsets of the data.
     
   - **[LSH Forest](https://afloresep.github.io/posts/2024/09/LSH-Forest/):**  
     An overview of Locality-Sensitive Hashing (LSH) and how the LSH Forest algorithm is used to approximate nearest neighbors in high-dimensional spaces.
     
   - **[K-NN Graph](https://afloresep.github.io/posts/2024/09/knn-graph/):**  
     A breakdown of K-Nearest Neighbor (K-NN) graphs and how they are applied in data clustering and classification.

More posts on related concepts and applications will be added soon as I continue to expand my research.

---

As I progress through my PhD and research projects, I will continue to update this space with more notes, tutorials, and insights. Keep an eye out for new additions, and feel free to reach out if you'd like to discuss any of these topics in more detail!

