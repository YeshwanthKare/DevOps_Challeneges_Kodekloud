# README - Logging an MLflow Experiment

## Overview

This lab demonstrates how to record a machine learning experiment in MLflow by logging:

- Hyperparameters
- Evaluation metrics
- Trained model artifacts

The experiment is tracked in the MLflow server running on:

```text
http://localhost:5000
```

---

## Project File

```text
/root/code/log_experiment.py
```

The script:

1. Creates a synthetic dataset.
2. Trains a `DummyClassifier`.
3. Computes evaluation metrics.
4. Logs parameters, metrics, and model artifacts to MLflow.

---

## MLflow Tracking Configuration

The script connects to the local MLflow Tracking Server:

```python
mlflow.set_tracking_uri("http://localhost:5000")
```

---

## Logged Parameters

The following hyperparameters are logged:

```python
params = {
    "n_estimators": 100,
    "max_depth": 5,
    "random_state": 42
}
```

Logging command:

```python
mlflow.log_params(params)
```

---

## Logged Metrics

The script computes:

```python
accuracy = accuracy_score(y_fit, preds)
f1 = f1_score(y_fit, preds)
```

Logging commands:

```python
mlflow.log_metric("accuracy", accuracy)
mlflow.log_metric("f1_score", f1)
```

---

## Logged Model Artifact

The trained sklearn model is stored as an MLflow artifact:

```python
mlflow.sklearn.log_model(
    model,
    artifact_path="model"
)
```

---

## Run the Experiment

Navigate to the project directory:

```bash
cd /root/code
```

Execute the script:

```bash
python3 log_experiment.py
```

Example output:

```text
accuracy=0.75, f1_score=0.8571428571428571
```

---

## Verify in MLflow UI

Open the **MLflow UI** from the lab interface.

Navigate to:

```text
Default Experiment
```

Open the newly created run and verify:

### Parameters

```text
n_estimators = 100
max_depth = 5
random_state = 42
```

### Metrics

```text
accuracy
f1_score
```

### Artifacts

```text
model/
```

containing the logged sklearn model.

---

## Validation Checklist

- New run appears in the Default experiment.
- Parameters are logged successfully.
- Metrics are visible in the run.
- Model artifact is stored under Artifacts.
- MLflow tracking server records the complete experiment.

---

## Key MLflow Commands Used

```python
mlflow.log_params(params)
mlflow.log_metric("accuracy", accuracy)
mlflow.log_metric("f1_score", f1)
mlflow.sklearn.log_model(model, artifact_path="model")
```

These commands provide complete experiment tracking for reproducibility and model comparison.
