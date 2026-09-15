---
# ─────────────────────────────────────────────────────────────
#  PROJECT 2
#  Rename this file to your project's slug — the filename is the URL.
#  To add a third project later, just copy this file.
# ─────────────────────────────────────────────────────────────

order: 2
title: "Tiny CLIP Model"
tagline: "Image Classifier Trained on 5,000 Images"
status: Live

demo_url: "https://multimodal-ai-learning-rojo.streamlit.app"
repo_url: "https://github.com/raul-rojor/multimodal-ai-learning"

tech:
  - Python
  - NumPy
  - scikit-learn
  - Matplotlib
  - Transformers
  - Torch
  - Pillow
  - Streamlit

highlights:
  - "Evaluated on 500 images, the model captured the correct image within its top 2nd-3rd ranked images on average when given the image's caption (MRR = 0.3959)."
  - "Maximized contrastive learning results on a CPU by employing pretrained image/text encoders."

cover: /images/improved_embeddings.png
cover_plot: true
cover_label: "Improved model — embedding space"
cover_caption: >-
  The pretrained encoders of the improved model aligned image and text encodings
  into a shared region of the embedding space, setting the stage for better
  contrastive learning.

facts:
  - label: Role
    value: "Solo build"
  - label: Timeline
    value: "Summer 2026"
  - label: Data
    value: "COCO dataset: 5,000 image-caption pairs"
---

## The problem

Prior to this project, I had no experience with machine learning algorithms. Having gained an interest in specifically multimodal AI alignment, I had no technical background in the field which greatly limited my ability to explore it. I needed a way to learn about multimodal models and I wanted to witness their construction and capabilities first-hand.

## Approach

I began my exploration of multimodal AI by having an LLM create learning files for me on key topics. I followed along by taking notes in a journal and filling out practice files. After learning the concepts needed, I worked with the LLM to create and train custom image and text encoders before applying contrastive learning between the two. The model was trained using images and captions from the COCO dataset, and its outputted captions to classify images are limited to 80 COCO image classes for model benchmarking reasons. After testing image classification results and visualizing modality alignment in the encoding space, the model's poor outcomes led me to consider methods of improvement. While the CPU greatly limited training capabilities, I wanted to improve the model without relying on greater computing power. The dilemma produced a worthwhile exploratory question: "does further training the encoders of individual modalities lead to improved contrastive learning results between the modalities?" To provide enough evidence to reach an answer, I swapped out my custom encoders with pretrained ResNet (image) and DistilBERT (text) encoders while keeping the contrastive learning mechanism untouched. While the low processing power issue remained, keeping only one variable independent made for stronger takeaways from the contrasting model results.

## Results

Given 500 images and their corresponding captions, the model was given one of the image's description and was tasked with ranking the 500 images on how well they match the caption.

| Metric | Random Baseline | Custom Encoders Model | Pretrained Encoders Model |
| --- | --- | --- | --- |
| Recall@1 (How often the correct image was ranked 1) | 0.0020 | 0.0000 | 0.2320 |
| Recall@5 (How often the correct image was ranked in top 5) | 0.0100 | 0.0100 | 0.5960 |
| Recall@10 (How often the correct image was ranked in top 10) | 0.0200 | 0.0140 | 0.7680 |
| MRR (1 / Average ranking of correct image) | 0.0040 | 0.0116 | 0.3959 |

{% include figure-pair.html
     plot=true
     src1="/images/custom_embeddings.png"
     alt1="PCA plot of the custom model's embedding space, with image and text encodings in two separate clusters"
     label1="Custom encoders"
     src2="/images/improved_embeddings.png"
     alt2="PCA plot of the improved model's embedding space, with image and text encodings spread across the same region"
     label2="Pretrained encoders"
     caption="The pretrained encoders of the improved model aligned image and text encodings far more closely than those of the custom model, which split the two modalities into separate clusters. This set the stage for better contrastive learning in the improved model." %}

{% include figure.html
     tall=true
     src="/images/tiny-clip-demo.png"
     alt="The deployed TinyCLIP app classifying an uploaded photo of a cat, with cat as the top prediction"
     label="Deployed app"
     caption="The model in use: an uploaded image is scored against the 80 COCO class names, and the top five zero-shot predictions are returned. Confidence stays low even when the ranking is right which is an expected consequence of training on a small dataset." %}

## What I'd do differently

With more time on this project, I'd swap my CPU usage for GPU processing to allow for more training epochs, higher-dimension encodings, and training on more data. Using pretrained encoders improves encodings alignment between modalities, but only with more data and training can the gap to genuine zero-shot classification accuracy be closed. Furthermore, with a more accurate model, I would unrestrict zero-shot classification from 80 classes to allow for more natural image captioning of users' images.
