---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "HGF for Behavioural Analysis: Perceptual Model for Anxiety Patients During Lockdown"
summary: "Course Project - Translational Neurmodeling - ETH Zurich"
authors: []
tags:
- Statistics
- Matlab
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

url_code: "https://github.com/dmenini/hgf-for-behavioural-anlysis"
url_pdf: "uploads/hgf/report.pdf"
url_slides: "uploads/hgf/slides.pptx"
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---
In this project we analyze via Hierarchical Gaussian Filtering (HGF) the relationship between State anxiety and the capacity of learning in a pool of subjects during COVID-19 lockdown. 

Our idea is to propose two tests to the subjects, simulating two scenarios: (1) an impartial context, where the subjects are exposed to a neutral scenario, where ideally there is not any preference or concern about the topic; (2) a psychologically involving context during COVID-19 lockdown, where the subjects are exposed to a possibly upsetting scenario, where they may be biased towards a certain belief based on their clinical conditions. These are the two behavioural tasks we took into account in our analysis. Moreover, a third test consisting of 20 questions from the ‘State-Trait Anxiety Inventory’ is used as ground truth to validate our data.
In order to reach a greater pool of subjects, the test is proposed as a mobile web application.

Eventually, we prove that all the subjects learn similarly in the impartial context, while they present clear differences in a psychologically involving context related to COVID-19. The analysis is performed in MATLAB
through HGF estimations, K-means clustering, response simulations, and all the results are consistent with our expectations.