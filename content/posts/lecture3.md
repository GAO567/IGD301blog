+++
title = 'Selection techniques'
date = 2023-12-09T10:35:20+01:00
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
[ref](https://dl.acm.org/doi/pdf/10.1145/3544549.3585615)
### MultiFingerBubble
[ref](https://dl.acm.org/doi/pdf/10.1145/3491101.3519692)