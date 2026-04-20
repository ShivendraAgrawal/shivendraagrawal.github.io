---
layout: page
title: ShelfHelp - An assitive robotic system to support grocery shopping for BVI
date: 2022-05-20
description: 2023 - Published paper @ AAMAS 23
img: assets/img/paper/shelfhelp/demo.gif
# img: https://images.rawpixel.com/image_1000/czNmcy1wcml2YXRlL3Jhd3BpeGVsX2ltYWdlcy93ZWJzaXRlX2NvbnRlbnQvcHgxMzgyNjcyLWltYWdlLWt3eXFrZHR5LmpwZw.jpg?s=5i_WsjSiGsjd3dh0cW88obuceCo8lP2eP7-3WYh62qs
# redirect: https://unsplash.com
importance: 1
category: work
---

    Team members:
    Shivendra Agrawal (lead)
    Suresh Nayak (Graduate researcher)
    Ashutosh Naik (Graduate researcher)
    Bradley Hayes (Advisor)

<br /><br />
**Papers**
<div class="publications">
{% bibliography -f papers -q @*[topic=shelfhelp]* %}
</div>

We are working towards making an end-to-end system that can assist with independent grocery shopping as shopping with sighted guide is prohibitive and often causes a loss of privacy. Grocery shopping primarily consists of three main subtasks: **navigation**, **product retrieval**, and **product examination**. Our current work focuses on **product retrieval**. *ShelfHelp* can locate items on the shelf and verbally provide fine-grain manipulation guidance to help people retrieve the desired item from an aisle. 

<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfhelp/demo.gif" title="System in action" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    With our system, the blindfloded user is able to retrieve the desired product from the shelf.
</div>

<br>
**Video with sound (recommended version)**
<div class="video-container">
    <iframe src="https://www.youtube.com/embed/6Yxme7-UHhI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>
<br />

<br>
<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfhelp/double_usecase.png" title="Our system's capabilities across tasks" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    ShelfHelp includes a robotic cane equipped with RealSense D455 and T265 cameras. The system is powered through a laptop in a backpack. Left: The system used as a navigational device. It uses audio and haptic feedback for navigation guidance. Right: The system used as a manipulation device. It uses audio for manipulation guidance.
</div>

<br /><br />
**Poster**
<div class="pdf-container" height="1000px">
    <iframe src="/assets/pdf/ShelfHelpAAMAS23.pdf#toolbar=1" allow="autoplay"></iframe>
</div>
<br /><br />

<br>
<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfhelp/system_diagram.png" title="System architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    System Diagram - Perception, planning, and verbal conveyance are executed on a backpack-worn laptop, while the sensing is mounted on the cane.
</div>

<br>
**ShelfHelp** uses a novel 2-stage CV pipeline to locate products on the shelf. The pipeline doesn't need any re-training.  In the first stage, we use a YoloV5 network that we train on the SKU-110K dataset, giving us the most likely bounding boxes to contain any product. In the second stage, we take these most likely regions and compare them against the image of the desired product. We freeze the weights of an autoencoder that we train on the MS-COCO dataset and take just the encoder portion to extract features from the desired product image and the proposed regions (the feature vectors are compared using cosine similarity).

<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfhelp/cv_pipeline_v1.png" title="CV pipeline overview" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>
<div class="caption">
    Our product search algorithm can reliably locate desired products on a grocery shelf. Regions with a high likelihood of containing any product are proposed in the first stage. The features of these regions are then compared against the target product image. Our data association solution is used to identify whether detections from incoming camera frames are new or re-detections of existing products.
</div>

<br>
<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/shelfhelp/verbal_planner.png" title="Manipulation Guidance Overview" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    (Left to right) A sample of discrete commands. The movement (in meters) each command caused. MDP and solution definition. We train a model of human hand movement from demonstrations that inform the transition probabilities T. S defines the state space, A defines the discrete set of verbal actions, and R is the reward function. A policy is learned offline that can be used across reaching tasks.
</div>


