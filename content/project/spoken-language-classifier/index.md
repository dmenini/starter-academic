---
title: Spoken Language Classification
summary: Classify the language of the speaker between Swiss German, German, Italian, French and English
tags:
- Machine Learning
- NLP
- Python
date: "2021-11-01"

# Optional external URL for project (replaces project detail page).
external_link: ""

image:
#  caption: Photo by rawpixel on Unsplash
  focal_point: Smart

links:
url_code: "https://github.com/dmenini/spoken-language-classification"
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

In this project I train a spoken language classifier able to classify the language of a recording. Suported languages are Swiss German, German, Italian, French and English.

## Training process

The dataset is composed of 88k WAV files. The four main languages are part of VoxLingua107, which contains audios extracted from YouTube videos sampled at 16 kHz, while the swiss german audios are recordings from the FHNW Institute for Data Science Datasets, sampled at 44.1 kHz. I process the data by extracting mel-bank filters features from trimmed signals, and then I feed them to the model. 

The classifier is composed of an encoder based on the pretrained Huggingface's Speech2TextFeatureExtractor, which extracts state-of-the-art encodings, and then a lightweight classifier head to generate the final probabiities. The model is trained with Categorical cross-entropy loss for 50 epochs. The optimizer is Adam with Decoupled Weight Decay Regularization (AdamW). The scheduler decreases the learning rate linearly from the initial value (0.001) set in the optimizer to 0, after a warmup period during which it increases linearly from 0 to the specified learning rate.
The best model is selected according to the best value of the monitored validation metric (F1 score).

## Evaluation

Average scores on the test set:

| Precision  | Recall | F1     | IoU    |
| :--------: | :----: | :----: | :----: |
|   0.9297   | 0.9302 | 0.9296 | 0.8699 |


Per class F1 score:

| Swiss German  | English | Italian | French | German |  
| :-----------: | :-----: | :-----: | :----: | :----: |  
|   0.9845      | 0.8981  | 0.9100  | 0.9373 | 0.9179 | 

## ONNX Runtime

The project allows to export the model to ONNX and to run it with the ONNX runtime. Please, read
the [torch.onnx documentation](https://pytorch.org/docs/stable/onnx.html) to see a tutorial and get more info. In our
case, the model is converted to a static graph by tracing, i.e. the model is executed once with the given inputs and all
operations that happen during that execution are recorded.

Our experiments show that ONNX runtime is 2.5 times faster than the standard PyTorch
evaluation (on CPU), while preserving same accuracy and model size.

#### ONNX Optimization

ONNX Runtime provides three levels of graph optimizations (graph-level transformations) to improve model performance:

1. Basic (constant-folding, redundant node eliminations, semantics-preserving node fusions), applied before graph
   partitioning and thus independent on the CPU provider
2. Extended (complex node fusions), applied after graph partitioning and thus dependent on the CPU provider
3. Layout Optimizations (change data layout, e.g. NCHWc optimization)

All optimizations can be performed either online (right before inference) or offline (saving the optimized model on
disk). Our experiments show no significant improvements in the runtime by using optimizations.

#### ONNX Quantization

Quantization refers to techniques for doing both computations and memory accesses with lower precision data, usually
int8 compared to floating point implementations. In ONNX this is done by mapping the floating point real values to an 8
bit quantization space: VAL_fp32 = Scale * (VAL_quantized - Zero_point). Quantization enables performance gains such as:

- 4x reduction in model size
- 2-4x reduction in memory bandwidth
- 2-4x faster inference due to savings in memory bandwidth and faster compute with int8 arithmetic. The exact speed up
  depends on hardware and model: old machines may have few instruction support for byte computation.

Quantization does not however come without additional cost. Fundamentally quantization means introducing approximations
and the resulting networks have slightly less accuracy.

There are three types of quantization:

1. Dynamic quantization calculates the quantization parameter (scale and zero point) for activations dynamically.
2. Static quantization leverages the calibration data to calculate the quantization parameter of activations.
3. Quantization aware training calculates the quantization parameter of activation while training, and the training
   process can control activation to a certain range.

We experimented with dynamic quantization applied to the whole model, and with static quantization applied to different
subsets of operations, with the goal of finding a good compromise between model size, latency and accuracy. Regarding
runtime, we noticed that inference on a statically quantized model runs 15% faster w.r.t. a dynamically quantized model,
which instead don't show any particular improvement in runtime w.r.t. non quantized models.

In addition to quantization, we also converted all weights to float16 precision, while keeping inputs and outputs to
float32 precision. No loss in accuracy is registered, while the model size is halved (as expected).

|Quantization           | F1     | IoU    | Size (MB) |
|:--------------------: | :----: | :----: | :-------: |
|None                   | 0.9313 | 0.8729 |   188.4   |
|Dynamic on all ops     | 0.9309 | 0.8722 |    56.8   |
|Static on all ops      | 0.2821 | 0.1739 |    56.8   |
|Static on MatMul only  | 0.9298 | 0.8703 |    75.2   |
|Convert to float16     | 0.9315 | 0.8732 |    94.2   |