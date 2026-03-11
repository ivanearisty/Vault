# Extra Reading Summaries (Weeks 1-7)

---

## Week 1 -- Intro

### [Refresher on ML Basics](https://chinmayhegde.github.io/introml-notes-sp2020/)

Open-access lecture notes by Chinmay Hegde (NYU) for an Introduction to Machine Learning course. The site covers the following lectures:

1. **Overview** -- Foundational concepts: what ML is, the learning paradigm, types of learning problems, and course roadmap.
2. **Nearest Neighbors** -- Distance-based classification (k-NN); how predictions are made by majority vote among the k closest training examples; choice of distance metric and k.
3. **Linear Regression** -- Predicting continuous outputs via linear models; least-squares formulation; closed-form (normal equation) and iterative solutions.
4. **Gradient Descent and SGD** -- Optimization for training ML models; batch gradient descent, stochastic gradient descent (SGD), mini-batch SGD; convergence behavior and learning rate selection.
5. **Model Selection** -- Bias-variance tradeoff; overfitting vs. underfitting; cross-validation; regularization strategies (ridge, lasso).
6. **Logistic Regression** -- Probabilistic binary classification; the sigmoid function; maximum likelihood estimation; decision boundaries.
7. **Classification** -- General classification techniques; decision boundaries; multi-class strategies; evaluation metrics.
8. **SVMs** -- Support vector machines; maximum-margin classifiers; the hinge loss; soft-margin formulation for non-separable data.
9. **Kernel Machines** -- Nonlinear extensions via the kernel trick; common kernels (polynomial, RBF); the idea of implicit high-dimensional feature maps.
10. **Neural Networks** -- Multi-layer perceptrons; activation functions; forward propagation; backpropagation and gradient computation.
11. **ConvNets, RNNs** -- Convolutional neural networks for image data (shared weights, pooling); recurrent neural networks for sequential data.
12. **PCA** -- Principal component analysis for dimensionality reduction; eigenvalue decomposition; variance maximization viewpoint.
13. **Clustering** -- Unsupervised grouping; k-means algorithm; initialization and convergence; hierarchical clustering.
14. **Reinforcement Learning** -- Learning through reward signals; Markov decision processes; policy and value functions.
15. **Misc Topics** -- Additional specialized ML concepts and emerging directions.

---

### [A PyTorch Primer](https://github.com/chinmayhegde/dl-demos/blob/main/demo01-basics.ipynb)

A Jupyter notebook introducing PyTorch from the ground up, culminating in training a classifier on Fashion-MNIST.

#### Tensors

PyTorch's fundamental data structure is the **tensor** -- a multi-dimensional array analogous to NumPy's `ndarray`. Key operations demonstrated:

- **NumPy interop:** `torch.from_numpy(np_array)` converts a NumPy array to a PyTorch tensor.
- **Creation:** `torch.rand(rows, cols)` generates tensors filled with random values.
- **Linear algebra:** `torch.matmul(A, x)` performs matrix multiplication. Standard arithmetic (`+`, element-wise ops) works as expected.

#### Automatic Differentiation (Autograd)

PyTorch tracks operations on tensors to build a **computational graph** for automatic gradient computation:

- Create a tensor with `requires_grad=True` to signal that gradients should be tracked.
- Perform operations (matrix multiply, add bias, sum, etc.) -- PyTorch records each step.
- Call `.backward()` on the final scalar output to compute all gradients via the chain rule.
- Access gradients via the `.grad` attribute on leaf tensors.

```python
A = torch.rand(2, 2)
x = torch.rand(2, 1, requires_grad=True)
y = torch.matmul(A, x) + b
z = y.sum()
z.backward()       # computes dz/dx
print(x.grad)      # the gradient tensor
```

This eliminates the need for manual derivative calculations entirely.

#### Dataset Loading: Fashion-MNIST

The notebook uses **Fashion-MNIST** -- 60,000 training and 10,000 test grayscale images (28x28 pixels) of clothing items across 10 classes. Loaded via `torchvision.datasets.FashionMNIST` with `transforms.ToTensor()` to convert PIL images to tensors normalized to [0, 1].

#### DataLoaders

`torch.utils.data.DataLoader` wraps a dataset and provides:

- **Batching:** groups samples into mini-batches (e.g., `batch_size=64`).
- **Shuffling:** randomizes order each epoch (`shuffle=True` for training).
- **Iteration:** enables a simple `for images, labels in dataloader:` loop.

```python
trainDataLoader = DataLoader(trainingdata, batch_size=64, shuffle=True)
testDataLoader  = DataLoader(testdata, batch_size=64, shuffle=False)
```

#### Model Definition

A single-layer linear classifier (logistic regression) is built by subclassing `torch.nn.Module`:

```python
class LinearReg(torch.nn.Module):
    def __init__(self):
        super().__init__()
        self.linear = torch.nn.Linear(28*28, 10)

    def forward(self, x):
        x = x.view(-1, 28*28)   # flatten 28x28 image to 784-dim vector
        return self.linear(x)    # output 10 class scores
```

