# Fraud Detection ML Pipeline with DVC

## Overview

This project implements a reproducible Machine Learning workflow for fraud detection using **DVC (Data Version Control)**. The pipeline automates the complete data preparation and model training process while ensuring version control for datasets, parameters, and generated artifacts.

The workflow consists of three main stages:

1. **Data Processing** – Cleans and prepares raw transaction data.
2. **Data Splitting** – Creates training and testing datasets.
3. **Model Training** – Trains a machine learning model using configurable parameters.

DVC tracks all dependencies, outputs, and parameters so that only affected stages are re-executed when changes occur.

---

# Project Structure

```text
.
├── data
│   ├── raw
│   │   └── transactions.csv
│   └── processed
│       ├── clean_transactions.csv
│       ├── train.csv
│       └── test.csv
│
├── models
│   └── model.pkl
│
├── src
│   ├── data
│   │   ├── process_data.py
│   │   └── split_data.py
│   │
│   └── models
│       └── train.py
│
├── dvc.yaml
├── params.yaml
└── README.md
```

---

# Pipeline Stages

## 1. Data Processing

Processes the raw transaction dataset and generates a cleaned dataset suitable for model training.

### Command

```bash
python3 src/data/process_data.py
```

### Input

```text
data/raw/transactions.csv
```

### Output

```text
data/processed/clean_transactions.csv
```

---

## 2. Data Splitting

Splits the cleaned dataset into training and testing datasets.

### Command

```bash
python3 src/data/split_data.py
```

### Input

```text
data/processed/clean_transactions.csv
```

### Outputs

```text
data/processed/train.csv
data/processed/test.csv
```

---

## 3. Model Training

Trains a machine learning model using the training dataset and configurable hyperparameters.

### Command

```bash
python3 src/models/train.py
```

### Input

```text
data/processed/train.csv
```

### Parameters

Defined in:

```text
params.yaml
```

Example:

```yaml
n_estimators: 100
```

### Output

```text
models/model.pkl
```

---

# Prerequisites

## Python Dependencies

Install the project dependencies:

```bash
pip install -r requirements.txt
```

## Install DVC

If DVC is not already installed:

```bash
pip install dvc
```

Verify installation:

```bash
dvc version
```

---

# Configuration

Model parameters are stored in `params.yaml`.

Example:

```yaml
n_estimators: 100
```

Modify parameter values as needed before running the pipeline.

---

# Running the Pipeline

Execute the complete workflow:

```bash
dvc repro
```

DVC automatically executes the stages in the correct order:

1. Process Data
2. Split Data
3. Train Model

---

# Running Individual Stages

Run only the training stage (including any required upstream dependencies):

```bash
dvc repro train
```

---

# Force Rebuild

To rerun all stages regardless of previous outputs:

```bash
dvc repro --force
```

---

# Check Pipeline Status

Determine whether any stage requires re-execution:

```bash
dvc status
```

---

# Visualize the Pipeline

Display the dependency graph:

```bash
dvc dag
```

Example:

```text
process_data
      |
split_data
      |
   train
```

---

# Generated Artifacts

After a successful execution, the following outputs will be available:

| Output File                             | Description                    |
| --------------------------------------- | ------------------------------ |
| `data/processed/clean_transactions.csv` | Cleaned transaction dataset    |
| `data/processed/train.csv`              | Training dataset               |
| `data/processed/test.csv`               | Testing dataset                |
| `models/model.pkl`                      | Trained machine learning model |

---

# Reproducibility with DVC

DVC manages and tracks:

- Dataset dependencies
- Pipeline stages
- Model parameters
- Generated outputs
- Experiment reproducibility

This ensures:

- Consistent results across environments
- Efficient pipeline execution
- Traceable data and model versions
- Simplified collaboration among team members

---

# Example Workflow

Install dependencies:

```bash
pip install -r requirements.txt
pip install dvc
```

Run the pipeline:

```bash
dvc repro
```

Check pipeline status:

```bash
dvc status
```

Visualize pipeline structure:

```bash
dvc dag
```

Force a complete rebuild:

```bash
dvc repro --force
```

---

# Benefits of Using DVC

- Version control for datasets and models
- Reproducible machine learning workflows
- Efficient tracking of data lineage
- Seamless integration with Git
- Automated pipeline orchestration
- Simplified collaboration across teams

---

# Summary

This project demonstrates a complete DVC-managed machine learning workflow for fraud detection. By combining Git for code versioning and DVC for data and model management, the pipeline remains reproducible, scalable, and easy to maintain across development and production environments.
