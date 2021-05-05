---
title: Neural Style Transfer for Ultrasound Imaging
summary: Semester's Thesis - Computer Vision Lab (CVL) - ETH Zurich
tags:
- Computer Vision
- Machine Learning
- Python
- Tensorflow
date: "2020-07-20"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
#  caption: Photo by rawpixel on Unsplash
  focal_point: Smart

links:
url_code: "https://github.com/dmenini/nst-for-us-imaging"
url_pdf: "uploads/nst/report.pdf"
url_slides: "uploads/nst/slides.pptx"
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: 
---

The simulation of ultrasound images is fundamental for the training of specialized medical personnel.
This semester thesis aims to develop a post-processing step to improve quality and realism of simulated US images by using Neural Style Transfer. The idea is to transfer an high-quality US style or a clinical US style, depending on the task, on a simulated content image. 

We employ an optimization approach, which iteratively minimizes a weighted combination of style and content loss, and a learning approach, which consists of a neural network trained with a perceptual loss.
Moreover, we enrich these algorithms with some additions to take advantage of all the information available as input, e.g. segmentation maps and a whole dataset of style images. Eventually, the outcomes show that the learning approach outperforms the optimization approach. Indeed the output images better represent the input style, and the network runs in real-time on GPU. 
The best result achieves a SSIM of 70% on average, which is not optimal but is a good starting point for further research.