`torch.nn.Linear(in_features, out_features)` applies $y = xW^T + b$ with learnable weight matrix $W$ and bias $b$.

#### Loss Function and Optimizer

- **Loss:** `torch.nn.CrossEntropyLoss()` -- combines `LogSoftmax` and `NLLLoss`; standard for multi-class classification.
- **Optimizer:** `torch.optim.SGD(net.parameters(), lr=0.01)` -- stochastic gradient descent with learning rate 0.01.

#### Training Loop (20 Epochs)

Each epoch iterates through all mini-batches:

1. **Forward pass:** compute predictions `net(images)`.
2. **Loss:** compute `Loss(predictions, labels)`.
3. **Zero gradients:** `optimizer.zero_grad()` -- clears accumulated gradients from the previous step.
4. **Backward pass:** `loss.backward()` -- computes gradients of the loss w.r.t. all parameters.
5. **Update:** `optimizer.step()` -- adjusts parameters by $\theta \leftarrow \theta - \eta \nabla_\theta \mathcal{L}$.

After each epoch, the model is evaluated on the test set inside a `torch.no_grad()` context (disables gradient tracking for efficiency). Training loss decreases from ~0.96 to ~0.48 over 20 epochs.

#### Results

- Loss curves (training vs. test) are plotted with `matplotlib`, showing convergence.
- Sample test images are visualized alongside model predictions to qualitatively assess accuracy.

---

## Week 2 -- Neural Nets

### [History of Neural Nets](https://news.mit.edu/2017/explained-neural-networks-deep-learning-0414)

*By Larry Hardesty, MIT News, April 14, 2017.*

A narrative history of neural networks spanning seven decades, explaining why the field has cycled through periods of excitement and abandonment.

#### Origins (1944)

**Warren McCullough and Walter Pitts** (University of Chicago, later MIT) first proposed the neural network concept. Their contribution was primarily conceptual -- demonstrating that networks of simple neuron-like units could, in principle, compute any function a digital computer could. The framing treated the brain as a computing device.

#### The Perceptron (1957)

**Frank Rosenblatt**, a Cornell psychologist, built the **Perceptron** -- the first *trainable* neural network. It resembled modern networks structurally but had only **one adjustable layer** of weights between input and output. Despite this limitation, it could learn to classify simple patterns from labeled examples.

#### The 1969 Collapse

MIT's **Marvin Minsky** and **Seymour Papert** published *Perceptrons* (1969), proving mathematically that single-layer perceptrons could not compute certain functions (e.g., XOR) and that extending them would be "impractically time consuming." This had a devastating chilling effect on neural network research funding and interest for over a decade. MIT professor **Tomaso Poggio** notes this occurred during a period when symbolic AI and formal programming languages were ascendant.

#### The 1980s Renaissance

Researchers developed efficient algorithms for training **multi-layer** networks (notably backpropagation), removing the theoretical limitations Minsky and Papert had identified. Networks with multiple adjustable layers could now learn complex, nonlinear decision boundaries. Interest surged again.

#### The Early 2000s Decline

**Support vector machines (SVMs)** -- built on "clean and elegant mathematics" with strong theoretical guarantees -- displaced neural networks as the preferred ML method. Neural nets were seen as unprincipled and hard to analyze.

#### The Modern Deep Learning Era (2010s)

The resurgence was driven by **GPUs** (graphics processing units), originally developed for video games. GPUs pack thousands of simple processing cores onto a single chip, and their architecture is "remarkably similar to neural nets themselves." This hardware made training deep networks (10, 15, 50+ layers) computationally feasible for the first time.

#### How Neural Networks Work

- **Architecture:** Thousands or millions of simple nodes organized into layers, with connections flowing feed-forward (input to output).
- **Node operation:** Each node receives inputs from the layer below, multiplies each by a learned **weight**, sums the products, and fires (passes output forward) only if the sum exceeds a learned **threshold**. Otherwise it outputs nothing.
- **Training:** Weights and thresholds start random. Labeled training data is fed through the network; outputs are compared to correct answers; weights and thresholds are adjusted so that inputs with the same label produce similar outputs.
- **Convolutional networks:** The dominant architecture for vision tasks. Nodes cluster into overlapping groups, and each cluster feeds into multiple nodes in the next layer, enabling hierarchical feature detection.

#### The Opacity Problem

Even after successful training, the learned weights and thresholds are **"indecipherable"** -- researchers cannot easily determine what features the network recognizes or how it combines them. Understanding *why* a network makes a specific decision remains an active research challenge.

#### Open Theoretical Questions (CBMM Research)

Poggio and colleagues at MIT's Center for Brains, Minds, and Machines identified three key questions:

1. **Expressivity:** What range of computations can deep networks perform, and when do deeper networks provide advantages over shallow ones?
2. **Optimization:** Can we guarantee that training finds globally optimal weight settings?
3. **Generalization:** Why don't over-parameterized networks overfit catastrophically to training data?

