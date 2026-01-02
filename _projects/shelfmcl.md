---
layout: page
title: ShelfAware - Real-Time Semantic Localization in Quasi-Static Environments
date: 2025-05-20
description: 2024-2025 - In submission
img: assets/img/paper/shelfmcl/husky.gif
# img: https://images.rawpixel.com/image_1000/czNmcy1wcml2YXRlL3Jhd3BpeGVsX2ltYWdlcy93ZWJzaXRlX2NvbnRlbnQvcHgxMzgyNjcyLWltYWdlLWt3eXFrZHR5LmpwZw.jpg?s=5i_WsjSiGsjd3dh0cW88obuceCo8lP2eP7-3WYh62qs
# redirect: https://unsplash.com
importance: 1
category: work
---

    Team members:
    Shivendra Agrawal, Jake Brawer, Ashutosh Naik, Alessandro Roncone, Bradley Hayes
    
<div class="publications">
{% bibliography -f papers -q @*[topic=shelfmcl]* %}
</div>

## Abstract
Localization in dynamic, visually aliased environments like grocery stores is a difficult challenge for autonomous systems. Aisles often look identical geometrically, and stock changes frequently. **ShelfAware** is a novel semantic localization framework that achieves robust, real-time global localization using only low-cost sensors—specifically, a monocular RGB-D camera on a smartphone.
Unlike traditional methods that rely on expensive LiDAR or detailed geometric maps, ShelfAware uses a **Semantic Particle Filter**. It leverages Visual-Inertial Odometry (VIO) for motion estimation and corrects drift by matching detected product categories (e.g., "cereal", "soda") against a lightweight semantic map. This allows the system to localize accurately even in featureless or repetitive aisles.

<div class="row justify-content-center">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/shelfmcl/husky.gif" title="Our system's capabilities across tasks" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## The ShelfAware Approach
The core of our solution is a particle filter that fuses visual-inertial odometry with semantic observations.

1. **Semantic Mapping:** We utilize a pre-built semantic map that stores the locations of product categories rather than individual items, making the map robust to daily stock changes.
2. **Observation Model:** As the agent moves, a custom YOLO-based detector identifies product categories in the camera frame.
3. **Particle Update:** We project these detections into 3D space. Particles that "expect" to see the detected products at their hypothesized location receive higher weights, while those that don't are penalized. This effectively converges the particle cloud to the robot's true location.

<div class="row justify-content-center">
    <div class="col-sm-6 mt-3 mt-md-0 d-flex align-items-stretch" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfmcl/sensors.png" title="Sensors Used in the System" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>

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

<div class="row justify-content-center align-items-center">
    <div class="col-sm-4 mt-3 mt-md-0" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfmcl/raycast.gif" title="Product Clusters After Pose Correction" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-1 text-center" style="font-size: 2em;">
        <i class="fas fa-arrow-right"></i>
    </div>
    <div class="col-sm-4 mt-3 mt-md-0" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfmcl/shelfaware_product_map.png" title="ShelfAware Product Map" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>
- ### Semantic Localization
Semantic information is fused with the depth observation to in a Monte Carlo Localization framework.

<div class="row justify-content-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/shelfmcl/overview_v3.png" title="System Architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>
## Demo
<div class="video-container">
    <iframe src="https://www.youtube.com/embed/8q65wmsDsjU?si=5Ti_ab-E149Cmji4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

<br>

## Experimental Results
We evaluated ShelfAware in a semantically dense retail environment (a mock store) across diverse conditions, including cart-mounted and wearable setups. The system achieved a **96%** global localization success rate with a mean time-to-convergence of **1.91s**, significantly outperforming geometric baselines like MCL (**22%** success) and AMCL (**10%** success). The system operates in real-time (**9.6Hz**) on consumer laptop-class hardware, demonstrating robust tracking even in dynamic, visually aliased aisles.

<div class="row justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0 mx-3" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfmcl/sample_trajectories.png" title="Sample Trajectories" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Ground truth trajectories shown in blue and trajectories predicted by ShelfAware shown in red.
</div>

<br>

<div class="publications">
{% bibliography --cited_in_order %}
</div>

```bibtex
@article{agrawal2025shelfaware,
  title={ShelfAware: Real-Time Visual-Inertial Semantic Localization in Quasi-Static Environments with Low-Cost Sensors},
  author={Agrawal, Shivendra and Brawer, Jake and Naik, Ashutosh and Roncone, Alessandro and Hayes, Bradley},
  journal={arXiv preprint arXiv:2512.09065},
  year={2025}
}
```















