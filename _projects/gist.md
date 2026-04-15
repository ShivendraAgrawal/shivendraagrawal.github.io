---
layout: page
title: GIST - Multimodal Knowledge Extraction and Spatial Grounding
date: 2026-04-15
description: Coming to arXiv in a week
img: assets/img/paper/gist/zone_map.png
importance: 1
category: work
---

Team members:
Shivendra Agrawal, Bradley Hayes

<div class="row justify-content-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/gist/system.png" title="The GIST Multimodal Knowledge Extraction Architecture" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The GIST Multimodal Knowledge Extraction Architecture transforms raw multimodal inputs into Structured Spatial Knowledge.
</div>

## Abstract
Navigating and searching complex, densely packed environments such as retail stores, warehouses, libraries, and hospitals poses a great spatial grounding challenge for both humans and embodied AI. We present **GIST** (Grounded Intelligent Semantic Topology), a multimodal knowledge extraction pipeline that transforms a consumer-grade mobile point cloud into a semantically annotated navigation topology.

<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/gist/topology_products.png" title="Semantic Topology" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    GIST Semantic Topology: The skeletonization-derived graph providing walking nodes and traversable edges, overlaid with localized products.
</div>

## 1. Semantic Zone Classification
<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0 d-flex align-items-stretch" style="vertical-align:middle">
        {% include figure.liquid path="assets/img/paper/gist/zone_map.png" title="Semantic Zone Classification" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Free-space pixels are assigned to semantic zones via KD-tree voting over localized product positions. This continuous overlay reveals latent spatial patterns autonomously.
</div>

## 2. Intent-Aware Search & Category Estimation
<div class="row justify-content-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/gist/biryani_ingredients.png" title="Biryani Ingredients" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/gist/vegan_protein.png" title="Vegan Protein" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The search engine supports identifying distinct physical items for multi-goal recipes (left) and graceful degradation via category estimation when specific items are occluded (right).
</div>

## 3. One-Shot Semantic Localization
The zero-shot textual semantic localizer accurately estimates the user pose from a single smartphone image without geometric feature matching. 

<div class="row justify-content-center">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/gist/mirror_poses_cropped.png" title="Semantic Aliasing in Localization" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Visualization of semantic aliasing. The system identifies correct physical products even if exact viewing angles across mirrored viewpoints are aliased.
</div>

| Metric | All 20 Frames | Correct Zone (80%) |
|-------|---------------|-------------------|
| Top-1 Error | 3.78m | 2.71m |
| Top-3 Error | 2.05m | 1.35m |
| Top-5 Error | 1.45m | 1.04m |

## 4. Visually-Grounded Spatial Routing
The generative routing engine produces robust, egocentric instructions to aid universal human accessibility and offset cognitive load. 

<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/gist/instruction_eval_pastel.png" title="Multi-Criteria LLM Evaluation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    GIST explicit topological grounding significantly outperforms raw RGB sequence models, improving Safety, Completeness, and Egocentric clarity. An in-situ ecological probe achieved an 80% navigation success rate using GIST verbal cues alone.
</div>