#### Poggio's Analogy

Poggio compares scientific trends to flu strains: "There are apparently five or six basic strains of flu viruses, and apparently each one comes back with a period of around 25 years." Researchers "fall in love with an idea, get excited about it, hammer it to death," then move on -- creating cyclical patterns. The hope is that deep learning's current theoretical grounding will break this cycle.

---

### [Universal Approximation Theorem](https://towardsai.net/p/deep-learning/understanding-the-universal-approximation-theorem)

The Universal Approximation Theorem (UAT) is a foundational result guaranteeing that neural networks are, in principle, capable of representing any continuous function. The article explains the theorem's statement, intuition, constructive proof strategy, and practical limitations.

#### Formal Statement

Let $C(K)$ denote the set of continuous functions on a compact set $K \subseteq \mathbb{R}^n$. For any $f \in C(K)$ and any $\varepsilon > 0$, there exists a feedforward neural network $\hat{f}$ with a **single hidden layer** such that:

$$|f(x) - \hat{f}(x)| < \varepsilon \quad \text{for all } x \in K$$

The single-hidden-layer network computes:

$$\hat{f}(x) = \sum_{i=1}^{M} c_i \cdot \sigma(w_i^T x + b_i)$$

where $M$ is the number of hidden neurons, $c_i$ are output weights, $w_i$ and $b_i$ are hidden-layer weights and biases, and $\sigma$ is a nonlinear activation function.

**Requirements on $\sigma$:** must be non-constant, bounded, continuous, and monotonically increasing. Common qualifying activations include:

- **Sigmoid:** $\sigma(x) = \frac{1}{1 + e^{-x}}$
- **Tanh:** $\sigma(x) = \frac{e^x - e^{-x}}{e^x + e^{-x}}$
- **ReLU:** $\sigma(x) = \max(0, x)$ (extended results cover ReLU despite it being unbounded)

The theorem was first proved by **George Cybenko in 1989** for sigmoid activations, and later generalized.

#### Constructive Proof Intuition

The proof proceeds in three conceptual steps:

**Step 1 -- Sigmoids approximate step functions.** When the weight $w$ is made very large, the sigmoid $\sigma(wx + b)$ becomes extremely steep, approaching a step function that jumps sharply from 0 to 1 at the point $x = -b/w$. The bias $b$ controls *where* the step occurs; the weight $w$ controls how *sharp* it is.

**Step 2 -- Two step functions create a "bump."** By taking two sigmoid neurons with large weights but slightly different biases (step positions), and subtracting one from the other, you get a **bump (tower) function** -- a rectangular pulse that is high over a narrow interval and zero elsewhere. The output-layer weights $c_i$ control the *height* of each bump.

**Step 3 -- Combining bumps approximates any function.** Any continuous function on a compact domain can be approximated by a piecewise-constant (staircase) function. Each "stair" is one bump of appropriate position, width, and height. As the number of neurons $M$ increases, more bumps can be placed with finer widths, and the approximation $\hat{f}(x)$ converges to $f(x)$ within any desired $\varepsilon$.

This construction extends naturally to **multiple inputs** (bumps become hyper-rectangular regions in $\mathbb{R}^n$) and **multiple outputs** (separate output weights for each output dimension).

#### Key Limitations and Caveats

1. **Existence, not construction.** The theorem proves a suitable network *exists* but says nothing about how to *find* the right weights. As the article emphasizes: "just because we know a neural network exists that can (say) translate Chinese text into English, that doesn't mean we have good techniques for constructing or even recognizing such a network."

2. **Network size unspecified.** The required number of neurons $M$ could be impractically large -- potentially exponential in the input dimension for certain functions.

3. **Compactness requirement.** The result only holds on compact (closed and bounded) subsets of $\mathbb{R}^n$, not on all of $\mathbb{R}^n$.

4. **No generalization guarantee.** The theorem addresses approximation of a *known* target function, not generalization from finite training data to unseen inputs.

5. **Training difficulty.** Gradient-based optimization can get stuck in local minima or saddle points; the theorem does not guarantee that SGD (or any practical algorithm) will converge to the optimal weights.

6. **Overfitting risk.** A network with enough neurons to approximate a function perfectly on training data may memorize noise rather than learn the true underlying pattern.

---

## Week 3 -- Deep Neural Nets

