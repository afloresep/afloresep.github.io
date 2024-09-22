---
title: "Projection Models"
excerpt: "First part of the Computer Vision course."
collection: computer_vision
permalink: /computer-vision/computer-vision/
---

First part of the HS2024-Computer Vision course. 


1. [[#Basic Camera Design]]
2. [[#Pinhole Camera]]
3. [[#3D to 2D Projection]]
4. [[#Perspective Projection]]
   4.1. [[#Modelling Perspective Projection]]
   4.2. [[#The Coordinate System]]
   4.3. [[#Projection Equation]]
5. [[#Vanishing Points, Lines, and Planes]]
6. [[#More complex projections]]
7. [[#Homogeneous coordinates]]
	7.1. [[#The Linearity problem in Cartesian Coordinates]]
	7.2. [[#How does linearity matter?]]

## Basic Camera Design
Idea 1: Simple Film Placement
The initial concept involves placing a piece of film in front of an object. However, this approach presents a significant problem:

- Too much light impacts the film from all directions, resulting in an overexposed and unfocused image.
- The lack of control over light rays means that each point on the object projects light onto multiple points on the film, creating a blurred image.

Idea 2: Adding a Barrier
To address the issues with the first idea, we introduce a barrier to block most of the light rays:

![[Pasted image 20240918140410.png]]

This barrier, with a small opening, forms the basis of our next concept: the [[#Pinhole Camera]].

## Pinhole Camera

A [[Pinhole camera]] captures a pencil of rays - all rays passing through a single point. This point is known by several names:
- [[Aperture]]
- [[Center of projection]]
- [[Focal point]]
- [[Camera center]]

The image is formed on the [[Image plane]], which is where the film or sensor is located in a real camera.


## 3D to 2D Projection

The process of projecting a 3D scene onto a 2D image plane inevitably results in the loss of information. This raises an important question: What information is preserved, and what is lost in this projection?

![[Pasted image 20240918140810.png]]

### Preserved Properties
- Straight lines: Lines that are straight in 3D remain straight in the 2D projection.
- Incidence: The point where lines intersect is maintained in the projection.

### Lost Properties
- Angles: The angles between lines in 3D are not necessarily preserved in the 2D projection.
- Lengths: The relative lengths of objects are not maintained in the projection.
[[Pinhole camera]]: captures pencil of rays -all rays through a single point: the [[aperture]], [[center of projection]], [[focal point]], [[camera center]]

The image is formed on the image plane. 
## Perspective Projection

#### Modelling Perspective Projection
![[Pasted image 20240918142510.png]]
To compute the projection of *P'* of a scene point *P*, 
1. Form the **visual ray** connecting *P* to the camera center *O* and 
2. find where it **intersects** the image plane. 

The point *P* is in 3D somewhere. And amidst lights, where is the aperture, this light goes through in a straight line, and then will hit our film at position *P'*. So if we know the position of *P* relative to the center, and we know this plane relative to to the center then we want to know what is the location of *P'*. 

**Things to observe:**
- Any point along the red ray will end up in the same location and tells us already what we lost. When we look at the image at P' we don't know where the point was along the way. 

#### The Coordinate System
- The [[optical center]] (*O*) is at the origin 
- The image plane is **parallel to xy-plane** (perpendicular to z-axis) and at a distance $f$ from the center.  
Now giving name to the coordinates: 
- *P'* is the projected point and is computed by casting a ray through the origin  $$P' = \lambda\ P$$ where $\lambda$ is a scalar such that the third coordinate of P' is $-f$ (notice that is -f because the $z$ coordinates go in the position)
Therefore 
$$
f = \lambda \cdot Z \quad \rightarrow \quad \lambda = \frac{-f}{Z} \quad \Rightarrow \quad P' = - \left( \frac{f}{Z} \right) \cdot P
$$
 Alternatively, we can think in 2D instead of 2D, so same as before but just with the Z-Y axes. ![[Pasted image 20240918152221.png]]
 Then both triangles have to be proportional to each other. Which we can write as: 
 
  Similar triangles give us equations between proportions
$$ 
 \frac{y}{f} = \frac{Y}{Z}\rightarrow y= f\frac{Y}{Z}
$$

![[Pasted image 20240918153434.png]]Let's now consider the image plane and do a trick that will simplify it a little bit. Let's **flip the sign**. (i.e. instead of considering the plane on the left side, we want to move it in front of the scene, still at the distance $f$.) Now the image plane and the object are on the same plane. 
When we do that, all we get is that we essentially change the sign of the $z$ from the *P'* from minus to plus.  
>[!NOTE] Flipping the sign a.k.a. moving the plane
>Inside your sensor, the image is upside down, and the algorithms flip it back. When you work with the math about this and you need to think about it, then you don't work in the flipped images, you work with the corrected ones. This is essentially used to make it easier to understand. Otherwise, you have to always flip the coordinates upside down, and that can just lead to more error

So now we've got: 

>[! PROJECTION EQUATION]
>$$
P = (X, Y, Z) \rightarrow P' = (f\frac{X}{Z}, f\frac{Y}{Z},f)
$$

Also, on the image plane we consider only the first two coordinates
$$
P = (X, Y, Z) \rightarrow p = \pi [P] = (f\frac{X}{Z}, f\frac{Y}{Z})
$$

*P = point in the object
P'= projected point in the plane
$f$ = distance between the plane and the origin center also known as [[focal length]]*
$p$ = *projection of P used to denote the coordinate as only on the image plane. Also can be written as $\pi [P]$, where P is again the three coordinates points and $\pi$ for projection.*
## Vanishing Points, Lines, and Planes

![[Pasted image 20240918160530.png]]
A **[[vanishing point]]** is a point on the image plane where the 2D perspective projections of mutually parallel lines in 3D space appear to **converge**.   

So now, any point on the scene projected to the image plane will be obtained from this equation
$$
V = (V_X, V_Y, V_Z)
$$
We can describe all the points along the parallel line as taking one of the points on them and adding some constant ($\lambda$) and multiplying $V$. 
$$\lim_{\lambda \rightarrow \infty} \pi[P + \lambda V] = \lim_{\lambda \rightarrow \infty}f(\frac{X + \lambda V_X}{Z+\lambda V_Z},\frac{Y + \lambda V_Y}{Z+\lambda V_Z}) = f(\frac{V_X}{V_Z}, \frac {V_Y}{V_Z})$$
However, when we go to **infinity**, the $\frac {X}{Z}$ and $\frac {Y}{Z}$ become insignificant, which leaves us with only $V_X$ and $V_Y$ -_the coordinates of the vanishing point_- 

> [!Key Point]
>  Any parallel line with direction V project to infinity **to the same point** which is what we call the **vanishing point**. Which can be described as: 
>  $$ Vanishing\ Point= f(\frac{V_X}{V_Z}, \frac {V_Y}{V_Z})$$
>  - All that matters is the direction of the line (parallel lines share the same direction $V$)
>  - Exception: Directions parallel to the image plane ($Z=0$) where $V$ goes to infinity
>  

This exception, lines on the $XY$ plane raises the question: *are there vanishing points of lines on a plane?*

![[Pasted image 20240921202312.png]]
#### What are the lines on a plane? 
A plane is defined by a _normal_ ($N$), and then any vector on the plane ($V$) will be **orthogonal** (i.e. perpendicular) to $N$.
That also means that N transpose V, the inner product, is going to be zero. More beautifully said:

all $V$, such that $N^T V = 0$, where $N$ is the plane normal. 
$$i.e.,\ \ N_X V_X + N_Y V_Y+N_ZV_Z=0$$
If we look at the set of all rays that satisfy this equation and we compute all the vanishing points: 

#### Case 1: $N_Z \neq 0$
If $\ N_Z \neq 0,$ and the set is  $\{\lambda (V_X, V_Y)\}$ for $\forall \lambda$ -_basically, all the vectors_-, then:
$$
V_Z = -\frac {N_X V_X + N_Y V_Y}{N_Z}
$$
Which means that the set of **vanishing points** is the <u>whole image plane</u> ($V_X$ and $V_Y$ can vary arbitrarily) 

#### Case 2: $N_Z = 0$ (Plane perpendicular to the image plane)
However, if $\ N_Z = 0$, so when you have a floor that is **orthogonal** (i.e. perpendicular) to the image plane, then: 
$$
V_X = -V_Y \frac{N_Y}{N_X}\ \ or \ \ V_Y = -V_X \frac{N_X}{N_Y} 
\,,\ \{ \lambda(-N_Y, N_X)\} \ \ for \ \ \forall \lambda$$
The vanishing points form a set that is a 2D line (with direction (-$N_X, N_Y$)). 

A special case is the **ground plane**. In the case where $N_Z = 0$ and $N_X = 0$ and we have $\lambda$ being any scalar, then we have basically the **horizontal line** -it is basically the x-axis-. 
The line of these vanishing points has a special name. It's called the **horizon**.
- All points at the same height as the camera, project to the horizon. 
 - Projects higher than the camera project above the horizon 

### More complex projections
If now we take a 3D object like a sphere and we want to project it, then the typical assumption or the typical intuition is that we take the projection from the largest point of the sphere and project it. However, there are some factors that come into play, particularly the [[occlusion boundary]]. 

When we project a sphere onto a 2D image plane, we see the following:
1. The projection of the sphere appears as a circle (**or an ellipse if the projection is not orthogonal**).
2. The occlusion boundary projects to the outline of this circle/ellipse.
Therefore, everything behind the [[occlusion boundary]] is hidden from the view. 

## Homogeneous coordinates
#### The Linearity problem in Cartesian Coordinates
In Cartesian coordinates, we represented a 3D point as $P = (X, Y, Z)$ and the perspective projection as: $$\pi[P] = f/Z (X, Y)$$We ask the question
#### Is the projection of $\pi [P] = \frac {f}{Z}(X, Y)$ a linear function in $P = (X, Y, Z)$?
 To determine it, we need to check if it satisfies the two properties of linear functions:
 1. Additivity: $f(ax+by) = af(x) + bf(y)$. 
 2. Homogeneity: $\pi[\alpha P] = \alpha \pi [P]$ for any scalar $\alpha$
 for any $a,b \in R$ and $x, y \in R^n$ 

- Additivity: Let $P_1 = (X_1, Y_1, Z_1)$ and $P_2 = (X_2, Y_2, Z_2)$:

$$\pi[P_1 + P_2] = \pi[(X_1+X_2, Y_1+Y_2, Z_1+Z_2)] = \frac{f}{Z_1+Z_2} (X_1+X_2, Y_1+Y_2)$$ $$\pi[P_1] + \pi[P_2] = \frac{f}{Z_1} (X_1,Y_1) + \frac{f}{Z_2} (X_2,Y_2) = (\frac{fX_1}{Z_1} + \frac{fX_2}{Z_2}, \frac{fY_1}{Z_1} + \frac{fY_2}{Z_2})$$ These are not equal in general, so additivity doesn't hold.
- Homogeneity: Let $P = (X, Y, Z)$ and $\alpha$ be a scalar $$\pi[\alpha P] = \pi[(\alpha X, \alpha Y, \alpha Z)] = \frac{f}{\alpha Z} (\alpha X, \alpha Y) = \frac{f}{Z} (X, Y) = \pi[P]$$ $$\alpha \pi[P] = \alpha(\frac{f}{Z} (X, Y)) = \frac{f}{Z} (\alpha X, \alpha Y)$$ These are not equal for all $\alpha \neq 1$, so homogeneity doesn't hold either.
Since the projection function fails both the additivity and homogeneity tests, we can conclude that $\pi [P] = \frac{f}{Z} (X,Y)$ **==is not==** a linear function in point $P = (X,Y,Z)$.

>[!Key Point]
>The non-linearity arises from the division by $Z$, which introduces a perspective effect. This non-linearity is what allows the projection to capture the 3D to 2D transformation accurately, including effects like distant objects appearing smaller.

#### How does linearity matter? 
Linearity is desirable because we know how to solve and analyse linear systems. 
The trick to work around this problem is by using [[Homogeneous coordinates]] **which allow us to represent perspective projection as a linear transformation**, simplifying many computations in computer graphics and computer vision. The non-linearity is effectively "hidden" in the final step of converting back to Cartesian coordinates.

#### How it works
We simply transform the coordinates in 2D to a 3D vector just by adding 1. 
##### Forward transformation 
$$(x,y) \rightarrow \begin{bmatrix}
X \\
Y \\
1
\end{bmatrix}
 $$
##### Backward transform
Since the 1 may no longer be a 1 after the operations are done, we divide by whatever value may have ($w$)
 $$\begin{bmatrix}
x \\
y \\
w
\end{bmatrix}
\rightarrow \begin{pmatrix}\frac {x}{w}, \frac{y}{w} 
 \end{pmatrix}$$
 