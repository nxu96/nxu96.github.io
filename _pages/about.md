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
    <a href="https://www.youtube.com/watch?v=e3dfXj6ATA0">
      <img src="/assets/img/nvidia-neural-reconstruction.gif" class="img-fluid rounded z-depth-1" alt="NVIDIA neural reconstruction for autonomous vehicles" />
    </a>
  </div>
  <div class="col-sm-8 mt-3">
    <b>Neural Reconstruction for Autonomous Vehicles at NVIDIA</b><br />
    Turning real-world driving logs into fully simulatable 3D Gaussian Splatting worlds for closed-loop AV simulation with NVIDIA Omniverse NuRec — from integrating the feed-forward Instant NuRec model into the production reconstruction pipeline to delivering the LiDAR-free NuRec release.<br />
    <a href="https://www.youtube.com/watch?v=e3dfXj6ATA0">Video</a>
  </div>
</div>

<div class="row">
  <div class="col-sm-4 mt-3">
    <a href="https://www.nuro.ai/blog/unified-perception-model">
      <img src="/assets/img/nuro-unified-perception.gif" class="img-fluid rounded z-depth-1" alt="Nuro unified perception model" />
    </a>
  </div>
  <div class="col-sm-8 mt-3">
    <b>Unified Perception Model at Nuro</b><br />
    Prototyped and developed a DETR-based DNN model for online HD map construction, integrated within Nuro's unified camera–LiDAR BEV perception framework — accelerating the deployment of transformer-based perception models onto the road.<br />
    <a href="https://www.nuro.ai/blog/unified-perception-model">Blog</a>
  </div>
</div>

<div class="row">
  <div class="col-sm-4 mt-3">
    <a href="https://www.nuro.ai/blog/exploring-hd-mapping-that-scales">
      <img src="/assets/img/nuro-online-mapping.gif" class="img-fluid rounded z-depth-1" alt="Nuro scalable online mapping" />
    </a>
  </div>
  <div class="col-sm-8 mt-3">
    <b>Scalable Online Mapping at Nuro</b><br />
    Applied research on fusing prior HD map data with real-time sensor streams, significantly improving robustness and safety against real-world environmental and structural map changes — the foundation of our CVPR 2024 paper.<br />
    <a href="https://www.nuro.ai/blog/exploring-hd-mapping-that-scales">Blog</a>
  </div>
</div>

<div class="row">
  <div class="col-sm-4 mt-3">
    <a href="https://www.nuro.ai/blog/the-nuro-autonomy-stack">
      <img src="/assets/img/nuro-city-slam.gif" class="img-fluid rounded z-depth-1" alt="Nuro 3D city-scale SLAM" />
    </a>
  </div>
  <div class="col-sm-8 mt-3">
    <b>3D City-Scale SLAM System at Nuro</b><br />
    Built and maintained a highly reliable, multi-city-scale 3D mapping and SLAM pipeline — analyzing and optimizing scan matching and parallel graph optimization to unlock massive-scale, physics-grounded map building.<br />
    <a href="https://www.nuro.ai/blog/the-nuro-autonomy-stack">Blog</a>
  </div>
</div>
