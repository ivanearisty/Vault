# Lecture 5: Object Detection - Deep Learning

## ConvNets in Computer Vision

Convolutional networks excel in computer vision applications due to two key properties: locality and shift invariance. Natural images exhibit spatial correlation where adjacent pixels convey information at multiple scales—from edges and corners to object parts and entire scenes.

While convnets provide shift invariance through design, they lack inherent scale and rotation invariance. However, stacking convolutional layers with pooling operations simulates multiple scales, addressing this limitation.

---

## Object Detection

Object detection extends beyond image classification by answering two questions: "what is in the image?" and "where is it located?"

### Model Interpretability

One approach involves explaining model decisions through techniques like:
- Class Activation Mapping (CAM)
- Gradient-based CAM (GradCAM)
- LIME (Linear Interpretable Model-Agnostic Explanations)

**GradCAM** computes neuron importance weights from feature maps and generates heatmaps highlighting regions influencing class predictions. However, "interpretable deep learning" remains contested regarding what it truly means.

### The Jaccard Index

Since accuracy metrics insufficiently measure localization quality, the **Jaccard Index** (Intersection-over-Union) evaluates bounding box predictions:

$$J(A,B) = \frac{|A \cap B|}{|A \cup B|}$$

Values range from 0 to 1, with 0.5 as a typical threshold for good predictions.

### Procedure

The general approach involves:

1. Identifying object presence via standard convnet classification
2. Identifying candidate regions (boxes) containing objects
3. Training a network to predict box edges matching ground-truth annotations

Bounding boxes are encoded as either (top-left, bottom-right) coordinates or as $(x, y, s, r)$ tuples representing center location, size, and aspect ratio.

The process generates **anchor boxes**—discrete candidates at various scales and ratios. Ground-truth boxes are matched to anchor boxes maintaining minimum IoU thresholds, creating training data with category labels and spatial offsets.

### Region Prediction Networks

An RPN architecture includes:

- **Base network**: Feature extractor (truncated VGG/ResNet)
- **Anchor box generation**: Prepares candidate boxes
- **Category prediction**: Conv layer with $N(k+1)$ channels for $N$ anchors and $k$ categories
- **Bounding box prediction**: Conv layer with $4N$ channels for offset values

Training uses cross-entropy loss for categories and Huber loss (L2 near zero, L1 elsewhere) for offsets, since Jaccard similarity is non-differentiable.

Common RPN variants include:
- SSD (Single-Shot Detection)
- R-CNNs
- Fast/Faster R-CNNs

### Alternative: Semantic Segmentation

Rather than predicting rectangular boxes, pixel-level category assignment provides higher localization precision. Tradeoffs include improved accuracy but significantly increased labeling difficulty and data requirements.
