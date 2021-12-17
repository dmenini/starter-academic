---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "GPU Acceleration of Inverted Index Search"
summary: Course Project - Systems-on-Chip for Data Analytics and Machine Learning - ETH Zurich
authors: []
tags:
- Python
- C
- CUDA
- Hardware Acceleration
categories: []
date: 2019-08-01

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

url_code: "https://github.com/dmenini/gpu-inverted-index-search"
url_pdf: ""
url_slides: "uploads/inverted-index-search/slides.pptx"
url_video: ""

# Slides (optional).
#   Associate this project with Markdown slides.
#   Simply enter your slide deck's filename without extension.
#   E.g. `slides = "example-slides"` references `content/slides/example-slides.md`.
#   Otherwise, set `slides = ""`.
slides: ""
---
Searching for a matching word into a large text database is a common problem in any scenarios, e.g. when searching on the web. In order to make this process faster, each text can be encoded in a more compact representation called Bloom filters, which take individual words as inputs and generate a fixed length bit vector with a fixed number of S set bits by means of hash functions. Bloom filters are conservative probabilistic data structures. They do not result in false negatives, but false positives may happen. The database is encoded in a "dictionary", which is a large matrix of encoded words.

Instead of using conventional hashing methods to generate the Bloom filters we propose an approach involving Hyperdimensional Computing. First, the letters in the alphabet are encoded as high-dimensional bit vectors of length D where only S ones are placed at random in the vector. The idea is that with a low S compared to D we obtain possibly orthogonal hypervectors for each base letter. To encode a word the letters' vectors are combined to obtain a Bloom filter of the same dimensionality as the base hypervectors. I will skip the specific details on the transformation. 

In a forward index search, to find out which text has a word that matches the query we have to look would have to compare (logical AND) the bits of the query with each row in the database.
A more interesting algorithm is the inverted index search: by simply rotating the dictionary a much more efficient access pattern can be exploited. 

We use a text database of originally 21000 texts from the HDC-Language-Recognition set. We implement the inverted index search both in Python to serve as a golden model, in C (running on CPU Intel Core i7-4790 @3.60GHz) and in CUDA (running on GPU NVDIA GeForce GTX 1080-Ti @1.734Ghz).
We evaluate the performances with varying database size. Eventually, our results show that the GPU implementation is twice faster than the CPU one. However certain sections of the code are computed faster on CPU, thus the best solution consist of an heterogeneous CPU-GPU implementation, which in the case of the largest database is 10 times faster than the basic CPU implementation.
