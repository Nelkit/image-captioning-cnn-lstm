# 🖼️ Image Captioning with CNN + LSTM — VizWiz Dataset

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=flat&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?style=flat&logo=pytorch&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat&logo=jupyter&logoColor=white)

**UTS Deep Learning · Assignment 3 · MSc Data Science & Innovation**

Two PyTorch image captioning architectures trained and evaluated on the [VizWiz-Captions](https://vizwiz.org/tasks-and-datasets/image-captioning/) validation set (7,750 images taken by people who are blind). Compares a lightweight MobileNetV3 + LSTM baseline against a GoogLeNet + LSTM with Luong spatial attention.

---

## 📁 Repository Structure

```
dl-image-captioning-at3/
├── notebooks/
│   ├── shared/           # Phase 1 — shared data preparation (run once per group)
│   └── students/         # Phase 2 & 3 — individual notebooks per student
├── checkpoints/          # Saved model weights (not tracked by git)
├── reports/              # Final group report
├── docs/                 # Supplementary documentation
└── README.md
```

> All notebooks are self-contained and run on Google Colab. Dependencies are installed inside each notebook. Dataset files live in the shared Google Drive folder.

---

## 🧪 My Models — Nelkit Chavez

### Model 1 — MobileNetV3-Small + LSTM (Phase 2)

A lightweight, controlled baseline following the Show and Tell architecture (Vinyals et al., 2015).

**Encoder:** MobileNetV3-Small pretrained on ImageNet. Features extracted via global average pooling (576-d), projected to 256-d via Linear → BatchNorm → ReLU. Partial unfreezing: `features[0:9]` frozen, `features[9:12]` trainable to allow domain adaptation to VizWiz's challenging images (blurry, poorly framed, low-lit) without catastrophic forgetting.

**Decoder:** Single-layer LSTM, no attention. Embedding dim: 256, hidden size: 512, vocabulary: 3,965 tokens.

**Training:** Adam optimizer, encoder lr=3e-5 / decoder lr=3e-4, batch size 32, 10 epochs, ReduceLROnPlateau scheduler, dropout 0.5, gradient clipping at norm 5.0. Beam search k=3 at inference.

---

### Model 2 — GoogLeNet + LSTM + Luong Attention (Phase 3)

Designed to address the representational ceiling identified in Phase 2 group discussion: all baseline models hit a BLEU-4 ceiling below 0.24 with low type-token ratios pointing to repetitive captions.

**Encoder:** GoogLeNet (Inception v1) pretrained on ImageNet. Instead of a single global vector, the encoder exposes the full 7x7 spatial grid from the last inception block (49 patches, 1,024 channels each), projected to 512-d. This gives the decoder a sequence of region descriptors rather than a single summarised representation. Freeze ratio: blocks 0-12 frozen, blocks 13-15 trainable (~70/30).

**Decoder:** Single-layer LSTM with Luong General attention (input-feeding variant, Luong et al., 2015). At each decoding step, the hidden state scores all 49 spatial patches via a learned weight matrix, and the weighted sum becomes the context vector. LSTM input: 256 (embedding) + 512 (context) = 768-d, hidden size: 512.

**Training:** Same setup as Model 1. Best checkpoint saved at epoch 6 (earliest best validation loss).

---

## 📊 Results

| Model | BLEU-1 | BLEU-2 | BLEU-3 | BLEU-4 | ROUGE-L |
|---|---|---|---|---|---|
| Model 1 — MobileNetV3 + LSTM | **0.614** | **0.427** | **0.296** | **0.208** | **0.542** |
| Model 2 — GoogLeNet + LSTM + Attention | 0.568 | 0.401 | 0.282 | 0.202 | 0.511 |

Both models exceed the expected VizWiz baseline ranges (BLEU-1: 0.45-0.60, ROUGE-L: 0.30-0.50).

**Why Model 1 outperformed Model 2:** Model 2 had 3.2x more trainable encoder parameters (2,851,808 vs 884,712). With a fixed 10-epoch budget and 6,181 training images, the larger parameter space did not receive enough gradient updates to converge before overfitting (best checkpoint at epoch 6 vs epoch 10 for Model 1). The performance gap narrows consistently at higher n-gram orders (BLEU-4 gap: 0.006), suggesting the attention mechanism was functioning correctly but needed more training time to realise its benefit.

---

## 📦 Dataset Setup

All data lives in a shared Google Drive folder named `AT3-DL-ImageCaptioning`.

```
AT3-DL-ImageCaptioning/
├── raw/                      # Original VizWiz files
└── processed/
    ├── images_224/           # 7,750 images resized to 224x224
    ├── captions_clean.json   # Tokenised captions keyed by image_id
    ├── vocab.pkl             # Vocabulary (word2idx, idx2word, special tokens)
    └── splits.json           # Train/val/test splits (80/10/10)
```

**First time only:** run `notebooks/shared/data_preparation.ipynb` on Colab (~5-10 min). This downloads VizWiz, processes the data, and uploads to Drive. Done once per group.

---

## 🚀 Running on Google Colab

1. Clone the repo and open your student notebook in VS Code
2. Install the Google Colab extension (publisher: Google)
3. Click the kernel picker and select **Connect to Google Colab**
4. Mount Drive in the first cell: `drive.mount('/content/drive')`
5. Run all cells — GPU runs remotely on Colab

**Tips:**
- Runtime resets between sessions — re-run from the top each time
- If you hit the free GPU quota: `Runtime → Change runtime type → T4 GPU`
- Checkpoints are saved to Drive, not committed to git

---

## 🌿 Branch Strategy

Each student works on their own branch from Phase 1 onward.

```bash
# Create your branch from main
git checkout main && git pull origin main
git checkout -b student/<your-name>
git push -u origin student/<your-name>

# Keep your branch up to date
git fetch origin && git rebase origin/main
```

Open a Pull Request when a phase is complete. At least one teammate reviews before merging to main.

---

## 🛠️ Stack

`Python` · `PyTorch` · `torchvision` · `NumPy` · `NLTK` · `matplotlib` · `Google Colab`

---

## 👤 Author

**Nelkit Isael Chavez Calona** | Student ID: 25609227
University of Technology Sydney — MSc Data Science & Innovation
