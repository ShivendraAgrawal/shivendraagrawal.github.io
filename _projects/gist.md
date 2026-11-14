---
layout: page
title: "GIST: What Do VLM Agents Need to Give Good Directions to People?"
date: 2026-04-15
description: 2026 - In Submission
img: assets/img/paper/gist/rgb_depth_poster.jpg
video: assets/video/gist/rgb_depth_thumb.mp4
importance: 1
category: work
threads: [embodied]
---

    Team members:
    Shivendra Agrawal, Bradley Hayes

<div class="row justify-content-center mt-3">
    <div class="col-sm-12">
        {% include video.liquid path="assets/video/gist/rgb_depth.mp4" class="img-fluid rounded" autoplay=true loop=true muted=true controls=false poster="/assets/img/paper/gist/rgb_depth_poster.jpg" %}
    </div>
</div>
<div class="caption">
    RGB and depth from a handheld smartphone in a grocery aisle: dense, long-tail, and geometrically repetitive.
</div>

Vision-language models (VLMs) know a lot about what things are, but not where they are. Ask one to guide a person through a grocery store or an office and it will happily describe a shopping cart that has since moved, or tell you to "turn right down the next aisle" without saying which one. **GIST** asks a simple question: *what spatial input does a VLM agent actually need to give a person grounded directions?*

GIST builds a lightweight **semantic topology** from a single scan by a smartphone or a quadruped robot: a traversability graph of walkable space, with open-vocabulary landmarks (products, furniture, signs) bound to its edges in a shared coordinate frame. Geometry (turns, distances, which side a landmark is on) is computed, never generated, and the VLM is only asked to turn those facts into human-centered language.

<div class="row justify-content-center mt-3">
    <div class="col-sm-12">
        {% include figure.liquid path="assets/img/paper/gist/system_v2.jpg" title="GIST overview" class="img-fluid rounded" %}
    </div>
</div>
<div class="caption">
    Offline, one traversal (a phone in a grocery store, a Unitree Go1 in an office) yields an occupancy map, a topology graph, and VLM-annotated landmarks. Online, GIST plans a path on the graph and the VLM turns it into egocentric instructions.
</div>

## Explicit route facts beat richer visual input

We compared five ways of giving the same VLM a route across 30 routes in two environments: GIST's route description plus an annotated map image, the route description alone (**GIST Text**), the map image with only a coarse summary (**GIST Visual**), a coarse **Route Summary**, and a future-view image sequence in the style of **NavComposer**. Two held-out LLM judges from other model families scored the instructions blind on five criteria from spatial-cognition research.

<div class="chart-pair">
  <div class="bar-chart">
    <div class="chart-title">Grocery store: average score (1–5)</div>
    <div class="bar-row ours"><span>GIST</span><div class="bar-track"><div class="bar" style="width: 88.4%"></div></div><span class="bar-value">4.42</span></div>
    <div class="bar-row"><span>GIST Text</span><div class="bar-track"><div class="bar" style="width: 87.4%"></div></div><span class="bar-value">4.37</span></div>
    <div class="bar-row"><span>GIST Visual</span><div class="bar-track"><div class="bar" style="width: 70.6%"></div></div><span class="bar-value">3.53</span></div>
    <div class="bar-row"><span>NavComposer</span><div class="bar-track"><div class="bar" style="width: 62.4%"></div></div><span class="bar-value">3.12</span></div>
    <div class="bar-row"><span>Route Summary</span><div class="bar-track"><div class="bar" style="width: 47%"></div></div><span class="bar-value">2.35</span></div>
  </div>
  <div class="bar-chart">
    <div class="chart-title">Office: average score (1–5)</div>
    <div class="bar-row ours"><span>GIST</span><div class="bar-track"><div class="bar" style="width: 84.6%"></div></div><span class="bar-value">4.23</span></div>
    <div class="bar-row"><span>GIST Text</span><div class="bar-track"><div class="bar" style="width: 85.4%"></div></div><span class="bar-value">4.27</span></div>
    <div class="bar-row"><span>GIST Visual</span><div class="bar-track"><div class="bar" style="width: 65.6%"></div></div><span class="bar-value">3.28</span></div>
    <div class="bar-row"><span>NavComposer</span><div class="bar-track"><div class="bar" style="width: 69%"></div></div><span class="bar-value">3.45</span></div>
    <div class="bar-row"><span>Route Summary</span><div class="bar-track"><div class="bar" style="width: 44.6%"></div></div><span class="bar-value">2.23</span></div>
  </div>
</div>
<div class="chart-note">Mean of two held-out judges over 15 routes per environment. GIST outperforms NavComposer in both environments (paired Wilcoxon, significant for both judges).</div>

The route description is what matters. With it, adding the map image changes nothing detectable (GIST vs. GIST Text). Without it, the image helps a sparse summary but cannot replace the computed route.

## What the VLM actually needs

To pin this down, we removed one input at a time from GIST (240 additional instructions, same judges). Removing route information hurts in both environments; removing the map image, or either of its layers, does not.

