
# [[Convolutional Neural Networks OLD]]

add using covnets with small datasets

explain what i need to know do sketch networks

![[Screenshot 2025-10-17 at 2.56.58 AM.png]]

## 1) Output size & parameter count (fast formulas)

- **Conv 2D output spatial size** (for H×W input, kernel (K), padding (P), stride (S)):  
    (H_{out}=\left\lfloor\frac{H+2P-K}{S}\right\rfloor+1,\quad W_{out}=\left\lfloor\frac{W+2P-K}{S}\right\rfloor+1)
    
- **#Params (no groups):** (K_hK_w,C_{in},C_{out} + C_{out}) (bias optional)
    
- **FLOPs (≈ MACs×2)**: (2\cdot H_{out}W_{out},K_hK_w,C_{in},C_{out})
    

> **Exam pattern:** “Given H,W,K,S,P and channels, compute output shape/parameters/FLOPs.”

---

## 2) Padding, stride, dilation (how to explain)

- **Same padding** preserves spatial size (when (S=1)) if (P=\lfloor K/2\rfloor).
    
- **Stride (>1)** downsamples; **dilation** enlarges receptive field without extra params.
    

---

## 3) Receptive field (quick propagation rule)

Track per layer (r,,j) (RF size, effective jump):

- Start: (r_0=1,\ j_0=1)
    
- Layer with kernel (k), stride (s):  
    (j_\text{new}=j\cdot s,\quad r_\text{new}=r + (k-1)\cdot j)
    

> **Exam pattern:** “What RF size after stack (conv/pool)?” Use the rule top-to-bottom.

---

## 4) Pooling & downsampling

- **Max/avg pool**: reduces spatial dims, increases RF, adds invariance; no params.
    

---

## 5) Residual & bottleneck CNN blocks (connect to DNN ideas)

- **Residual block function:** learns (F(x)) with skip (x\to x+F(x)).
    
- **Bottleneck:** (1\times1) reduce → (3\times3) process → (1\times1) restore; cuts params/compute.  
    _Why used:_ enables **very deep** nets without exploding cost/vanishing gradients.
    

---

## 6) Normalization in CNNs

- **BatchNorm2d** over (N,H,W) per channel → stabilizes and speeds up training.
    
- **Layer/Group Norm** when batches are tiny or for certain architectures.
    

---

## 7) Typical head & losses

- **Classifier head:** GAP (global avg pool) → FC → softmax, CE loss.
    
- **Detector/segmenter heads:** add regression terms (smooth-L1, IoU), pixel-wise CE or Dice.
    

---

## 8) Why CNNs work (one paragraph answer)

- **Local connectivity + weight sharing → translation equivariance**, strong **inductive bias** for images; fewer parameters than fully connected layers at equal resolution, better sample efficiency; deeper stacks build **hierarchies** (edges → motifs → parts → objects).
    

---

## 9) Contrastive/metric learning with CNNs (diagram you can sketch)

- **Siamese/twin CNNs** (f(\cdot)) with shared weights; feed image pairs/triplets, compute embedding distances; train with **contrastive** or **triplet** loss; **inference** via k-NN/threshold in embedding space.
    

---

## 10) Common quick justifications

- **Why residual connections?** Deeper nets trainable; identity path preserves gradient.
    
- **Why bottlenecks?** Same representational power with far fewer params/latency.
    
- **Why BN before/after ReLU?** Either appears in literature; most modern stacks use **Conv → BN → ReLU** (or **Pre-Act** ResNets with BN/ReLU before conv).
    
- **Why GAP instead of FC stacks?** Fewer params, reduced overfitting, spatially robust.
    

---

## 11) Mini checklists for typical exam prompts

**A) “Compute shape/params/FLOPs for this block”**

1. Compute (H_{out},W_{out}) with stride/pad.
    
2. Params (=K_hK_wC_{in}C_{out} (+C_{out})).
    
3. MACs (=H_{out}W_{out}K_hK_wC_{in}C_{out}) (×2 for FLOPs).
    

**B) “Explain residual block function”**  
State (y=x+F(x)); the block learns a **residual** (F(x)=h(x)-x); helps optimization in deep nets.

**C) “Why bottleneck in ResNet-50?”**  
Use (1\times1) to compress/expand channels → huge parameter/latency savings while keeping depth.

**D) “Explain a contrastive loss diagram & inference”**  
Twin CNNs → embeddings → loss that **pulls same class** and **pushes different classes** beyond a margin; at test time, **k-NN** or distance thresholding.

---

## 12) Speed answers (one-liners you can drop in)

- **ERM vs. true risk:** (L_e) averages over the **finite** training set; (L) is expectation over the **data distribution**. Aim: learn (g) with small (L) using (L_e).
    
- **Why nonlinearity?** Without it, stacked linear layers = one linear map.
    
- **Why BN?** Stabilizes distributions, permits larger LR, faster convergence.
    
- **Why dropout?** Implicit ensembling → better generalization.
    
- **Translation equivariance?** Convolution + shared kernels → shifts in input shift feature maps.
    

---

## 13) Tiny worked example (shape + params)

**Input:** (64\times64\times 32) → **Conv** (3\times3), stride 1, pad 1, (C_{out}=64).

- (H_{out}=W_{out}=64)
    
- **Params:** (3\cdot3\cdot32\cdot64 + 64 = 18{,}496)
    
- **FLOPs (≈):** (2\cdot 64\cdot64\cdot3\cdot3\cdot32\cdot64 \approx 1.5\times10^9)
    

---

## 14) When to mention pitfalls

- **Vanishing/exploding gradients:** use residuals, good init, BN, appropriate activations.
    
- **Overfitting on small data:** use aug, weight decay, dropout, early stopping, transfer learning.
    
- **Tiny batches:** prefer Group/Layer Norm or accumulate grads.
    

---

If you want, I can drop this into **Obsidian-friendly markdown** with your preferred front-matter and exam tags.