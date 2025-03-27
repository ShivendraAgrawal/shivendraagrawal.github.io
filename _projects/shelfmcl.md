---
layout: page
title: ShelfMCL - Semantic particle filter localization with low-cost sensors
date: 2024-05-20
description: 2023-2024 - Paper in progress
img: assets/img/paper/shelfmcl/husky.gif
# img: https://images.rawpixel.com/image_1000/czNmcy1wcml2YXRlL3Jhd3BpeGVsX2ltYWdlcy93ZWJzaXRlX2NvbnRlbnQvcHgxMzgyNjcyLWltYWdlLWt3eXFrZHR5LmpwZw.jpg?s=5i_WsjSiGsjd3dh0cW88obuceCo8lP2eP7-3WYh62qs
# redirect: https://unsplash.com
importance: 1
category: work
---

    Team members:
    Shivendra Agrawal (lead)
    Ashutosh Naik (Graduate researcher)
    Dusty Woods (Lab Manager)
    Jake Brawer (Postdoc)
    Bradley Hayes (Advisor)

## Overview
- **Problem:** Estimating where an autonomous system is in a dynamic and cluttered environment such as a grocery store is challenging and unsolved. Such localization ability is essential to providing navigation guidance.
<div class="row justify-content-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/shelfmcl/husky.gif" title="Our system's capabilities across tasks" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
- **Solution:** We present a novel Semantic Monte Carlo Localization algorithm that requires just an RGB-D camera and a visual odometry camera. Our system can be **1.** mounted on a cart or **2.** worn as a wearable.

## Challenges
**Environmental Complexity:**
- The uniform and symmetric nature of aisles makes depth or LiDAR observations essentially featureless and unreliable.
<div class="row justify-content-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/shelfmcl/rays.gif" title="Our system's capabilities across tasks" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
- Constantly changing product stock and poses exacerbates the localization challenge.
<div class="row justify-content-center">
    <div class="col-sm-4 mt-3 mt-md-0 mx-3">
        {% include figure.liquid path="assets/img/paper/shelfmcl/dusty_stock.gif" title="First Image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0 mx-3">
        {% include figure.liquid path="assets/img/paper/shelfmcl/lidarline.gif" title="Second Image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>
## Our Solution: A Novel Semantic Particle Filter

**Minimal Sensor Requirements**

Our system uses low-cost sensors such as an RGB-D camera and a visual odometry camera, without requiring LiDAR or wheel odometry.

<div class="row justify-content-center">
    <div class="col-sm-6 mt-3 mt-md-0" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfmcl/sensors.png" title="Sensors Used in the System" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

**Modularity**
- **Mountable on Carts/Strollers:**  Can add autonomous capabilities to existing equipment.
- **Wearable:** Can support assistive technology for navigation.

<div class="row justify-content-center">
    <div class="col-sm-4 mt-3 mt-md-0 mx-3" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfmcl/maria_cart.gif" title="Mountable Rig on a Cart" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0 mx-3" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfmcl/dusty_wearable.gif" title="Wearable Rig for Assistive Technology" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>
## Algorithm

- ### Semantic Mapping
We trained a custom classifier to classify products into a fixed number of classes.

<div class="row justify-content-center">
    <div class="col-sm-4 mt-3 mt-md-0 mx-3" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfmcl/classification_rgb.gif" title="Semantic Mapping - Classification" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0 mx-3" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfmcl/classification_d.gif" title="Semantic Mapping - Classification" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

- ### Pose Correction
Real-world pose estimates obtained through inverse camera projection are refined using ray casting on the semantic map.

<div class="row justify-content-center">
    <div class="col-sm-6 mt-3 mt-md-0" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfmcl/raycast.gif" title="Product Clusters After Pose Correction" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>
- ### Semantic Localization
Semantic information is fused with the depth observation to in a Monte Carlo Localization framework.

<div class="row">
    <div class="video-container">
        <iframe src="https://1drv.ms/p/c/09290a906a0fdef7/IQSkXC-1UcIzTJuyaf0_m_m7AYbU4TXNwcfVCWSSnWS6j0Y?em=2&amp;wdAr=1.7777777777777777" width="476px" height="288px" frameborder="0">This is an embedded <a target="_blank" href="https://office.com">Microsoft Office</a> presentation, powered by <a target="_blank" href="https://office.com/webapps">Office</a>.</iframe>
    </div>
</div>

<br>
## Demo
<div class="video-container">
    <iframe src="https://www.youtube.com/embed/8q65wmsDsjU?si=5Ti_ab-E149Cmji4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

<br>

<div class="row justify-content-center">
    <div class="col-sm-5 mt-3 mt-md-0 mx-3" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfmcl/optitrack_trajectory_plot.png" title="Ground Truth Trajectory Plot" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-5 mt-3 mt-md-0 mx-3" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfmcl/particle_filter_trajectory_plot.png" title="Localization Trajectory Plot" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    (Left) Ground truth pose for the above & (Right) Localization pose estimates from our system.
</div>

## Preliminary Results
(Detailed results are coming soon)
- Without semantic information, the system converges incorrectly.
- Using semantic information, our system performs robust global localization and maintains it.
















