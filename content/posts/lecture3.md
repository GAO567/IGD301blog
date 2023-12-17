+++
title = 'Selection techniques'
date = 2023-12-09T10:35:20+01:00
summary = "Introduce three selection techniques: EyeSQUAD,HandDepthCursor and MultiFingerBubble."
draft = false
+++

### EyeSQUAD 
It's a novel selection technique with eye tracking–Eye-controlled Sphere-casting refined by QUAD-menu. EyeSQUAD is divided into two subtasks: sphere-casting and quad-menu refinement. For the sphere-casting subtask, EyeSQUAD allows the user to control the selection sphere with eyes by calculating the convergence point from the user’s eye ray data. Once the sphere-casting selection has been triggered, the set of objects inside it are evenly and randomly distributed on an out-of-context quad-menu. Users then refine the set of selectable objects by gazing in the direction of the quadrant that contains the target and trigger the selection again. 
![alt text](eyesquad.jpeg)
* **Reach - Infinite**
    It uses the spherer-casting to get a set of objects.
* **Cardinality - Multiple:**
    It first select a set of objects then refine to get the target.
* **Progressive Refinement - Discrete → Iterative:** 
    It select the target by iteratively reducing the number of candidate objects by 4 until find the final one.

Ref:
[Wang Y, Kopper R. Efficient and Accurate Object 3D Selection With Eye Tracking-Based Progressive Refinement[J]. Frontiers in Virtual Reality, 2021, 2: 607165.](https://www.frontiersin.org/articles/10.3389/frvir.2021.607165/full)

### HandDepthCursor
HandDepthCursor uses raycasting as the selection mechanism. The use can go deeper and move back to the densely cluttered environment via two non-dominant hand gestures. To go deeper, the user can point her index finger to the front. To
come close, the user can point her thumb to the back.
![alt text](handdepthcursor.png)
![alt text](handdepthcursor.gif)
* **Reach - Infinite**
    It uses the raycasting to select an object.
* **Cardinality - Single:**
    The depth cursor is used to move the cursor in the densely cluttered environment rahter than selecting a set of objects. It still use raycasting to select one object.
* **Progressive Refinement - Continuous:** 
    It gradually changes the cursor depth to see the hidden object. Then use raycasting to select it.

Ref:
[Shi R, Zhang J, Yue Y, et al. Exploration of Bare-Hand Mid-Air Pointing Selection Techniques for Dense Virtual Reality Environments[C]//Extended Abstracts of the 2023 CHI Conference on Human Factors in Computing Systems. 2023: 1-7.](https://dl.acm.org/doi/pdf/10.1145/3544549.3585615)
### MultiFingerBubble
MultiFingerBubble allows users to control a semi-transparent sphere by mapping the hand position onto the sphere position. The sphere can contain multiple targets - up to four. Each target is linked to a specific finger. Users can then select the desired target by flexing the corresponding finger.

![alt text](multifingerbubble.png)
![alt text](multifingerbubble.gif)
* **Reach - Infinite**
    It casts a sphere to contain the targets.
* **Cardinality - Single:**
    It can only select one object.
* **Progressive Refinement - Discrete → Single Step:** 
    MultiFingerBubble includes mutliple targets in the volume selection. Each target in the volume selection is associated with a specific finger. Users can select a target by flexing its corresponding finger. 

Ref:
[Delamare W, Daniel M, Hasan K. MultiFingerBubble: A 3D Bubble Cursor Variation for Dense Environments[C]//CHI Conference on Human Factors in Computing Systems Extended Abstracts. 2022: 1-6.](https://dl.acm.org/doi/pdf/10.1145/3491101.3519692)