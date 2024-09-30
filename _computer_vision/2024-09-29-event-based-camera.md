---
title: "Event Based Camera"
excerpt: "Event Based Camera description"
collection: computer_vision
permalink: /computer-vision/event-based-camera/
---

### Event Cameras vs. Traditional Cameras

A traditional camera outputs frames at fixed time intervals ($\Delta$). 
By contrast, and event camera outputs **asynchronous events** at **microsecond resolution**. An event is generated each time a single pixel detects a change of intensity -in the light-. For any change that is significant enough, it will generate an event. 

Event Structure:
An event is a tuple containing:
1. Timestamp ($μs$ precision)
2. Pixel coordinates $(x, y)$
3. Polarity (+1 for intensity increase, -1 for decrease)

To be more specific, the event based camera measures changes in the **log** scale of the Intensity. Let $I$ be the intensity at a pixel ($x,y$) over time ($t$), then an event is generated when they pass a contrast threshold ($C$)

$$
log I(x,y,t + \Delta t ) - log I (x,y,t) = \pm C
$$

![image](https://github.com/user-attachments/assets/27f791af-07e6-4823-ba49-eff3a8fed566)


When we take a scene and nothing changes, there's no need to transmit any information -that is why it becomes more power efficient-. 

To summarize:
Event cameras sample intensity when this crosses a **threshold** ($C$) (level-crossing sampling) while a standard camera the sample intensity at uniform time intervals (uniform time sampling)

Event cameras are particularly useful in scenarios requiring high-speed vision, such as robotics, autonomous vehicles, and high-speed motion capture.

This  approach to visual sensing offers new possibilities in computational photography and computer vision, especially for applications where capturing rapid changes or operating in challenging lighting conditions is crucial.

### Self-Check Questions
- What are the key differences between an event based camera and a conventional frame camera? 
- What information do the “pixels” of an event based camera transmit? 
- When are events triggered? Show the mathematical formula to describe when an event is triggered 
- How can I reconstruct intensities from events? Show the mathematical formula to do that 
- Can one recover the absolute intensities of an object viewed with an event based camera? Justify the answer 
- What are the advantages and disadvantages of event based cameras
