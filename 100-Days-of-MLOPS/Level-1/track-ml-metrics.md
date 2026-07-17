# Fraud Detection Pipeline

## Overview

This project implements an end-to-end **Machine Learning pipeline** for fraud detection using **DVC (Data Version Control)**. The pipeline automates data processing, dataset splitting, model training, and metrics tracking while ensuring reproducibility and efficient version control.

---

## Project Structure

```text
.
├── data
│   ├── raw
│   │   └── transactions.csv
│   └── processed
│       ├── clean_transactions.csv
│       ├── train.csv
│       └── test.csv
├── models
│   └── model.pkl
├── metrics.json
├── src
│   ├── data
│   │   ├── process_data.py
│   │   └── split_data.py
│   └── models
│       └── train.py
├── dvc.yaml
├── dvc.lock
├── params.yaml
└── README.md
```

---

## Pipeline Workflow

```text
data/raw/transactions.csv
            │
            ▼
      process_data
            │
            ▼
data/processed/clean_transactions.csv
            │
            ▼
        split_data
            │
      ┌─────┴─────┐
      ▼           ▼
 train.csv    test.csv
      │
      ▼
         train
      │       │
      ▼       ▼
 model.pkl  metrics.json
```

---

## DVC Pipeline Configuration

The workflow is defined in `dvc.yaml`.

### Data Processing

Processes raw transaction data and generates a cleaned dataset.

```yaml
process_data:
  cmd: python3 src/data/process_data.py
  deps:
    - data/raw/transactions.csv
    - src/data/process_data.py
  outs:
    - data/processed/clean_transactions.csv
```

### Data Splitting

Splits the cleaned dataset into training and testing datasets.

```yaml
split_data:
  cmd: python3 src/data/split_data.py
  deps:
    - data/processed/clean_transactions.csv
    - src/data/split_data.py
  outs:
    - data/processed/train.csv
    - data/processed/test.csv
```

### Model Training

Trains the model, saves the trained artifact, and records evaluation metrics.

```yaml
train:
  cmd: python3 src/models/train.py
  deps:
    - data/processed/train.csv
    - src/models/train.py
  outs:
    - models/model.pkl
  metrics:
    - metrics.json:
        cache: false
```

---

## Prerequisites

Install project dependencies:

```bash
pip install -r requirements.txt
```

Install DVC:

```bash
pip install dvc
```

Initialize DVC (if not already initialized):

```bash
dvc init
```

---

## Running the Pipeline

Execute the complete pipeline:

```bash
dvc repro
```

Force all stages to rerun:

```bash
dvc repro -f
```

---

## Monitoring Pipeline Status

Check whether any stage needs to be reproduced:

```bash
dvc status
```

Visualize the pipeline graph:

```bash
dvc dag
```

---

## Metrics Tracking

The `metrics.json` file is configured with:

```yaml
metrics:
  - metrics.json:
      cache: false
```

This means:

- Metrics are tracked by Git.
- Metrics are not stored in the DVC cache.
- Performance can be compared across commits and branches.

View current metrics:

```bash
dvc metrics show
```

Compare metrics with another branch or commit:

```bash
dvc metrics diff main
```

---

## Version Control Strategy

### Git Tracked Files

```text
src/
dvc.yaml
dvc.lock
params.yaml
metrics.json
README.md
```

Commit changes:

```bash
git add src/ dvc.yaml dvc.lock params.yaml metrics.json README.md
git commit -m "Update pipeline and metrics"
```

### DVC Tracked Files

```text
data/raw/transactions.csv
data/processed/
models/model.pkl
```

Push tracked data to the configured remote:

```bash
dvc push
```

Pull data from the remote:

```bash
dvc pull
```

---

## Outputs

After a successful pipeline execution, the following artifacts are generated:

| File                                    | Description                    |
| --------------------------------------- | ------------------------------ |
| `data/processed/clean_transactions.csv` | Cleaned dataset                |
| `data/processed/train.csv`              | Training dataset               |
| `data/processed/test.csv`               | Testing dataset                |
| `models/model.pkl`                      | Trained machine learning model |
| `metrics.json`                          | Model evaluation metrics       |

---

## Benefits

- Reproducible ML workflows
- Automated pipeline execution
- Efficient data and model versioning
- Lightweight Git repository
- Experiment tracking and metric comparison
- Seamless collaboration using DVC remotes

---

## Common Commands

```bash
# Run pipeline
dvc repro

# Force rerun
dvc repro -f

# Check status
dvc status

# View DAG
dvc dag

# Show metrics
dvc metrics show

# Compare metrics
dvc metrics diff main

# Upload data/model artifacts
dvc push

# Download data/model artifacts
dvc pull
```
