* **Matching / search → Dual encoders**
* **Generating text → Encoder–decoder**
* **Predicting next token → Decoder-only**
* **Images + text align ment → Contrastive learning**
# Architecture
## 1) **Dual-Encoder (CLIP-style)**
**Use:** image–text matching, retrieval, search
**Structure:**
* Image encoder: **ResNet / CNN OR ViT**
* Text encoder: **Transformer**
* Projection → **shared embedding space**
* Similarity: **dot product / cosine**
* Training: **contrastive loss (positive + negative pairs)**

![[Pasted image 20251218235441.png]]

## 2) **Encoder Backbones**
![[Pasted image 20251219094406.png | 500]]

* **CNN / ResNet** – spatial inductive bias
* **Vision Transformer (ViT)** – patch tokens + self-attention
* **ConvNeXt** – modern CNN alternative
## 3) **Text Encoder Variants**
![[Pasted image 20251219094529.png | 600]]
* **Transformer encoder (BERT-style)** – bidirectional context
* **Transformer decoder (GPT-style)** – causal masking (sometimes used in CLIP variants)
## 4) **Loss / Training Variants**

| Loss               | Normalization | Direction | Notes                            |
| ------------------ | ------------- | --------- | -------------------------------- |
| **InfoNCE**        | Softmax       | One-way   | Pulls positive vs all negatives  |
| **Symmetric CLIP** | Softmax       | Both ways | Standard CLIP loss               |
| **SigLIP**         | Sigmoid       | Pairwise  | Scales better, no batch coupling |
## 5) **Unified / Variant Architectures (Bonus)**
![[Pasted image 20251219094905.png | 500]]
* **Single-encoder multimodal (CLIPPO)**
  * Render text as pixels → one vision encoder
![[Pasted image 20251219095001.png | 500]]
* **Video-CLIP variants**
  * Extend encoders to temporal tokens
## 6) **Encoder–Decoder**
**Use:** captioning, translation, generation
**Structure:**
* Encoder → latent representation
* Decoder → autoregressive token generation
* Training: **teacher forcing**
🚫 *Not for retrieval / QS3 unless generation is required.*
![[Pasted image 20251219095043.png | 500]]
## 7) **Decoder-Only Models**
**Use:** language modeling
* Masked self-attention
* Next-token prediction (GPT)
🚫 *Not multimodal alignment.*
![[Pasted image 20251219095307.png]]