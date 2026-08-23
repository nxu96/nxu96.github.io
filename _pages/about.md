---
layout: about
title: About
permalink: /
subtitle: Senior Machine Learning Engineer at <a href="https://www.nvidia.com/">NVIDIA</a>.

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

I am a Senior Machine Learning Engineer at NVIDIA, working on world simulation for autonomous vehicles. Over the past six years, I have built spatial systems across the autonomy stack, spanning centimeter-accurate localization, online mapping, city-scale 3D SLAM, and neural scene reconstruction. This progression now drives a broader question in my work: **how can we build models that not only reconstruct environments but also simulate how they evolve through interaction?**

At NVIDIA, I helped bring [Instant NuRec](https://research.nvidia.com/labs/sil/projects/instant-nurec/) — a feed-forward 3D Gaussian Splatting model that turns a short multi-camera driving log into a fully simulatable 3D world — into NVIDIA's autonomous vehicle simulation stack. Previously, at [Nuro](https://www.nuro.ai/), I developed BEV-transformer models for online HD mapping and 3D SLAM systems operating at multi-city scale. At [Motional](https://motional.com/), I engineered centimeter-accurate localization for production robotaxis. I received my M.S. in Robotics from the University of Michigan, advised by [Prof. Chad Jenkins](https://web.eecs.umich.edu/~ocj/), and my B.E. from Beihang University.

I am particularly interested in action-conditioned world models, 3D/4D generative reconstruction, and data-driven simulation. I believe world models can create diverse, controllable, and closed-loop simulation environments for training and evaluating robots and autonomous vehicles at scale.

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
    Turning real-world driving logs into fully simulatable 3D Gaussian Splatting worlds for closed-loop AV simulation with NVIDIA Omniverse NuRec — from integrating the feed-forward Instant NuRec model into the production reconstruction pipeline to delivering the camera-only NuRec solution.<br />
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
    Prototyped and developed a BEV-transformer model for online HD map construction, integrated within Nuro's unified camera–LiDAR BEV perception framework — accelerating the deployment of multi-task perception models onto the road.<br />
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
