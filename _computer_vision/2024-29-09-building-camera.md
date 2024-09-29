1. #Pin Hole Camera vs Lens Camera
2. #Adding a lens
3. #Depth of Field
4. #Lens Flaws
	1. #Radial Distortion]
	2. #Chromatic Aberration
	3. #Vignetting
5. #Digital Camera
	1. #Camera pixel
	2. #Color sensing in camera Color filter array
	3. #Demosaicing
	4. #Color Moiré

### Pin Hole Camera vs Lens Camera
In a **pinhole camera**, light is captured using a film or sensor behind a barrier with a tiny hole. The hole allows light rays to pass through, forming an image on the film. This design works well because without the barrier, the entire scene would appear overwhelmingly bright and washed out, with no distinct image. The small aperture of the pinhole limits the amount of light, creating a sharper and more defined image.

However, if we increase the size of the aperture, we start compromising the sharpness of the image. A larger aperture lets in more light, but it also causes the image to become blurry because the light rays spread out and are less focused. On the other hand, making the aperture smaller improves the sharpness, as it localizes the light more accurately on the image plane.

A common question might be: **Why not make the aperture as small as possible?** The main reason is that while a smaller aperture produces a sharper image, it also reduces the amount of light entering the camera. This makes the image dim and requires longer exposure times. While waiting longer can reduce noise in a static setting, it is not practical for capturing images instantly or in low-light conditions.

Additionally, if the aperture becomes too small, **diffraction effects** start to occur. Diffraction causes light to bend around the edges of the aperture, which can blur the image, even though the aperture is tiny. This is why simply shrinking the aperture isn't an ideal solution.

