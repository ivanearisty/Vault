# AI Midterm Cheat Sheet — Object Detection & Scene Understanding

> [!INFO] What the Professor Cares About (Priority Topics)
> - **“Why” questions**: Be able to explain *why* an algorithmic design choice exists (e.g., why Smooth L1 over L2, why IoU-based positives/negatives, why RPN improves trainability, why square-root width/height in YOLO, why NMS is needed, why AP/mAP and not accuracy).
> - **Detection metrics**: AP, mAP, Precision-Recall (PR) curves, interpolation variants, and how IoU thresholds shift the PR curve.
> - **Two-stage detectors**: R-CNN → Fast R-CNN → Faster R-CNN pipeline, training/inference flow, RoI Pooling, RPN + anchors, objectness vs. class scores, bbox regression.
> - **NMS**: The greedy procedure, per-class NMS, typical thresholds, and failure modes.
> - **Feature backbones**: Why ResNets enable deeper models; the concept of shortcut/residual blocks; the backbone vs. head concept.
> - **Scene understanding**: The hierarchy from classification → localization → detection → segmentation (instance/semantic); key datasets (COCO) and tasks.
> - **De-prioritize**: Fine-grained YOLO details beyond the high-level idea and its loss. Know the contrast with two-stage methods, but don’t over-index on YOLO internals.

---

## Object Detection Pipeline (Mental Model)

Use this to answer “walk-me-through” questions quickly.
1.  **Backbone CNN** extracts a feature map from the image.
2.  **Proposals** (Selective Search or RPN) generate candidate regions likely to contain objects.
3.  **Region Features** are extracted using RoI Pooling (or RoIAlign) to get a fixed `H×W` feature map for each proposal.
4.  **Heads** perform the final prediction:
    - **Classification head**: Predicts one of `K+1` classes (including background).
    - **BBox regression head**: Refines the proposal box coordinates.
5.  **Post-processing**: Filter detections by score → apply per-class NMS.

---

## R-CNN → Fast R-CNN → Faster R-CNN

### ➡️ R-CNN (Generation 1)
- **Process**: Selective Search → ~2k region proposals → Warp each region to CNN input size → Extract features → Train per-class linear SVMs → Bbox regressor refines boxes.
- **Drawbacks**: Extremely slow (CNN forward pass per proposal) and requires multi-stage training (pretrain, finetune, SVMs, regressor).
- **Key Concepts**: Why a background class is useful; how positives/negatives are chosen by IoU thresholds; why bbox regression is needed; the purpose of NMS.

### ➡️➡️ Fast R-CNN (Generation 2)
- **Process**: Run CNN on the *entire image* once to get a feature map → Map proposals to the feature map → Use **RoI Pooling** to get fixed-size features per region → Feed to two heads: (1) softmax classifier (`K+1` classes) and (2) bbox regressor (`4*K` outputs).
- **Loss Function**: Multi-task loss combining classification and localization.
  $$L = L_{cls}(\hat y, y) + \lambda [y\ge 1] L_{loc}(t_y, v)$$
- **Classification Loss**: Negative Log Likelihood.
  $$L_{cls} = -\log(\hat y_y)$$
- **Localization Loss**: Smooth L1 loss.
  $$L_{loc} = \sum_{i\in\{x,y,w,h\}} \text{smooth}_{L1}(t^i_y - v^i)$$

### ➡️➡️➡️ Faster R-CNN (Generation 3)
- **Innovation**: Replaces the slow Selective Search with a **Region Proposal Network (RPN)**, making the pipeline fully differentiable and end-to-end.
- **RPN**: Uses **anchors** (pre-defined boxes of various scales and aspect ratios) at each location on the feature map. It predicts an *objectness* score and *bbox deltas* for each anchor.
- **Advantage**: Much faster training/inference because proposals are generated from the feature map, not the raw pixels.

---

## Core Concepts

