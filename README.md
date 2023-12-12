# Learning interpretable single-cell morphological profiles from 3D Cell Painting images

[**Vivek Gopalakrishnan**](vivekg.dev),
[Jingzhe Ma](https://www.linkedin.com/in/jingzhe-ma),
[Zhiyong Xie](https://scholar.google.com/citations?user=0DsebPAAAAAJ)
\
*Society of Biomolecular Imaging and Informatics (SBI2)*, 2023
\
**Winner of the 2023 SBI2 President's Innovation Award**

## Abstract
Quantifying the phenotypic effects of experimental perturbations from high-throughput imaging assays, a feature extraction process known as morphological profiling, is a necessary and challenging step in the analysis of Cell Painting data. Whereas traditional methods use handcrafted image processing algorithms to extract human-designed descriptors of cellular morphology, recent methods have taken a deep learning approach, training neural networks to extract features learned to be relevant from raw imaging data. While learned morphological profiles have been shown to enable better performance in downstream analysis tasks, the question of interpretability limits deep learning-based approaches: how can we ensure that the morphological profiles extracted by black-box deep learning models actually capture biologically relevant information about single cells, rather than exploiting confounding factors present in image data to minimize training loss (e.g., batch effects)? To address this uncertainty, we propose combining morphological profiles obtained using supervised learning with Gradient-weighted Class Activation Mapping (Grad-CAM), a technique that uses the gradients produced by a deep learning model to localize which regions of an input image the model paid the most attention to when making its prediction. Using single-cell segmentations produced by Cellpose (a generalist segmentation algorithm), we can measure, for each learned morphological profile, what proportion of the model’s attention is concentrated on the cell of interest versus the background. Using this interpretability metric, we can identify which morphological profiles capture biologically relevant components of the input image, helping to visualize the influence of confounders on the extracted features. Using a 3D convolutional neural network, we demonstrate how to scale this technique to 3D Cell Painting images: in a dataset of single-cell z-stacks, we find that only 47% of learned morphological profiles have Grad-CAMs that overlap with the cell’s segmentation map. This demonstrates that supervised feature extractors can cheat by exploiting non-biological information in microscopy data. Motivated by this disadvantage of supervised learning, we also explore self-supervised approaches for learning interpretable morphological profiles from single-cell 3D Cell Painting images.

![‎abstract](https://github.com/eigenvivek/zlearn/assets/29757116/1760a9d8-3e9f-4df3-aaee-5d9df811fb58)
> Given a 3D Cell Painting z-stack, we first use [`cellpose`](https://github.com/MouseLand/cellpose) to segment individual cells. Using segmentation masks, we create single cell crops.

## Supervised feature extraction

![efficientnet](https://github.com/eigenvivek/zlearn/assets/29757116/e341d1f8-0dae-4153-a669-3494fe921744)

## Where is the model looking?

<img width="1892" alt="image" src="https://github.com/eigenvivek/zlearn/assets/29757116/7f325a79-d142-49c5-840b-32a9a1ae5f49">
<img width="1892" alt="image" src="https://github.com/eigenvivek/zlearn/assets/29757116/a62fc597-bb25-4d93-9e50-cdd0f103f980">

## Grad-CAMO: a new interpretability metric

<img width="1167" alt="image" src="https://github.com/eigenvivek/zlearn/assets/29757116/e15ef478-6f86-4aa1-b54d-4dff19634962">

## Cell Painting channels are highly correlated

<img width="1179" alt="image" src="https://github.com/eigenvivek/zlearn/assets/29757116/f3b1f1f8-c33e-4bbd-84fa-22e240bb7ac5">