<div class="chart-pair">
  <div class="bar-chart">
    <div class="chart-title">Grocery store: score drop when removed</div>
    <div class="bar-group">Route information</div>
    <div class="bar-row ours"><span>Route, distances &amp; landmarks</span><div class="bar-track"><div class="bar" style="width: 85.3%"></div></div><span class="bar-value">1.22</span></div>
    <div class="bar-row ours"><span>Route + drawn path</span><div class="bar-track"><div class="bar" style="width: 93%"></div></div><span class="bar-value">1.33</span></div>
    <div class="bar-row ours"><span>Ordered route</span><div class="bar-track"><div class="bar" style="width: 60.8%"></div></div><span class="bar-value">0.87</span></div>
    <div class="bar-row ours"><span>Landmarks</span><div class="bar-track"><div class="bar" style="width: 46.2%"></div></div><span class="bar-value">0.66</span></div>
    <div class="bar-group">Map image</div>
    <div class="bar-row"><span>Whole map image</span><div class="bar-track"><div class="bar" style="width: 0%"></div></div><span class="bar-value">≈0</span></div>
    <div class="bar-row"><span>Occupancy background</span><div class="bar-track"><div class="bar" style="width: 3.5%"></div></div><span class="bar-value">0.05</span></div>
    <div class="bar-row"><span>Drawn graph &amp; path</span><div class="bar-track"><div class="bar" style="width: 0.7%"></div></div><span class="bar-value">0.01</span></div>
  </div>
  <div class="bar-chart">
    <div class="chart-title">Office: score drop when removed</div>
    <div class="bar-group">Route information</div>
    <div class="bar-row ours"><span>Route, distances &amp; landmarks</span><div class="bar-track"><div class="bar" style="width: 100%"></div></div><span class="bar-value">1.43</span></div>
    <div class="bar-row ours"><span>Route + drawn path</span><div class="bar-track"><div class="bar" style="width: 77.6%"></div></div><span class="bar-value">1.11</span></div>
    <div class="bar-row ours"><span>Ordered route</span><div class="bar-track"><div class="bar" style="width: 35%"></div></div><span class="bar-value">0.50</span></div>
    <div class="bar-row ours"><span>Landmarks</span><div class="bar-track"><div class="bar" style="width: 53.8%"></div></div><span class="bar-value">0.77</span></div>
    <div class="bar-group">Map image</div>
    <div class="bar-row"><span>Whole map image</span><div class="bar-track"><div class="bar" style="width: 2.1%"></div></div><span class="bar-value">0.03</span></div>
    <div class="bar-row"><span>Occupancy background</span><div class="bar-track"><div class="bar" style="width: 4.9%"></div></div><span class="bar-value">0.07</span></div>
    <div class="bar-row"><span>Drawn graph &amp; path</span><div class="bar-track"><div class="bar" style="width: 0.7%"></div></div><span class="bar-value">0.01</span></div>
  </div>
</div>
<div class="chart-note">Drop in the five-criterion average (two-judge mean). All route-information drops are significant for both judges; none of the map-image drops are.</div>

## Faithful and fully specified

We audited every GIST and NavComposer instruction against the planned route, blind to condition. GIST instructions were route-consistent on 27 of 30 routes and adequately specified on 29; NavComposer's on 22 and 16. Two typical instructions show the difference:

<div class="info-grid">
  <div class="info-card compare-quote">
    <h4>Future-view image sequence</h4>
    <p>"Walk forward past the shopping cart toward the back wall. Turn right into the aisle."</p>
    <div class="verdict">Names a movable object and never says where to turn. On this route the turn comes 3.5 m later, before the last aisle, so a walker heading for the wall can pass it.</div>
  </div>
  <div class="info-card compare-quote">
    <h4>GIST</h4>
    <p>"Walk forward down the aisle for 4.5 meters, passing Swad Kala Chana on your right after about 1.0 meter … Your destination, Laxmi Chana Dal, will be on your right."</p>
    <div class="verdict">Distances, a durable landmark with its side, and a clear stopping point.</div>
  </div>
</div>

<div class="row justify-content-center align-items-center mt-4">
    <div class="col-5 col-sm-3 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/gist/go1_lab.mp4" class="img-fluid rounded" autoplay=true loop=true muted=true controls=false %}
    </div>
    <div class="col-sm-9 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/gist/topology_two_env.png" title="Semantic topology in two environments" class="img-fluid rounded" %}
    </div>
</div>
<div class="caption">
    The same representation in (a) a grocery store scanned with a smartphone and (b) an office mapped by a Unitree Go1 quadruped (left): walking nodes, traversable edges, and localized objects.
</div>

**In-store pilot.** In a preliminary pilot in the grocery store (N=5 sighted participants, 10 trials), participants reached the target in 8 of 10 trials using only the generated verbal instructions. Both failures were bottom-shelf items the chest-height scan never saw. This is early real-world signal, not a study with target users.

## Beyond directions

The same topology supports other tasks people need before and during navigation. These are early demonstrations rather than the focus of the paper.

<div class="row justify-content-center mt-3">
    <div class="col-6 col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/gist/biryani_ingredients.png" title="Biryani ingredients" class="img-fluid rounded" %}
    </div>
    <div class="col-6 col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/gist/vegan_protein.png" title="Vegan protein" class="img-fluid rounded" %}
    </div>
</div>
<div class="caption">
    <b>Semantic goal search.</b> A query like "biryani ingredients" is decomposed into items located on the map (left). When nothing matches exactly, as with "vegan protein," the agent suggests mapped alternatives and the zone where such items are likely shelved (right).
</div>

<div class="row justify-content-center mt-3">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/paper/gist/zone_map.png" title="Semantic zones" class="img-fluid rounded" %}
    </div>
</div>
<div class="caption">
    <b>Zone classification.</b> Products are grouped into 18 zones and spread over the walkable floor plan, revealing structure such as the store's organic section without it being labeled.
</div>

**Localization from a single photo.** Matching the open-vocabulary description of one image against the topology gives coarse pose proposals, with median errors of 2.69 m (grocery) and 2.42 m (office). Turning this signal into full global localization and tracking is the subject of [VLM-GLoc](/projects/vlm_localization/).

## Paper
<div class="publications">
{% bibliography -f papers -q @*[topic=gist]* %}
</div>
<p class="chart-note">The current version is under review. An earlier version of this work is available on arXiv under the title above.</p>
