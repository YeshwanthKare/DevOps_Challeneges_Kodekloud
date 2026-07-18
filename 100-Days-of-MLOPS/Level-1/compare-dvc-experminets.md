# DVC Experiments for Hyperparameter Tuning

## Overview

This exercise demonstrates how to use **DVC Experiments** to tune the `max_depth` hyperparameter of a fraud detection model and promote the best-performing experiment to the workspace.

The objective was to run multiple experiments, compare their performance using the **F1 Score**, and apply the best configuration as the tracked project state.

---

## Initial Configuration

`params.yaml`

```yaml
n_estimators: 100
max_depth: 4
```

The baseline pipeline had already been executed once before running the experiments.

---

## Running Experiments

Three experiments were executed with different `max_depth` values:

```bash
dvc exp run --set-param max_depth=2
dvc exp run --set-param max_depth=6
dvc exp run --set-param max_depth=12
```

Each run retrained the model and generated a new `metrics.json`.

---

## Comparing Results

Experiment results were compared using:

```bash
dvc exp show
```

### Results

| Experiment | Name       | Accuracy | F1 Score |
| ---------- | ---------- | -------- | -------- |
| 01d707e    | fuggy-vega | 0.75     | 0.6795   |
| 49818e3    | local-suer | 0.745    | 0.6483   |
| ef6fb28    | mated-yogi | 0.60     | 0.2453   |

---

## Best Experiment

The experiment with the highest F1 Score was:

```text
01d707e [fuggy-vega]
Accuracy: 0.75
F1 Score: 0.6795
```

---

## Promoting the Best Experiment

Apply the best experiment to the workspace:

```bash
dvc exp apply 01d707e
```

This updates:

- `params.yaml`
- `metrics.json`
- `models/model.pkl`

with the best-performing configuration.

---

## Verification

Check the applied parameters and metrics:

```bash
cat params.yaml
cat metrics.json
```

Display the current metrics:

```bash
dvc metrics show
```

Check pipeline status:

```bash
dvc status
```

---

## Outcome

- Executed three DVC experiments using different `max_depth` values.
- Compared experiment metrics using `dvc exp show`.
- Selected the experiment with the highest **F1 Score**.
- Applied the best experiment (`01d707e - fuggy-vega`) to the workspace.
- Promoted the best model, parameters, and metrics as the tracked project state.

This workflow enables reproducible experimentation and simplifies model selection in an MLOps environment.