A **lens camera** offers a more effective approach. A lens can focus light rays onto the same point on the film or sensor without the need for an extremely small aperture. This allows more light to enter, resulting in a sharp image while reducing exposure time. Unlike the pinhole camera, which produces a sharp image but lets in very little light, a lens camera provides both sharpness and brightness by focusing light rays more efficiently.
![image](https://github.com/user-attachments/assets/de945499-e042-432c-a9b7-69d5ccfe96ae)

### Adding a lens
Adding a lens focuses light onto the film or sensor, improving image clarity and brightness. Using the **thin lens model**, only two key geometrical properties are needed to describe how light rays are deflected:

1. Any ray passing through the center of the lens remains undeviated, meaning it behaves just as it would in a pinhole projection model.
2. All rays that are parallel to the optical axis converge at a single point on a plane located at the lens's *focal length* \( f \).

These simple rules allow us to understand how lenses focus light more effectively, forming sharp images.

Mathematically
![image](https://github.com/user-attachments/assets/c2e6486b-ef67-4e9f-b7a3-c69a1c557aae)
![image](https://github.com/user-attachments/assets/acab5df3-bf93-4da5-aa5a-6420d75dc984)

In this scenario, light hitting the lens at a distance \( d \) from the center at an angle \( \alpha \) follows the relation:

$$
f = \lambda \cos \alpha = \rho \cos \beta
$$

where \( f \) is the focal length, \( \lambda \) and \( \rho \) are distances based on the geometry of the system, and \( \beta \) is the angle the light ray makes after refraction. Additionally, we know:

$$
\sigma \sin \alpha = \rho \sin \beta - d
$$

which gives us the relationship:

$$
\tan \beta = \tan \alpha + \frac{d}{f}
$$

This equation describes how the angle of the light ray changes after passing through the lens, depending on its distance from the center.

One important consideration with lens focusing is that objects at a specific **distance** from the lens will be "**in focus**", meaning their light rays converge precisely on the image plane. Objects closer or farther away will not project perfectly onto this plane and instead form a **circle of confusion**. This is the area where light from a point object spreads out, causing the image to appear blurry or out of focus. The size of this circle determines how blurred an image will be when not focused correctly.

![image](https://github.com/user-attachments/assets/af014b2e-a93d-42b4-ac4b-7674871935f4)
![image](https://github.com/user-attachments/assets/17183f31-92c3-4fdc-95c2-1ee353eea443)

Using similar triangles we can know: $y'/y = D'/D$ and $y'/y = (D'-f)/f$ and with this write the famous thin-lens formula, which relates $D'$ to $d$ and $f$

$$
\frac {1}{D'} + \frac{1}{D} = \frac 1{f} 
$$

### Depth of Field

So far, we've discussed *ideal cases* where we assume perfect measurement precision, but in reality, sensors have physical limitations, including **pixels**. Each pixel collects all the light that falls within its area, typically around 3-4( $\mu m$) in size. 

The **depth of field**  (DOF) refers to the range of distances within which objects appear acceptably sharp in an image. This range is influenced by how the light spreads across the sensor pixels. Objects within the DOF are in focus, while those outside this range project as slightly blurred, but still within the tolerance of the pixel resolution, maintaining clarity.

This concept is important because, rather than achieving perfect focus at a single point, the depth of field allows for a range where objects are perceived as equally sharp, taking into account the real-world limitations of sensors and pixel size

##### Controlling depth of field
To control the DOF we can play with the aperture size. 
- A **smaller aperture increases** the **range** in which the object is approximately in **focus** but at the cost of reducing the amount of light - **need to increase exposure**
![image](https://github.com/user-attachments/assets/9415d0f3-49e1-4823-aae2-7d5461cb66c3)


 The points in focus (**2**) project points onto the image plane (**5**), but points at different distances (**1** and **3**) project blurred images, or [circles of confusion](https://en.wikipedia.org/wiki/Circle_of_confusion "Circle of confusion"). Decreasing the aperture size (**4**) reduces the size of the blur spots for points not in the focused plane, so that the blurring is imperceptible, and all points are within the DOF.

### Lens Flaws
#### Radial Distortion
Radial distortion occurs due to imperfections in the lens, causing light rays to deviate from their intended paths. These deviations become more pronounced near the edges of the lens. A common example of this distortion is the **fisheye effect**, where straight lines appear curved outward or inward. To correct radial distortion, a **calibration procedure** is typically used, often involving a checkerboard calibration board to map and correct the distortions

#### Chromatic Aberration
Chromatic aberration arises because a lens refracts different wavelengths of light (colors) at slightly different angles, leading to color fringing around the edges of objects in an image. This occurs when a lens has varying refractive indices for different wavelengths, causing each color to focus at a slightly different point. The effect is most visible in high-contrast areas and can be corrected with multi-element lenses or post-processing.
![image](https://github.com/user-attachments/assets/3c9ad16a-8bed-4181-84f8-f2890915eca1)

![image](https://github.com/user-attachments/assets/c3bfd188-9529-4e3d-986d-1b141237a596)


#### Vignetting
Vignetting refers to the gradual reduction of image brightness or saturation at the periphery compared to the image center. It typically appears as darker corners in a photograph or image, creating a "tunnel vision" effect where the edges of the image seem to fade into darkness. It can be caused by various factors. 

### Digital Camera 
A digital camera replaces film with a sensor array. Each cell in the array is light-sensitive diode that converts photons to electrons. There are two common types: 
- **Charge coupled devices or CCD**: Pixels transfer charge across the chip, and the signal is read at one corner. Higher power consumption and slower readout.
- **Complementary metal oxide semiconductor or CMOS**: Each pixel has its own amplifier and is read directly at the pixel level. Lower power consumption and faster readout. 
#### Camera pixel 
Each pixel consists of: 
- a microlens to focus light onto the photodiode
- a color filter to select which color to discard or let through (enables color imaging). 
- a light-sensitive area (photodiode)
- circuitry to convert and transmit the photons into electric signals. 
#### Color sensing in camera: Color filter array
A **Bayer grid** is a color filter array used in digital image sensors to capture color information. Since camera sensors can only measure light intensity, the Bayer grid filters the incoming light into red, green, and blue components using an array of colored filters placed over the pixels. The grid typically consists of 50% green filters, 25% red filters, and 25% blue filters. This arrangement reflects the fact that the human eye is more sensitive to green light, which improves perceived image detail.
![image](https://github.com/user-attachments/assets/cfb80ed8-ad5a-4082-a0b3-8871ae7e1517)

Each pixel in the grid captures light from just one color channel (red, green, or blue), meaning we don't get full color information for every pixel. To reconstruct a full-color image, a process called **demosaicing** is used to estimate the missing color information by interpolating from neighboring pixels.
##### Why More Green?
The Bayer grid includes more green filters because the human eye is most sensitive to green wavelengths, which helps improve the perceived sharpness and brightness of the image. By capturing more green light, the sensor mimics the human eye’s response to visual detail.
#### Demosaicing
Since each pixel in the Bayer grid only captures one color, **demosaicing** is required to estimate the missing color components (e.g., a red pixel will need its blue and green values estimated). Demosaicing algorithms use the values from neighboring pixels to fill in the gaps, producing a full-color image.
#### Color Moiré
A potential issue with the Bayer grid is the occurrence of **color moiré**, which are color artifacts that appear due to the interaction between the regular sampling pattern of the Bayer grid and fine, repetitive details in the scene. These artifacts arise because the grid doesn't sample the colors at a high enough resolution in some cases, leading to patterns or false colors in areas with fine texture or detail. 

##### The cause of color Moiré 
**Color Moiré** occurs due to the interaction between the fine details of a scene and the **sampling limitations** of the Bayer grid. The Bayer grid captures color information using a checkerboard pattern of red, green, and blue filters, but since each pixel only captures one color, fine details can create sampling issues. 
1. **Sampling with the Bayer Grid**: 
   The sensor only captures one color per pixel (red, green, or blue). Fine patterns in a scene, such as thin stripes or repetitive textures, may not align well with the grid, which leads to incomplete or inaccurate sampling of the color information.
2. **Reconstructing the Image (Demosaicing)**: 
   To form a complete color image, missing color data is interpolated from neighboring pixels (demosaicing). However, if the details in the scene are too fine compared to the resolution of the Bayer grid, this interpolation can create errors because the algorithm cannot accurately guess the colors from the neighboring pixels.
3. **Color Moiré Artifacts**: 
   When the sensor’s resolution isn’t sufficient to resolve these fine details, especially in regions with repetitive patterns (like textiles or fine grids), the interpolated color values can be incorrect. This results in visible patterns of false colors known as **color moiré**. The moiré appears as bands or streaks of unnatural colors, often where the actual scene has a uniform color.

#### Self check questions
- What happens when the aperture is made very small? 
- Why do we add a lens to the aperture? 
- How does a lens work? Can you derive the equation that describes how a lens deflects a light ray? 
- What is the thin lens formula? Can you derive it?
- What is the depth of field?
- What are important lens distortions? 
- How do we capture color images? What is the Moiré effect? 

*Credit of the images to Steve Seitz, Frédo Durand, A. Efros*


 
