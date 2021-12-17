---
title: NVDLA Meets PULP
summary: Semester's Thesis - Integrated Systems Laboratory (IIS) - ETH Zurich
tags:
- Hardware Acceleration
- Machine Learning
- SystemVerilog
- C++
- Industrial EDA Tools
date: "2019-06-17"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
#  caption: Photo by rawpixel on Unsplash
  focal_point: Smart

links:
url_code: ""
url_pdf: ""
url_slides: "uploads/nvdla-meet-pulp/slides.pptx"
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: 
---
Machine learning is the hot topic of the industry at the moment. Dedicated hardware accelerators for various deep learning tasks are being developed by many different parties for an equally diverse set of applications. IIS and the PULP project is no exception: they have developed multiple accelerators targeted at different forms and stages of deep learning, as well as processors that are computationally capable to perform such tasks.
NVIDIA of GPU fame has recently released their take on such an accelerator, called the NVIDIA Deep Learning Accelerator (NVDLA), which is an open source hardware accelerator aiming inference operations in Deep Learning applications. The purpose of this project is to implement NVDLA in a modern ASIC technology node and evaluate it in terms of performance, area, and power consumption.

After a configuration phase, simulation and synthesis can run on NVDLA via some EDA tools such as Synopsys VCS and Synopsys Design Compiler. Simulation works by feeding the hardware with tests (e.g., 8x8 convolution in int16 precision) in trace form. Synthesis has been carried out in UMC 65nm technology. The main obstacle during was to provide synthesizable macros, since NVDLA asks for many RAMs of unusual size which are not directly available in the library of the chosen technology node. Once the required modules have been manually coded, the synthesis outputs accurate area estimations: at 65nm, NVDLA occupies 34.86 mm2 , with an area efficiency of 29.3 Gop/mm2s. Working at 500MHz, it achieves a peak performance of 1 Tflop/s in fp16 precision or 2 Tflop/s in int8 precision. 
Preliminary power results have been obtained from a power analysis with Synopsys PrimeTime. Nevertheless, they are inconclusive due to simulation issues to be resolved in future work.
