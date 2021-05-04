---
title: Real-time Reconstruction and Semantic Segmentation of Indoor Scenes 
summary: Master's Thesis - Computer Vision Lab (CVL) - ETH Zurich
tags:
- Computer Vision
- 3D Reconstruction
- Real-time
- 2D/3D Segmentation
- Machine Learning
- Python
- PyTorch
date: "2021-04-06"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
#  caption: Photo by rawpixel on Unsplash
  focal_point: Smart

links:
url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: 
---

Thanks to the wide availability of commodity RGB-D cameras, 3D reconstruction of indoor scenes by fusion of depth maps has become a central research topic in computer vision, especially for real-time settings. The major difficulty of this task is dealing with various amounts of noise and outliers from the range sensors, while being subject to memory and timing constraints. Anyway, the reconstructed geometry alone is not enough for a number of robotics applications that aims at a human-level scene understanding. While existing state-of-the-art approaches do not always take into account hardware limitations or online processing, we propose a real-time system for online reconstruction and semantic segmentation of indoor scenes.

Our reconstruction pipeline is based on RoutedFusion, which is a learning-based real-time capable system which fuses noisy depth maps from range sensors into a volumetric representation of the scene. Despite the good results obtained by RoutedFusion architecture, some problems are still evident, e.g., outlier surfaces, thickening artifacts along surface edges and problems with thin object structures. In this thesis, we demonstrate that the context information provided by the semantics of the scene can help the fusion network to learn noise resistant features which can better describe structure boundaries, thus mitigating the aforementioned problems. To the best of our knowledge, there are no previous works about the usage of per-pixel semantic predictions from a 2D CNN to improve the accuracy of the reconstructed geometry of the indoor scenes. 

We evaluate the results on our dataset, generated from the publicly available 3D scenes of the Replica Dataset. The analysis considers different resolutions of the input range images, architectures and implementations to find the best compromise among the precision of the reconstructed geometry and runtimes. Eventually, our optimal solution is able to integrate 37 frames per second in the volume, achieving an average reconstruction F1-score of 88% on the test scenes. However, depending on the application, one may desire to prioritize a faster solution that runs at 50 frames per second with a 84% F1-score, or a more accurate one that achieves 91\% F1-score at 10 frames per second. 

Eventually, by sacrificing some performances our system is also able to estimate in real-time the semantics of the reconstructed geometry with a mean Intersection-over-Union (IoU) of 51% by mapping the per-pixel semantic predictions from a 2D CNN into the volume.
