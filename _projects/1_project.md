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

This project uses 3D point clouds and vision algorithms to **navigate** a robot and **place objects** with accuracy.

---

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/robotic-vision/1.jpg" title="System Setup" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robotic-vision/2.jpg" title="3D Vision Output" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robotic-vision/3.jpg" title="Object Placement" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Left: hardware setup. Middle: 3D vision point cloud. Right: robot placing object accurately.
</div>

---

Here’s how the project pipeline works:

1. **Capture environment** with depth camera
2. **Generate 3D point cloud** and segment objects
3. **Localize robot and obstacles**
4. **Navigate safely** using path planning
5. **Place objects** at predefined targets

---

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robotic-vision/4.jpg" title="Navigation Demo" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/robotic-vision/5.jpg" title="Point Cloud Details" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="caption">
    Example of navigation demo (left) and a close-up of point cloud segmentation (right).
</div>

---
