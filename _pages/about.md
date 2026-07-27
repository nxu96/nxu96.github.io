---
layout: about
title: About
permalink: /
subtitle: Senior Machine Learning Engineer at <a href="https://www.nvidia.com/">NVIDIA</a>. Santa Clara, CA.

profile:
  align: right
  image: prof_pic.png
  image_circular: false # crops the image to make it circular

selected_papers: true # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: false

latest_posts:
  enabled: false
---

I am a Senior Machine Learning Engineer at NVIDIA. My work centers on one goal: helping physical agents — autonomous vehicles and robots — perceive and understand the 3D world they move through. Over the past six years I have built spatial perception systems across the autonomy stack, from centimeter-accurate localization, to city-scale mapping, to neural scene reconstruction.

At NVIDIA, I work on neural reconstruction for autonomous vehicle simulation. Most recently I helped bring [Instant NuRec](https://research.nvidia.com/labs/sil/projects/instant-nurec/) — a feed-forward 3D Gaussian Splatting model that turns a short multi-camera driving log into a fully simulatable 3D world — into NVIDIA's AV simulation stack. Previously, I developed transformer-based online HD mapping and multi-city-scale 3D SLAM systems at [Nuro](https://www.nuro.ai/), and engineered centimeter-accurate localization for production robotaxis at [Motional](https://motional.com/). I received my M.S. in Robotics from the University of Michigan, advised by [Prof. Chad Jenkins](https://web.eecs.umich.edu/~ocj/), and my B.E. from Beihang University.

I am broadly interested in spatial intelligence: 3D reconstruction, geometry foundation models, and neural simulation that closes the loop between the physical and digital worlds. I believe grounding robot foundation models in 3D geometry is key to building agents that can truly reason about — and act in — the physical world.

## [Selected Publications]({{ '/publications/' | relative_url }}){: style="color: inherit"}

{% include selected_papers.liquid %}

## Selected Projects

<div class="row">
  <div class="col-sm-4 mt-3">
    <a href="https://www.nuro.ai/blog/exploring-hd-mapping-that-scales">
      <img src="/assets/img/nuro-hd-mapping.gif" class="img-fluid rounded z-depth-1" alt="Nuro HD mapping" />
    </a>
  </div>
  <div class="col-sm-8 mt-3">
    <b>HD Mapping &amp; 3D SLAM at Nuro</b><br />
    Online DETR-based HD map construction in a unified camera–LiDAR BEV framework, fusion of prior map data with live sensor streams for robustness to real-world map changes, and a multi-city-scale 3D mapping and SLAM pipeline.<br />
    <a href="https://www.nuro.ai/blog/unified-perception-model">Unified Perception</a> ·
    <a href="https://www.nuro.ai/blog/exploring-hd-mapping-that-scales">HD Mapping that Scales</a> ·
    <a href="https://www.nuro.ai/blog/the-nuro-autonomy-stack">Nuro Autonomy Stack</a>
  </div>
</div>
