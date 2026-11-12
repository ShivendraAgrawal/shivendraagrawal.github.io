---
layout: page
title: An ARKit app to help people with learning disabilities
date: 2018-05-20
description: CSCI-5413 Augmented Reality Project using ARKit
img: assets/img/blog/arkit/GIF4.gif
# img: https://images.rawpixel.com/image_1000/czNmcy1wcml2YXRlL3Jhd3BpeGVsX2ltYWdlcy93ZWJzaXRlX2NvbnRlbnQvcHgxMzgyNjcyLWltYWdlLWt3eXFrZHR5LmpwZw.jpg?s=5i_WsjSiGsjd3dh0cW88obuceCo8lP2eP7-3WYh62qs
# redirect: https://unsplash.com
importance: 1
category: work
threads: [assistive]
img_position: center 80%
---


    Team members:
    Shivendra Agrawal 
    Saumya Sinha
    Shayon Gupta
    Daniel Szafir (Professor)


## Overview

We developed a proof-of-concept augmented reality ARKit app that can parse text in the wild and visually represent it for the ease of reading. We also built a basic text editor on top of it designed again around ease of reading. [[Github Link]](https://github.com/ShivendraAgrawal/EduAR)

## Preparing ARKit

**Spatial Mapping:**
  First, spatially map the surrounding environment. The yellow dots represent feature points indicating that ARKit is perceiving and mapping the environment.

  <div class="row">
      <div class="col-sm mt-3 mt-md-0" style="vertical-align:middle">
          {% include figure.liquid path="assets/img/blog/arkit/GIF1.gif" title="Spatial Mapping" class="img-fluid rounded z-depth-1" %}
      </div>
  </div>

<br>
## Parsing and Visualizing Text in a Scene

**Text Recognition and Visualization:**
  EduAR utilizes the LSTM mode of Tesseract OCR to read text from paper. It visualizes this by displaying 3D text on a floating plane in AR, anchored to the real-world text location.

  <div class="row">
      <div class="col-sm mt-3 mt-md-0" style="vertical-align:middle">
          {% include figure.liquid path="assets/img/blog/arkit/GIF2.gif" title="Text Parsing" class="img-fluid rounded z-depth-1" %}
      </div>
  </div>

<br>
## Visual Aide Text Editor

**Interactive Text Editing:**
  The Visual Aide Text Editor allows learners to generate visually anchored 3D text for all texts in a scene. Users can tap desired text segments to input visual aids.

  **Text Editor Features**
  - **Standard Options:** Copy, share, and define text.
  - **Unique Read Mode:** Focus on one paragraph, sentence, or word at a time, with swipe navigation.
  - **Real-Time Font Size Adjustment:** Modify font size on-the-fly for convenience.

  <div class="row">
      <div class="col-sm mt-3 mt-md-0" style="vertical-align:middle">
          {% include figure.liquid path="assets/img/blog/arkit/GIF3.gif" title="Text Editor" class="img-fluid rounded z-depth-1" %}
      </div>
  </div>

<br>
## 3D Objects Integration

**Enhancing Visual Learning:**
  - **Double Tap to Visualize:** Users can double-tap anywhere on the screen to spawn a 3D object on a detected plane.
  - **Object Interaction:** The spawned 3D objects can be rotated and resized using pinch gestures (movement is restricted).
  - **Object Database:** Objects are selected from a predefined database and are contextually linked to the scanned text, aiding visual learners in grasping concepts better.

  <div class="row">
      <div class="col-sm mt-3 mt-md-0" style="vertical-align:middle">
          {% include figure.liquid path="assets/img/blog/arkit/GIF4.gif" title="3D Objects" class="img-fluid rounded z-depth-1" %}
      </div>
  </div>

<br>
## Getting Started

1. **Download the Code:**
   Clone the repository to your local machine:
   
   `git clone https://github.com/ShivendraAgrawal/EduAR.git`