### RoI Pooling (Fast R-CNN)
- **What it does**: Projects a proposal from image coordinates to feature map coordinates. It then divides that projected region into a fixed grid (e.g., `7×7`) and max-pools each grid cell.
- **Why it matters**: It produces a fixed-length feature vector for any size proposal, enabling a shared backbone and efficient processing by the classification/regression heads. The max-pooling provides robustness to small misalignments.

### Region Proposal Network (RPN)
- **How it works**: A small network slides over the feature map. At each location, it evaluates `k` anchors (e.g., 3 scales × 3 aspect ratios = 9 anchors). For each anchor, it outputs:
    - **2k scores**: object vs. not-object (objectness).
    - **4k deltas**: box regression coordinates.
- **Why it's better**: It enables end-to-end learning, and the proposals are "tuned" to the features learned by the backbone, leading to higher quality proposals and a large reduction in computation.

### Non-Maximum Suppression (NMS)
- **Goal**: Remove duplicate bounding box predictions for the same object.
- **Procedure (per class)**:
    1. Sort all detection boxes by their confidence score.
    2. Take the box with the highest score and keep it.
    3. Suppress (remove) any other boxes that have an IoU with this box greater than a threshold `t_nms` (e.g., `0.3`).
    4. Repeat until no boxes are left.
- **Failure Mode**: In crowded scenes, NMS can accidentally suppress a true positive that is simply very close to another one. **Soft-NMS** is a variant that lowers scores instead of removing boxes entirely.

---

## Metrics: IoU, PR Curves, AP, mAP

- **IoU (Intersection over Union)**: Measures the overlap between a predicted box and a ground truth box. A threshold `t` (e.g., 0.5) determines if a prediction is a True Positive (TP) or False Positive (FP).
- **Precision-Recall (PR) Curve**: A plot of precision vs. recall generated by varying the score threshold for detections.
- **AP (Average Precision)**: The area under the PR curve. It provides a single number that summarizes the model's performance across all recall levels.
- **mAP (mean Average Precision)**: The average of the AP values across all object classes.

> [!TIP] Effect of IoU Threshold
> - **Higher `t`** (e.g., 0.75) is stricter. It leads to more FPs, lower recall, and generally a lower AP.
> - **Lower `t`** (e.g., 0.5) is more lenient. It leads to higher recall and often a higher AP.

---

## Common "Why?" Exam Questions

> [!QUESTION] Why Smooth L1 for bbox regression?
> It's less sensitive to large errors (outliers) than L2 loss, preventing exploding gradients. It acts like L2 for small errors (quadratic) and like L1 for large errors (linear).

> [!QUESTION] Why have a background class?
> It allows the model to explicitly learn what is *not* an object, reducing false positives and improving objectness discrimination.

> [!QUESTION] Why RPN over Selective Search?
> It's differentiable, allowing for end-to-end training. The proposals are adapted to the learned features, improving both accuracy and speed.

> [!QUESTION] Why NMS per class?
> Suppression should be class-conditional. A car prediction shouldn't suppress a nearby pedestrian prediction, even if they overlap.

> [!QUESTION] Why AP/mAP over accuracy?
> Object detection is a highly imbalanced problem (many potential negative boxes). AP/mAP correctly evaluates the model's ability to both **rank** detections (confidence scores) and **localize** them (via the IoU threshold).

> [!QUESTION] Why $\sqrt{w}$ and $\sqrt{h}$ in the YOLO loss?
> It gives more weight to errors in small boxes. A 5-pixel error is much more significant for a 20x20 box than for a 200x200 box. The square root helps normalize this effect.

> [!WARNING] Pitfalls & Common Gotchas
> - **NMS Threshold**: Too high → duplicate detections. Too low → suppresses correct detections in crowded scenes.
> - **Evaluation IoU**: Always state the IoU threshold (`t`) you are using for evaluation (e.g., AP@0.5). Conclusions can change dramatically at different thresholds.
> - **mAP vs. AP**: Reporting only mAP can hide catastrophic failures on individual classes. Always inspect per-class AP when possible.
