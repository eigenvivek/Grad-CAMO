# Learning interpretable single-cell morphological profiles from 3D Cell Painting images

[**Vivek Gopalakrishnan**](https://vivekg.dev/),
[Jingzhe Ma](https://www.linkedin.com/in/jingzhe-ma),
[Zhiyong Xie](https://scholar.google.com/citations?user=0DsebPAAAAAJ)
\
*Society of Biomolecular Imaging and Informatics (SBI2)*, 2023
\
**Winner of the 2023 SBI2 President's Innovation Award**

## Abstract
Quantifying the phenotypic effects of experimental perturbations from high-throughput imaging assays, a feature extraction process known as morphological profiling, is a necessary and challenging step in the analysis of Cell Painting data. Whereas traditional methods use handcrafted image processing algorithms to extract human-designed descriptors of cellular morphology, recent methods have taken a deep learning approach, training neural networks to extract features learned to be relevant from raw imaging data. While learned morphological profiles have been shown to enable better performance in downstream analysis tasks, the question of interpretability limits deep learning-based approaches: how can we ensure that the morphological profiles extracted by black-box deep learning models actually capture biologically relevant information about single cells, rather than exploiting confounding factors present in image data to minimize training loss (e.g., batch effects)? To address this uncertainty, we propose combining morphological profiles obtained using supervised learning with Gradient-weighted Class Activation Mapping (Grad-CAM), a technique that uses the gradients produced by a deep learning model to localize which regions of an input image the model paid the most attention to when making its prediction. Using single-cell segmentations produced by Cellpose (a generalist segmentation algorithm), we can measure, for each learned morphological profile, what proportion of the model’s attention is concentrated on the cell of interest versus the background. Using this interpretability metric, we can identify which morphological profiles capture biologically relevant components of the input image, helping to visualize the influence of confounders on the extracted features. Using a 3D convolutional neural network, we demonstrate how to scale this technique to 3D Cell Painting images: in a dataset of single-cell z-stacks, we find that only 47% of learned morphological profiles have Grad-CAMs that overlap with the cell’s segmentation map. This demonstrates that supervised feature extractors can cheat by exploiting non-biological information in microscopy data. Motivated by this disadvantage of supervised learning, we also explore self-supervised approaches for learning interpretable morphological profiles from single-cell 3D Cell Painting images.

![abstract](https://github.com/eigenvivek/Grad-CAMO/assets/29757116/b75775ad-e37c-4282-9961-4b7e18ba6c7b)

> Given a 3D Cell Painting z-stack, we first use [`cellpose`](https://github.com/MouseLand/cellpose) to segment individual cells. Using segmentation masks, we create single-cell 3D crops. Adapting approaches commonly used for 2D Cell Painting images (e.g., [`DeepProfiler`](https://github.com/cytomining)), we train a [3D EfficientNet](https://github.com/shijianjian/EfficientNet-PyTorch-3D) to predict the treatment label of an individual cell. Intermediate network activations can be used as single-cell features.

## Supervised feature extraction
We use `UMAP` to visualize the features extracted via supervised learning. The is trained and tested on disjoint wells and UMAPs are performed on the test images. Following [`DeepProfiler`](https://github.com/cytomining), we use the sphering transform to mean-correct the embeddings.

![efficientnet](https://github.com/eigenvivek/zlearn/assets/29757116/e341d1f8-0dae-4153-a669-3494fe921744)
> UMAP embeddings generated over the training trajectory of the `EfficientNet`. Supervised training is very fast, taking ~2 hours on a single 2080Ti GPU to produce this GIF.

## Where is the model looking?

These embeddings are highly seperable! Good clusters means good feature vectors... right?

**No.** Using [`Grad-CAM`](https://github.com/jacobgil/pytorch-grad-cam), we can visualize where the `EfficientNet` is looking in the image to make its prediction. For the examples below, we can see that the model sometime looks at the cell of interest, sometimes at its neighbors, and sometimes *at the image backround where no biological information is present*. This evidence of model cheating demonstrates that there is confounding information in the learned feature vectors.

![where](https://github.com/eigenvivek/Grad-CAMO/assets/29757116/f1b88793-44af-4298-bb5f-07f6332f6a75)
> Red areas are where the model is paying the most attention to when making its prediction.

## Grad-CAMO: a new interpretability metric

To quantify the biological relevance of a learned feature vector, we propose combining the Grad-CAM map with the single-cell segmentations produced by `cellpose`. Our proposed metric, Grad-CAM Overlap (`Grad-CAMO`), produces a quality score for every single-cell embedding where 0 is the worst and 1 is the best.
**In future work, `Grad-CAMO` could be used as a regularizer when training supervised feature extractors.**

![gradcamo](https://github.com/eigenvivek/Grad-CAMO/assets/29757116/b84f588b-9384-4e56-bf6a-8a09c45bc3b1)
> We find that only 30% of learned morphological profiles have Grad-CAMs that significantly overlap with the cell’s segmentation map in our dataset (>50% overlap).

## Cell Painting channels are highly correlated

<img width="1179" alt="image" src="https://github.com/eigenvivek/zlearn/assets/29757116/f3b1f1f8-c33e-4bbd-84fa-22e240bb7ac5">
> Evaluations performed on the open-source [`RxRx1`](https://www.rxrx.ai/rxrx1) dataset.
