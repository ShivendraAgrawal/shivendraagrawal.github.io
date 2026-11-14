---
layout: page
title: "ViLa-MCL: Vision-Language Monte Carlo Localization"
date: 2026-05-30
description: 2026 - In Submission
img: assets/img/paper/vilamcl/go1_hallway_poster.jpg
video: assets/video/vilamcl/go1_hallway_thumb.mp4
importance: 1
category: work
threads: [embodied]
---

    Team members:
    Shivendra Agrawal, Bradley Hayes

<div class="row justify-content-center mt-3">
    <div class="col-sm-12">
        {% include video.liquid path="assets/video/vilamcl/go1_hallway.mp4" class="img-fluid rounded" autoplay=true loop=true muted=true controls=false poster="/assets/img/paper/vilamcl/go1_hallway_poster.jpg" %}
    </div>
</div>

A robot that wakes up somewhere in a known building has to answer a basic question before it can do anything useful: *where am I?* In grocery aisles, office corridors, and rows of identical cubicles, the answer is hard, because the walls look the same everywhere. Classical lidar localization (AMCL) works well when it is told roughly where it started, but it cannot tell one aisle from another when it has to find itself from scratch.

People solve this by looking at *what* is around them, not at wall geometry. **ViLa-MCL** gives a robot the same ability. A vision-language model (VLM) describes what the camera sees in open-vocabulary language, and those descriptions become the observation model inside a standard particle filter. The VLM never outputs coordinates; probabilistic filtering, odometry, and range sensing still do the localization.

**Rich language, not class labels.** To an object detector, a cabinet is a "cabinet," and an office has many. To ViLa-MCL it is a "yellow metal flammables storage cabinet," which there is only one of. Descriptions keep color, material, readable text, packaging, and object state, exactly the details that distinguish repeated objects. The same interface works for office furniture seen by a quadruped and for long-tail grocery products seen by a smartphone.

<div class="row justify-content-center mt-4">
    <div class="col-sm-12">
        {% include figure.liquid path="assets/img/paper/vilamcl/pipeline.jpg" title="ViLa-MCL pipeline" class="img-fluid rounded" %}
    </div>
</div>
<div class="caption">
    Camera images are described by a VLM and embedded with a lightweight, fine-tuned sentence encoder. Matching against a prior semantic map seeds particles near the most likely poses (inverse semantic proposal); a hierarchical filter then leans on semantics first and tightens with range data once the particles converge.
</div>

## Results

We evaluated on 20 real trajectories in each of two very different settings: a 3,700 sq. ft. lab and office space with a Unitree Go1 quadruped, tested a month after mapping while bikes, robots, and furniture had moved, and a 3,500 sq. ft. grocery store with a handheld smartphone. Every method starts with no knowledge of its pose.

<div class="chart-pair">
  <div class="bar-chart">
    <div class="chart-title">Office (quadruped): global localization success</div>
    <div class="bar-row ours"><span>ViLa-MCL</span><div class="bar-track"><div class="bar" style="width: 87%"></div></div><span class="bar-value">87%</span></div>
    <div class="bar-row"><span>Fixed-class semantic MCL</span><div class="bar-track"><div class="bar" style="width: 17%"></div></div><span class="bar-value">17%</span></div>
    <div class="bar-row"><span>VLG-Loc</span><div class="bar-track"><div class="bar" style="width: 5%"></div></div><span class="bar-value">5%</span></div>
    <div class="bar-row"><span>AMCL (lidar)</span><div class="bar-track"><div class="bar" style="width: 3%"></div></div><span class="bar-value">3%</span></div>
    <div class="bar-row"><span>OMCL</span><div class="bar-track"><div class="bar" style="width: 0%"></div></div><span class="bar-value">0%</span></div>
    <div class="bar-row"><span>ROMAN</span><div class="bar-track"><div class="bar" style="width: 0%"></div></div><span class="bar-value">0%</span></div>
  </div>
  <div class="bar-chart">
    <div class="chart-title">Grocery store (smartphone): global localization success</div>
    <div class="bar-row ours"><span>ViLa-MCL</span><div class="bar-track"><div class="bar" style="width: 75%"></div></div><span class="bar-value">75%</span></div>
    <div class="bar-row"><span>ShelfAware</span><div class="bar-track"><div class="bar" style="width: 57%"></div></div><span class="bar-value">57%</span></div>
    <div class="bar-row"><span>AMCL (depth)</span><div class="bar-track"><div class="bar" style="width: 23%"></div></div><span class="bar-value">23%</span></div>
    <div class="bar-row"><span>Fixed-class semantic MCL</span><div class="bar-track"><div class="bar" style="width: 19%"></div></div><span class="bar-value">19%</span></div>
    <div class="bar-row"><span>ROMAN</span><div class="bar-track"><div class="bar" style="width: 10%"></div></div><span class="bar-value">10%</span></div>
    <div class="bar-row"><span>VLG-Loc</span><div class="bar-track"><div class="bar" style="width: 5%"></div></div><span class="bar-value">5%</span></div>
    <div class="bar-row"><span>OMCL</span><div class="bar-track"><div class="bar" style="width: 1%"></div></div><span class="bar-value">1%</span></div>
  </div>
