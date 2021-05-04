---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Lightweight Face Detection on Wearable Microcontroller"
summary: Course Project - Machine Learning on Microcontrollers - ETH Zurich
authors: []
tags:
- Computer Vision
- Machine Learning
- Real-time
- Python
categories: []
date: 2020-05-01

# Optional external URL for project (replaces project detail page).
external_link: ""

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Custom links (optional).
#   Uncomment and edit lines below to show custom links.
# links:
# - name: Follow
#   url: https://twitter.com
#   icon_pack: fab
#   icon: twitter

url_code: ""
url_pdf: ""
url_slides: ""
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---
Nowadays face detection is commonly performed by leveraging machine learning. However, most methods are based on deep neural networks that require great amounts of memory to store the parameters and they are not real-time capable. In this project we propose a lightweight neural network that fits the memory of a wearable microcontroller (ARM Cortex-M7) and it is able to detect faces in real-time with good precision. 

The starting model is the Multi-Task Cascaded Convolutional Neural Network (MTCNN) by Zhang et al. which both detects faces (i.e. face classification and bounding box regression) and recognizes facial landmarks. The MTCNN consists of 3 cascaded CNNs (Propose-Network, Refine-Network and Output-Network) wich work iteratively on scaled versions of the input image. Since facial details are unnecessary in our application, we opted to remove the O-Net; moreover, we empirically proved that removing also the R-Net did not affect much the final score. To avoid an eccessive drop in accuracy we still perform the three iterations at different input scale, even though the starting resolution is already small (i.e., 90x60x30) in order to fit the MCU memory and to keep the runtime fast. 
The resulting lightweight model is trained using the WIDER Face Dataset and then flashed on the ARM Cortex-M7 by means of STM X-CUBE-AI. The MCU is only used to run inference, while the image preprocessing (i.e., gaussian filtering and downsizing) and postprocessing (i.e., outlier filtering) are performed on the host device.

To evaluate the model we use the IoU-F1 metric, which measures the percentage of faces correctly detected (with a ±1 tolerance in our case). Our solution results in a 76.6% average IoU-F1, compared to the 86.6% of the original model, and the inference runtime is 363.6ms with an efficiency of 7 CPU cycles per MAC.