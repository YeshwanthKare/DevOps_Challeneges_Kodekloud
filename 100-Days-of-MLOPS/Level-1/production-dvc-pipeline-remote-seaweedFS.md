# README - Complete DVC Pipeline, Push to SeaweedFS, and Release v1.0

## Objective

Complete the fraud-detection DVC pipeline by:

- Fixing the misconfigured `preprocess` stage.
- Adding the `train` and `evaluate` stages.
- Reproducing the full pipeline.
- Pushing DVC artifacts to SeaweedFS.
- Committing all changes to Git.
- Tagging the release as `v1.0`.

---

## Project Location

```bash
cd /root/code/ml-pipeline
```

---

## Copy Training and Evaluation Scripts

```bash
cp scripts-staging/train.py scripts/
cp scripts-staging/evaluate.py scripts/
```

---

## Fix the Existing Pipeline

The `preprocess` stage was misconfigured and expected the wrong output file.

Correct output:

```yaml
preprocess:
  cmd: python3 scripts/preprocess.py
  deps:
    - data/raw/data.csv
    - scripts/preprocess.py
  outs:
    - data/processed/clean.csv
```

---

## Add Train Stage

```yaml
train:
  cmd: python3 scripts/train.py
  deps:
    - data/processed/clean.csv
    - scripts/train.py
  params:
    - n_estimators
    - max_depth
    - test_size
    - random_seed
  outs:
    - models/model.pkl
    - data/processed/test_split.csv
  metrics:
    - metrics.json:
        cache: false
```

---

## Add Evaluate Stage

```yaml
evaluate:
  cmd: python3 scripts/evaluate.py
  deps:
    - models/model.pkl
    - data/processed/test_split.csv
    - scripts/evaluate.py
  metrics:
    - reports/evaluation.json:
        cache: false
```

---

## Run the Pipeline

```bash
dvc repro
```

Expected stages:

1. ingest
2. validate
3. preprocess
4. train
5. evaluate

---

## Verify Outputs

```bash
ls -R data models reports
```

Expected files:

```text
data/processed/clean.csv
data/processed/test_split.csv
models/model.pkl
metrics.json
reports/evaluation.json
```

---

## Commit Changes

```bash
git add .
git commit -m "Complete DVC pipeline with train and evaluate stages"
```

---

## Push Artifacts to SeaweedFS

```bash
dvc push
```

Verify the remote bucket through the SeaweedFS Filer UI:

```text
/buckets/dvc-storage/files/md5/
```

---

## Create Release Tag

```bash
git tag v1.0
```

Verify:

```bash
git tag
```

Expected output:

```text
v1.0
```

---

## Final Verification

Check Git status:

```bash
git status
```

Check DVC status:

```bash
dvc status
```

Expected:

```text
nothing to commit, working tree clean
Data and pipelines are up to date.
```

---

## Useful Commands

```bash
# Run pipeline
dvc repro

# Show metrics
dvc metrics show

# Check status
dvc status

# Push data
dvc push

# Create release tag
git tag v1.0

# List tags
git tag
```

## Result

- Fixed the broken `preprocess` stage.
- Added `train` and `evaluate` stages.
- Generated model and evaluation artifacts.
- Pushed DVC cache to SeaweedFS.
- Committed all changes to Git.
- Tagged the release as `v1.0`.
- Created a fully reproducible ML pipeline.