- **[Why Momentum Really Works](https://distill.pub/2017/momentum/)**
  Challenges the common "ball rolling downhill" intuition for momentum in gradient descent. Through eigenvalue decomposition of convex quadratic functions, shows momentum achieves a quadratic speedup by square-rooting the condition number. The true dynamics are a damped harmonic oscillator (mass-spring system), where the momentum term acts as velocity that dampens oscillations. At optimal hyperparameters, convergence rate improves from $(κ-1)/(κ+1)$ to $(\sqrt{κ}-1)/(\sqrt{κ}+1)$.

- **[Learning Representations by Back-Propagating Errors](https://www.nature.com/articles/323533a0) (Rumelhart, Hinton, Williams, 1986)**
  Landmark Nature paper introducing backpropagation for training multi-layer neural networks by adjusting weights to minimize output error. Key contribution: hidden units can learn meaningful internal representations through gradient descent, distinguishing it from earlier methods like the perceptron convergence procedure. Widely regarded as one of the foundational works making modern deep learning possible.

- **[Deep Double Descent](https://openai.com/index/deep-double-descent/) (OpenAI, 2019)**
  Describes the "double descent" phenomenon across CNNs, ResNets, and Transformers: test performance first improves with model size, then degrades, then improves again. The error peak occurs at the "interpolation threshold" where the model barely has enough parameters to fit training data perfectly. Manifests in three dimensions: model-wise (more parameters), epoch-wise (longer training), and sample-wise (more data). Challenges both classical wisdom ("bigger models overfit") and naive modern intuition ("bigger is always better").

- **[Fit Without Fear](https://arxiv.org/abs/2105.14368) (Belkin, 2021)**
  Uses interpolation (fitting training data exactly, even when noisy) as a lens to understand why deep learning succeeds despite violating classical overfitting intuitions. Argues that over-parameterization is a feature, not a liability, enabling models to interpolate while still generalizing well. Provides a theoretical perspective explaining why massively over-parameterized networks do not simply memorize noise. Advances the case for rethinking the bias-variance tradeoff in modern ML.

---

## Week 4 -- DNNs and Convolution

- **[Visual Explanation of Convolution](https://gregorygundersen.com/blog/2017/02/24/cnns/)**
  Builds intuition for CNNs from scratch, demonstrating how sliding a kernel across an image is mathematically equivalent to using shared weights in a neural network layer. Illustrates 1D signal smoothing and 2D edge detection showing how filters reveal features in a location-invariant manner. Key insight: while a fully connected layer on a 256×256 image would require ~200,000 weights per neuron, convolutional weight sharing reduces this dramatically. Also covers pooling for translational invariance and how CNNs learn kernels automatically from data.

- **[LeCun's Classic LeNet Paper](http://vision.stanford.edu/cs598_spring07/papers/Lecun98.pdf) (LeCun, Bottou, Bengio, Haffner, 1998)**
  Introduces LeNet-5, the CNN architecture for handwritten digit recognition that became the blueprint for modern CNNs. Stacks convolutional layers (shared-weight feature maps), sub-sampling/pooling layers, and fully connected layers, all trained end-to-end with gradient descent and backpropagation. Systematically compares CNNs against other methods and demonstrates LeNet-5 outperforms all alternatives. Advocates for learning feature extractors from data rather than hand-engineering them -- now the dominant paradigm in deep learning.

---

## Week 5 -- CNNs

### [A Guide to Mask R-CNN](https://viso.ai/deep-learning/mask-r-cnn/)

#### Background: From CNN to Faster R-CNN

Standard CNNs work for classifying images with a single object but struggle with complex multi-object scenes. The R-CNN family progressively solved this:

- **R-CNN** uses region-based evaluation: it proposes bounding boxes across object regions, then evaluates a CNN independently on each Region of Interest (RoI) for classification. This is slow because it runs the CNN thousands of times per image.
- **Fast R-CNN** improves this with a two-stage procedure: (1) a Region Proposal Network (RPN) proposes candidate objects, and (2) features are extracted from candidate boxes using RoI Pooling, then classified with bounding-box regression. The CNN runs once on the full image.
- **Faster R-CNN** optimizes further by performing convolution only once per image to generate a feature map, then using an RPN (instead of selective search) to generate RoIs from that feature map. This produces two outputs per region: a class label and a bounding-box offset.

#### Mask R-CNN Architecture

Mask R-CNN extends Faster R-CNN by adding a **third output branch** that predicts a pixel-level object mask in parallel with the existing class label and bounding-box branches. The key innovation is **pixel-to-pixel alignment**, which was the critical missing piece from Fast/Faster R-CNN -- it enables fine spatial layout extraction of each object instance.

#### Semantic vs. Instance Segmentation

- **Semantic segmentation** classifies every pixel into a fixed set of categories but does not distinguish between individual object instances (e.g., all people are labeled "person" as one blob).
- **Instance segmentation** detects every object and segments each instance individually, distinguishing between objects even when they belong to the same category. Mask R-CNN performs instance segmentation.

#### Advantages

1. **Simple** to train relative to its capabilities
2. **High performance** -- outperforms single-model entries on every task it was tested on
3. **Efficient** -- adds only a small computational overhead to Faster R-CNN
4. **Flexible** -- generalizable to other tasks like human pose estimation

#### Applications

- **Satellite imagery**: detecting sports fields (baseball, soccer, tennis, football, basketball) in OpenStreetMap data; fields are ideal targets because they remain visible regardless of tree cover and have consistent geometry
- **Humanitarian mapping**: creating maps for aid organizations from satellite images
- **Autonomous vehicles** and **medical imaging** (tumor detection, coronavirus-related feature identification)

---

### [Deep Residual Learning for Image Recognition](https://arxiv.org/pdf/1512.03385) (He, Zhang, Ren, Sun -- 2015)

#### The Degradation Problem

As neural networks grow deeper, a counterintuitive problem emerges: accuracy saturates and then **degrades** -- not from overfitting (training error also increases), but from optimization difficulty. Even though a deeper network could theoretically copy a shallower one's weights and add identity mappings for extra layers, solvers fail to find these solutions in practice.

#### Core Idea: Residual Learning

Instead of learning a desired mapping $H(x)$ directly, the network learns the **residual** $F(x) := H(x) - x$. The original mapping becomes $F(x) + x$. The intuition: if the optimal function is close to identity, it is easier to push the residual toward zero than to learn an identity mapping through stacked nonlinear layers.

#### Identity Shortcut Connections

The building block is:

$$y = F(x, \{W_i\}) + x$$

The shortcut simply adds the input $x$ to the output of stacked layers. When dimensions differ (e.g., after downsampling), a linear projection is used: $y = F(x, \{W_i\}) + W_s x$. These identity shortcuts add **zero parameters** and negligible computation.

#### Network Architectures

**Plain baselines** follow VGG-style design: mostly 3x3 filters, with two rules: (i) same output size means same filter count; (ii) halved feature maps means doubled filters. The 34-layer plain baseline uses only 3.6 GFLOPS vs. VGG-19's 19.6 GFLOPS.

**Residual networks** insert shortcut connections into the plain baselines.

**Bottleneck design** (for deeper nets): replaces 2-layer blocks with 3-layer blocks (1x1, 3x3, 1x1) where the 1x1 layers reduce then restore dimensions, creating a bottleneck at the 3x3 layer.

| Depth | FLOPs | Design |
|-------|-------|--------|
| 18 | 1.8B | Basic 2-layer blocks |
| 34 | 3.6B | Basic 2-layer blocks |
| 50 | 3.8B | Bottleneck blocks |
| 101 | 7.6B | Bottleneck blocks |
| 152 | 11.3B | Bottleneck blocks |

ResNet-152 has **lower** complexity than VGG-16/19 despite being 8x deeper.

#### Training Details

ImageNet training: 224x224 random crops, standard color augmentation, batch normalization after each convolution, SGD with batch size 256, learning rate 0.1 (divided by 10 at plateaus), weight decay 0.0001, momentum 0.9, no dropout, 600K iterations.

#### Key Results -- ImageNet

**Plain networks degrade**: 34-layer plain (28.54% top-1) performs *worse* than 18-layer plain (27.94%), confirming the degradation problem. This is not vanishing gradients -- batch normalization prevents signal collapse.

**Residual networks improve with depth**:

| Model | Top-1 Error | Top-5 Error |
|-------|-------------|-------------|
| ResNet-34 | 24.19% | 7.40% |
| ResNet-50 | 22.85% | 6.71% |
| ResNet-101 | 21.75% | 6.05% |
| ResNet-152 | 21.43% | 5.71% |

ResNet-34 improves 3.5% over its plain counterpart. A single ResNet-152 (4.49% top-5) outperforms all previous ensemble results. The 6-model ensemble achieved **3.57% top-5 error**, winning ILSVRC 2015.

**Shortcut type comparison**: Zero-padding shortcuts (A: 25.03%), projection for dimension changes only (B: 24.52%), and all-projection (C: 24.19%) differ only marginally, proving identity shortcuts suffice.

#### CIFAR-10 Results

ResNet-110 achieves 6.43% error with only 1.7M parameters, beating Highway-19 (7.54%, 2.3M params) and FitNet-19 (8.39%, 2.5M params). A 1202-layer network trains without optimization difficulty but overfits on this small dataset (7.93% error) without regularization.

**Layer response analysis**: Deeper ResNets have smaller response magnitudes per layer, supporting the hypothesis that residual functions stay close to zero -- each layer makes only small modifications to the signal.

#### Object Detection Transfer (PASCAL VOC and MS COCO)

Using Faster R-CNN with ResNet-101 backbone vs. VGG-16:

| Dataset | VGG-16 | ResNet-101 | Gain |
|---------|--------|------------|------|
| VOC 2007 | 73.2% mAP | 76.4% mAP | +3.2% |
| VOC 2012 | 70.4% mAP | 73.8% mAP | +3.4% |
| COCO mAP@.5 | 41.5% | 48.4% | +6.9% |
| COCO mAP@[.5,.95] | 21.2% | 27.2% | +6.0% (28% relative) |

ResNets won COCO 2015 detection (37.4% mAP ensemble) and ILSVRC 2015 detection (62.1% mAP ensemble) and localization (9.0% top-5 error, 64% reduction over 2014).

#### Key Takeaways

1. Residual learning solves the degradation problem, allowing depth to consistently improve accuracy
2. Identity shortcuts are parameter-free yet sufficient for the framework
3. Improvements transfer broadly to detection, localization, and segmentation
4. Networks of 1000+ layers can be trained successfully, though regularization is needed for small datasets

---

## Week 6 -- Object Detectors and NLP Intro

### [YOLOv3: An Incremental Improvement](https://arxiv.org/pdf/1804.02767.pdf) (Redmon & Farhadi, 2018)

#### Bounding Box Prediction

YOLOv3 uses dimension clusters as anchor boxes. The network predicts four coordinates per box ($t_x, t_y, t_w, t_h$). Given cell offset $(c_x, c_y)$ and prior dimensions $(p_w, p_h)$:

$$b_x = \sigma(t_x) + c_x, \quad b_y = \sigma(t_y) + c_y$$
$$b_w = p_w \cdot e^{t_w}, \quad b_h = p_h \cdot e^{t_h}$$

Training uses sum-of-squared-error loss. An objectness score via logistic regression indicates ground-truth overlap. Only one anchor prior is assigned per ground-truth object (unlike Faster R-CNN which assigns multiple). Predictions with IoU > 0.5 are assigned; those below are ignored (no negative penalty).

#### Class Prediction

Uses **multilabel classification** with independent logistic classifiers and binary cross-entropy loss, rather than softmax. This handles overlapping labels (e.g., "Woman" and "Person" on the same box), which is important for datasets like Open Images.

#### Multi-Scale Predictions

YOLOv3 predicts at **three scales** using feature pyramid network concepts. The output tensor at each scale has dimensions $N \times N \times [3 \times (4 + 1 + C)]$ where $C$ is the number of classes (80 for COCO). Feature maps from earlier layers are upsampled 2x and concatenated with higher-resolution features, combining semantic and fine-grained spatial information. This process repeats for the third scale.

Nine anchor box priors are determined by k-means clustering, divided evenly across scales. For COCO: (10x13), (16x30), (33x23), (30x61), (62x45), (59x119), (116x90), (156x198), (373x326).

#### Feature Extractor: Darknet-53

A hybrid of YOLOv2's Darknet-19 and residual networks, using successive 3x3 and 1x1 convolutional layers with shortcut connections, totaling 53 convolutional layers.

| Backbone | Top-1 | Top-5 | BFLOPs | FPS |
|----------|-------|-------|--------|-----|
| Darknet-19 | 74.1% | 91.8% | 7.29 | 171 |
| ResNet-101 | 77.1% | 93.7% | 19.7 | 53 |
| ResNet-152 | 77.6% | 93.8% | 29.4 | 37 |
| **Darknet-53** | **77.2%** | **93.8%** | **18.7** | **78** |

Darknet-53 matches ResNet-152 accuracy while running **2x faster** and achieving the highest GPU utilization (BFLOP/s).

#### Performance on COCO

| Detector | Backbone | AP | AP50 |
|----------|----------|----|------|
| Faster R-CNN w/ FPN | ResNet-101-FPN | 36.2 | -- |
| SSD513 | ResNet-101 | 31.2 | -- |
| RetinaNet | ResNet-101-FPN | 39.1 | -- |
| RetinaNet | ResNeXt-101-FPN | 40.8 | -- |
| **YOLOv3 608x608** | **Darknet-53** | **33.0** | **57.9** |

YOLOv3 at 320x320 achieves 28.2 mAP in 22ms, matching SSD accuracy at **3x the speed**. On AP50, YOLOv3 reaches 57.9 in 51ms vs. RetinaNet's 57.5 in 198ms (**3.8x faster**). The weakness: YOLOv3 struggles with precise bounding-box alignment at higher IoU thresholds, explaining the gap in average AP but near-parity at AP50.

Multi-scale predictions reversed YOLO's historical weakness with small objects -- small-object AP improved substantially, though medium and large object performance was comparatively weaker.

#### Things That Did Not Work

- **Anchor box x,y offset predictions** (as width/height multiples with linear activation): decreased stability
- **Linear x,y predictions**: ~2 point mAP drop vs. logistic activation
- **Focal loss**: reduced mAP by ~2 points; the authors hypothesize YOLOv3's separate objectness prediction already addresses the class imbalance problem focal loss targets
- **Dual IoU thresholds** (Faster R-CNN style: 0.7 positive, 0.3 negative): did not improve results

#### Commentary on Metrics

The authors question COCO's emphasis on high-IoU precision, noting research showing humans cannot distinguish 0.3 from 0.5 IoU. They argue the metric switch from PASCAL VOC's AP50 was never justified and that bounding-box precision may not matter as much as classification accuracy. They express preference for segmentation masks over bounding boxes.

---

### [DeepDISC: Detection, Instance Segmentation, and Classification for Astronomical Surveys](https://arxiv.org/pdf/2307.05826) (Merz et al., 2023)

#### Overview

DeepDISC applies Facebook AI Research's **Detectron2** framework to astronomy, performing simultaneous object detection, deblending (separating overlapping sources), and classification on Hyper Suprime-Cam (HSC) imaging data. This is the first application of instance segmentation models to **real** (not simulated) astronomical survey images for these tasks.

#### Data and Setup

- **Training**: 1,000 images (1000x1000 pixels) from HSC Deep/UltraDeep fields
- **Ground truth**: generated using the `scarlet` deblending code combined with SExtractor detection
- **Three contrast scalings** tested: z-scale, Lupton, and high-contrast Lupton
- **Augmentation**: random flips, 90-degree rotations, 50% crops

#### Architectures Compared

**ResNet-based backbones**: R101c4, R101fpn, R101dc5, R50def, R50cas, X101fpn

**Transformer backbones**: MViTv2 (multi-head pooling attention) and Swin Transformer (shifted window attention)

#### Training Strategy

Transfer learning from MS-COCO/ImageNet-1k pretrained weights. Two phases: 15 epochs with frozen backbone (lr = 0.001), then 35 epochs unfrozen (lr = 0.0001, decayed 10x every 10 epochs). ResNets trained in ~3 hours on 2 V100 GPUs; transformers ~4 hours on 4 GPUs.

#### Key Results

**Detection and Deblending**:
- Transformer median bounding box IoU: **0.94** (up to 0.99), robust across all scaling methods
- ResNet median bounding box IoU: **0.75--0.81** depending on architecture
- Best segmentation mask IoU: ~0.64

**Classification (validated against HST COSMOS catalog)**:

| Class | Recall | Precision | F1 (Transformer) |
|-------|--------|-----------|-------------------|
| Galaxy | 99.6% | 99.2% | 0.99 |
| Stars | 85.4% | 91.5% | 0.87--0.88 |

Galaxy performance was stable across the full magnitude range. Stellar performance degraded gracefully, maintaining >80% precision to magnitude 25.

**Transformers vs. ResNets on AP**: Transformers achieved ~50--52 AP for galaxies and ~34--36 AP for stars, vs. ResNet best of ~46 AP (galaxies) and ~26 AP (stars).

#### Robustness to Preprocessing

A critical practical finding: **ResNets** were highly sensitive to contrast scaling choice (performance varied 10--40 AP points), while **transformers** were robust across scalings (variations < 3 AP points). This suggests transformers generalize better when deploying across surveys with different dynamic ranges.

#### Comparison to Existing Methods

The standard HSC extendedness classifier achieves ~100% galaxy purity but only ~50% stellar completeness and ~40% stellar purity at magnitude 25. Transformer DeepDISC achieves ~99% galaxy purity *and* ~99% completeness, with ~85% stellar completeness and ~91% purity -- a substantial improvement, especially for stars.

#### Label Challenges and Custom Metrics

Ground truth creation introduced artifacts: the 5-sigma detection threshold misses faint objects, and SExtractor over-deblends (shreds) extended or saturated objects. The authors developed modified precision/recall metrics that restrict evaluation to matched detections, isolating classification accuracy from detection completeness. Cross-matching with HST COSMOS data reduced classification label bias.

#### Significance

Demonstrates feasibility of integrating instance segmentation into astronomical survey pipelines as surveys approach LSST-scale data volumes. Transformer models can process hundreds of objects simultaneously with high accuracy and are robust to varying preprocessing -- a practical requirement for multi-survey deployment.

---

## Week 7 -- RNNs and Attention

### [The Recurrent Neural Network -- History and Development](https://com-cog-book.github.io/com-cog-book/features/recurrent-net.html)

#### Motivation: Why Sequences Matter

Previous architectures (MLPs, CNNs) treat data as perceived "all at once" and assume each sample is independent. But cognition fundamentally involves time: movement, speech, planning, and decision-making all unfold sequentially. RNNs introduce **memory** by allowing past states to influence future predictions.

#### Hopfield Networks (1982)

**John Hopfield** (Caltech) introduced energy-based recurrent networks with binary threshold units and symmetric bidirectional connections. Each unit connects to every other unit:

$$y_i = T\left(\sum w_{ji} y_j + b_i\right)$$

The network minimizes a global energy function, inspired by Hebbian learning ("neurons that fire together wire together"). The key concept is **content-addressable memory** (CAM): retrieving stored patterns from partial or corrupted input, mimicking how human memory works through association rather than explicit addressing.

#### Elman Networks (1990)

**Jeffrey Elman** (UC San Diego) posed the question: how should sequences and time be represented in neural networks?

**Two approaches**:
1. **Explicit**: map sequences spatially as a fixed-length vector $x = [x_1, x_2, \ldots, x_n]$. Drawbacks: fixed input size, conflates spatial location with temporal position, cannot distinguish relative vs. absolute position.
2. **Implicit** (Elman's approach): represent time through the network's own intermediate computations.

Elman added **context units** that store the previous timestep's hidden state. The hidden state update:

$$h_t = \sigma(W_{hh} h_{t-1} + W_{xh} x_t + b_h)$$

The context units "clone" hidden values from $t-1$ and feed them back at time $t$ through **trainable weights**. This allows the network to accept variable-length sequences without architectural changes.

**Empirical validation**: Elman trained on temporal XOR using 3,000-element sequences over 600 iterations. Error showed cyclic improvement (lower MSE every 3 outputs), demonstrating the network learned to predict the third element. Hidden-unit representations clustered semantically similar words together in hierarchical clustering -- evidence for RNNs as cognitive models of language processing.

#### The Vanishing and Exploding Gradient Problem

Identified by **Bengio et al. (1994)** and **Pascanu et al. (2012)**. When RNNs are unfolded over time, each timestep becomes a "layer." Gradients computed via repeated multiplication of weight matrices either:

- **Vanish**: weights < 1 cause products to shrink exponentially, so early timesteps barely receive gradient signal
- **Explode**: weights > 1 cause products to grow exponentially, destabilizing training

**Consequence**: RNNs cannot learn **long-term dependencies** -- relationships between elements far apart in a sequence.

#### Backpropagation Through Time (BPTT)

Standard backpropagation adapted for recurrent connections. For a 3-layer RNN computing $z_t = W_{hz} h_t + b_z$ and $h_t = \sigma(W_{hh} h_{t-1} + W_{xh} x_t + b_h)$:

Computing $\frac{\partial E_3}{\partial W_{hh}}$ requires summing contributions through all timesteps because weights are shared across time:
- Direct contribution from $h_3$
- Indirect contribution via $h_2$: $\frac{\partial h_3}{\partial h_2} \cdot \frac{\partial h_2}{\partial W_{hh}}$
- Further indirect paths through $h_1$, etc.

This chain of multiplications is precisely why gradients vanish or explode.

#### LSTM Networks (Hochreiter & Schmidhuber, 1997)

LSTMs solve the vanishing gradient problem by introducing units that combine short-term and long-term memory through **gating mechanisms**.

**Components**:
- **Memory cell** ($c_t$): long-term storage
- **Hidden state** ($h_t$): memory controller / short-term output
- **Three gates** controlling information flow

**Three decisions at each timestep**:

1. **Forget gate** -- is old information $c_{t-1}$ worth keeping?
$$f_t = \sigma(W_{xf} x_t + W_{hf} h_{t-1} + b_f)$$

2. **Input gate** -- is new information worth saving?
$$i_t = \sigma(W_{xi} x_t + W_{hi} h_{t-1} + b_i)$$
$$\tilde{c}_t = \tanh(W_{xc} x_t + W_{hc} h_{t-1} + b_c)$$

3. **Output gate** -- what elements of memory are relevant right now?
$$o_t = \sigma(W_{xo} x_t + W_{ho} h_{t-1} + b_o)$$

**Cell state and hidden state updates**:
$$c_t = (c_{t-1} \odot f_t) + (i_t \odot \tilde{c}_t)$$
$$h_t = o_t \odot \tanh(c_t)$$

**Why it works**: When forget gates do not erase information, gradients flow through the memory cell without repeated multiplication by weight matrices, counteracting the vanishing gradient problem.

#### Text Representation for RNNs

**Tokenization**: word-level, character-level, or n-grams.

**Encoding**:
- **One-hot**: unique binary vector per token (e.g., cat = [1,0,0], dog = [0,1,0]). Disadvantage: sparse and high-dimensional (50K vocabulary = 50K dimensions), no semantic relationships encoded.
- **Word embeddings**: dense real-valued vectors in lower dimensions (e.g., cat = [0.1, 0.8], dog = [0.2, 1.0]). Preserves semantic relationships. Can be learned during training or use pretrained embeddings (Word2Vec, GloVe).

#### RNNs in Cognitive Science

RNNs have been applied to model: object permanence (Munakata et al., 1997), text comprehension (St. John, 1992), word reading in quasi-regular domains (Plaut et al., 1996), recursive language structure (Christiansen & Chater, 1999), sequential human action (Botvinick & Plaut, 2004), and movement patterns in typical/atypical development. They are also used extensively in computational neuroscience for modeling brain function.

#### Historical Timeline

| Year | Contributor | Contribution |
|------|-------------|--------------|
| 1949 | Hebb | Hebbian learning principle |
| 1982 | Hopfield | Energy-based recurrent networks |
| 1986 | Jordan | Recurrent connections to memory units |
| 1990 | Elman | Context units with trainable recurrent weights |
| 1994 | Bengio et al. | Formal analysis of vanishing/exploding gradients |
| 1997 | Hochreiter & Schmidhuber | LSTM architecture |

#### Key Takeaways

1. RNNs introduce memory into neural networks -- past states influence future predictions via recurrent connections
2. The Elman network's implicit time representation (context units) is more flexible than explicit fixed-length encodings
3. Vanishing/exploding gradients are the fundamental obstacle to learning long-range dependencies
4. LSTM's gating mechanisms solve this by providing an uninterrupted gradient path through memory cells
5. RNNs successfully model human sequential cognition, providing both engineering tools and scientific models of the brain
