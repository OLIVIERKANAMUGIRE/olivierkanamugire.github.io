---
layout: page
title: Robotic 3D Vision
description: Robotic Navigation and Object Placement Using 3D Computer Vision
img: /assets/img/robotic-vision/2.png
importance: 2
category: work
related_publications: true
github: https://github.com/OLIVIERKANAMUGIRE/Robotic-Navigation-and-Object-Placement-Using-3D-Computer-Vision
---

## Robotic Navigation + Object Placement

**3D Computer Vision Project**

Check it out on GitHub:  
👉 [Robotic Vision on GitHub]({{ page.github }})

This project involves calibrating the robot's environment using camera calibration with DLT, followed by tasks such as cube picking and transporting it to a designated location

---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/robotic-vision/2.png" title="System Setup" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robotic-vision/1.png" title="3D Vision Output" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robotic-vision/3.png" title="Object Placement" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Left: hardware setup. Middle: 3D vision point cloud. Right: robot placing object accurately.
</div>

---

1. **Camera Calibration (Direct Linear Transform)**
   To convert 2D image coordinates into 3D world coordinates, the camera was calibrated using the Direct Linear Transform (DLT). A known set of 3D-2D correspondences was used to estimate the projection matrix P, allowing us to reconstruct 3D coordinates from a single image.

**Key Steps**

- Capture known 3D object points and their 2D image projections.
- Solve for the 3x4 projection matrix P using least squares.
- Use P to map future 2D detections into 3D world space.

2. **\*Localization: Robot and Object**
   Using the camera’s projection matrix, we estimate the 3D positions of all important elements:

- Robot base and arm
- Colored cubes (r_cube, g_cube, b_cube)
- Target locations (\*\_target)

3. **Distance and Angle Computation**
   Once all objects are localized, we compute:

- Euclidean Distance from the robot's arm to each cube.
- Orientation Angle required for the arm to align with the cube’s position.

---

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robotic-vision/3.png" title="Navigation Demo" class="img-fluid rounded z-depth-1" %}
    </div>
    <!-- <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robotic-vision/5.jpg" title="Point Cloud Details" class="img-fluid rounded z-depth-1" %}
    </div> -->
</div>

<div class="caption">
    Example of navigation demo (left) and a close-up of point cloud segmentation (right).
</div>

---
