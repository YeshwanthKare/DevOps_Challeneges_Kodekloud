# Data Version Control (DVC) Pipeline

This project uses DVC (Data Version Control) to manage and reproduce the data processing workflow. The pipeline consists of two stages:

1. **process_data** – Cleans and preprocesses the raw transaction data.
2. **split_data** – Splits the processed data into training and testing datasets.

## Project Structure

```text
.
├── data/
│   ├── raw/
│   │   └── transactions.csv
│   └── processed/
│       ├── clean_transactions.csv
│       ├── train.csv
│       └── test.csv
├── src/
│   └── data/
│       ├── process_data.py
│       └── split_data.py
├── dvc.yaml
├── dvc.lock
└── README.md
```

## DVC Pipeline Definition

The pipeline is defined in `dvc.yaml`:

```yaml
stages:
  process_data:
    cmd: python3 src/data/process_data.py
    deps:
      - src/data/process_data.py
      - data/raw/transactions.csv
    outs:
      - data/processed/clean_transactions.csv

  split_data:
    cmd: python3 src/data/split_data.py
    deps:
      - src/data/split_data.py
      - data/processed/clean_transactions.csv
    outs:
      - data/processed/train.csv
      - data/processed/test.csv
```

## Prerequisites

Install DVC and project dependencies:

```bash
pip install dvc
```

Install any additional Python dependencies required by the project:

```bash
pip install -r requirements.txt
```

## Running the Pipeline

To execute the entire pipeline:

```bash
dvc repro
```

DVC will:

1. Run `process_data.py` to generate `clean_transactions.csv`.
2. Run `split_data.py` to generate `train.csv` and `test.csv`.
3. Record pipeline metadata in `dvc.lock`.

## Pipeline Reproducibility

DVC tracks:

- Source scripts (`process_data.py`, `split_data.py`)
- Input datasets (`transactions.csv`)
- Generated outputs (`clean_transactions.csv`, `train.csv`, `test.csv`)

If any dependency changes, running:

```bash
dvc repro
```

will automatically re-execute only the affected stages and any downstream stages.

## Visualizing the Pipeline

To view the pipeline structure:

```bash
dvc dag
```

Expected workflow:

```text
transactions.csv
        │
        ▼
 process_data
        │
        ▼
clean_transactions.csv
        │
        ▼
   split_data
      ├── train.csv
      └── test.csv
```

## Tracking Changes

After updating data or code:

```bash
dvc repro
git add dvc.yaml dvc.lock
git commit -m "Update data pipeline"
```

This ensures both the pipeline definition and its tracked outputs remain reproducible across environments.
