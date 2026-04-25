# Branch Strategy

## Branches

| Branch | Owner | Purpose |
|---|---|---|
| `main` | All | Stable, reviewed code. **No direct pushes.** |
| `shared/data-prep` | Group | Phase 1 – shared data preparation notebook |
| `student/<n>-model` | Individual | Phase 2 & 3 – individual architectures |

## How to start working

```bash
# Clone
git clone https://github.com/<org>/dl-image-captioning-at3.git
cd dl-image-captioning-at3

# Create your personal branch from main
git checkout -b student/<your-name>-model

# Push your branch to remote
git push -u origin student/<your-name>-model
```

## Pull Request checklist

Before opening a PR to `main`:
- [ ] Notebook cells all executed with output
- [ ] No data files or checkpoints staged (check `.gitignore`)
- [ ] BLEU scores clearly printed in the notebook
- [ ] At least one teammate assigned as reviewer

## Commit message convention

```
[phase] short description

Examples:
[data] add caption tokenisation and vocab builder
[model1] add ResNet encoder + LSTM decoder
[model2] add attention mechanism based on group discussion
[eval] add BLEU-1 to BLEU-4 evaluation loop
```
