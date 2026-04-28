# dl-image-captioning-at3

Deep Learning Assignment 3 — Image Captioning with PyTorch  
Dataset: [VizWiz-Captions](https://vizwiz.org/tasks-and-datasets/image-captioning/) validation set (7,750 images)

---

## Project Structure

```
dl-image-captioning-at3/
│
├── notebooks/
│   ├── shared/           # Phase 1 – shared data preparation notebook (run once)
│   └── students/         # Phase 2 & 3 – one notebook per student
│
├── checkpoints/          # Saved model weights (NOT tracked by git)
├── reports/              # Final group report (Word / PDF)
├── docs/                 # Any supplementary documentation
│
└── README.md
```

> All notebooks are self-contained and run on **Google Colab**. Dependencies are  
> installed inside each notebook with `!pip install`. There is no `src/` module,  
> no `requirements.txt`, and no `data/` folder in this repo — dataset files live  
> in the shared Google Drive folder (see Data Setup below).

---

## Branch Strategy

Each student works on their own branch from **Phase 1 onward**. There is no shared branch for individual work — everyone branches off `main` independently.

| Branch             | Purpose                                                   |
|--------------------|-----------------------------------------------------------|
| `main`             | Stable, reviewed code only. No direct pushes.             |
| `student/<name>`   | One branch per student — covers all phases (1, 2, 3)      |

### Examples

```text
student/nelkit        ← Nelkit's branch (Phase 1 → 2 → 3)
student/john          ← John's branch
student/maria-jose    ← Compound names: use a hyphen
```

### Workflow

```bash
# 1. Create your branch from main (once, at the start)
git checkout main
git pull origin main
git checkout -b student/<your-name>

# 2. Push it to the remote
git push -u origin student/<your-name>

# 3. Work on your notebook, commit regularly
git add notebooks/students/<your-name>-<student_id>-notebook.ipynb
git commit -m "feat: implement EDA cell"
git push

# 4. Open a Pull Request when a phase is complete
# At least one teammate reviews before merging to main
```

> Tip: keep your branch up to date with `main` as teammates merge in.

```bash
git fetch origin
git rebase origin/main
```

---

## Setup

### 1. Clone the repo
```bash
git clone https://github.com/Nelkit/dl-image-captioning-at3.git
```

### 2. Data Setup — Google Drive (Colab workflow)

All data lives in a **shared Google Drive folder** named `AT3-DL-ImageCaptioning`.

```text
AT3-DL-ImageCaptioning/          ← shared Drive folder (all team members have access)
├── raw/                          ← original VizWiz files (kept as backup)
└── processed/
    ├── images_224/               ← all 7,750 images resized to 224×224
    ├── captions_clean.json       ← tokenised captions keyed by image_id
    ├── vocab.pkl                 ← vocabulary (word2idx, idx2word, special tokens)
    └── splits.json               ← train/val/test image ID lists (80/10/10)
```

#### First time only — run the shared notebook (one person per group)

1. Share the `AT3-DL-ImageCaptioning` Drive folder with all team members
2. Open `notebooks/shared/data_preparation.ipynb` in Google Colab
3. Run all cells — the notebook downloads VizWiz, processes data, and uploads to Drive
4. This takes ~5-10 min due to image upload. Done once for the whole group.

#### Every student — run your individual notebook

1. Copy `notebooks/students/TEMPLATE_student_notebook.ipynb` and rename it following the convention:

   ```text
   [firstname]-[student_id]-notebook.ipynb
   ```

   Examples: `nelkit-12345678-notebook.ipynb`, `john-87654321-notebook.ipynb`

   Rules: lowercase only, no spaces, no accents, hyphen as separator.
   For compound first names use a hyphen: `maria-jose-12345678-notebook.ipynb`

2. Set `STUDENT_NAME` in cell 0 to your name
3. Mount Drive (cell 0) — data is immediately available, no download needed
4. Implement and train your models

---

## Evaluation Metrics

All student models are evaluated at minimum with:
- **BLEU-1, BLEU-2, BLEU-3, BLEU-4**

Additional metrics may include METEOR, CIDEr, or qualitative visual inspection.

---

## Team

| Student | Branch |
|---|---|
| TBD | `student/<name>-model` |
| TBD | `student/<name>-model` |
| TBD | `student/<name>-model` |

---

## Deadline

**20 May 2026, 23:59** — late penalty: 10 pts per day.