</div>
<div class="chart-note">Converged within 0.7 m and 45°. Mean over five seeds × 20 trajectories. In the grocery store, ViLa-MCL also keeps tracking stably on 66% of runs, versus 25% for <a href="/projects/shelfmcl/">ShelfAware</a>, our earlier retail-specific system.</div>

## What makes it work

Each ablation changes one thing and re-runs all 20 trajectories with five seeds.

<div class="chart-pair">
  <div class="bar-chart">
    <div class="chart-title">Office: global localization success</div>
    <div class="bar-row ours"><span>Full ViLa-MCL</span><div class="bar-track"><div class="bar" style="width: 87%"></div></div><span class="bar-value">87%</span></div>
    <div class="bar-row"><span>No text augmentation</span><div class="bar-track"><div class="bar" style="width: 57%"></div></div><span class="bar-value">57%</span></div>
    <div class="bar-row"><span>Frozen text encoder</span><div class="bar-track"><div class="bar" style="width: 40%"></div></div><span class="bar-value">40%</span></div>
    <div class="bar-row"><span>Generic class labels</span><div class="bar-track"><div class="bar" style="width: 26%"></div></div><span class="bar-value">26%</span></div>
  </div>
  <div class="bar-chart">
    <div class="chart-title">Grocery: global localization success</div>
    <div class="bar-row ours"><span>Full ViLa-MCL<br><small>encoder trained on office only</small></span><div class="bar-track"><div class="bar" style="width: 75%"></div></div><span class="bar-value">75%</span></div>
    <div class="bar-row"><span>Frozen text encoder</span><div class="bar-track"><div class="bar" style="width: 71%"></div></div><span class="bar-value">71%</span></div>
    <div class="bar-row"><span>Keep blurry products</span><div class="bar-track"><div class="bar" style="width: 62%"></div></div><span class="bar-value">62%</span></div>
    <div class="bar-row"><span>Generic class labels</span><div class="bar-track"><div class="bar" style="width: 56%"></div></div><span class="bar-value">56%</span></div>
  </div>
</div>

- **Rich descriptions are the key ingredient.** Replacing "yellow metal flammables cabinet" with "cabinet," with everything else fixed, drops office success from 87% to 26%.
- **Augmentation in language, not pixels.** Randomly dropping objects from text descriptions during fine-tuning teaches the encoder that a real camera rarely sees everything the map expects: +30 points over fine-tuning without it.
- **The VLM filters its own bad views.** Letting the VLM flag blurry, unreadable products and discarding them adds 13 points in the handheld grocery setting.
- **It transfers.** The encoder fine-tuned only on the office works directly in the grocery store, with no grocery training.

## Where the robot thinks it is

<div class="row justify-content-center mt-3">
    <div class="col-sm-12">
        {% include figure.liquid path="assets/img/paper/vilamcl/topk.jpg" title="Inverse semantic pose proposals" class="img-fluid rounded" %}
    </div>
</div>
<div class="caption">
    Top-10 pose proposals (orange) from a single camera view, with the true pose in green and semantic similarity as the heatmap. Proposals land in the right region despite motion blur (b), mirrored viewpoints (c, f, g, h), repeated doors (e), and objects moved in the month since mapping (f, g).
</div>

<div class="row justify-content-center mt-3">
    <div class="col-sm-12">
        {% include video.liquid path="assets/video/vilamcl/grocery_localization.mp4" class="img-fluid rounded" autoplay=true loop=true muted=true controls=false poster="/assets/img/paper/vilamcl/grocery_localization_poster.jpg" %}
    </div>
</div>
<div class="caption">
    Global localization in the grocery store from a smartphone, six runs. Particles (blue) start spread across the whole store and converge on the true pose (red) as product descriptions come in; the estimate is in green.
</div>

## Paper
<div class="publications">
{% bibliography -f papers -q @*[topic=vlmgloc]* %}
</div>
<p class="chart-note">The current version is under review. An earlier version of this work is available on arXiv under the name VLM-GLoc.</p>
