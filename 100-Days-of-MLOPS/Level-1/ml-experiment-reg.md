# MLflow Experiment Registration - xFusionCorp ML Platform

## Overview

The xFusionCorp Industries ML platform team required onboarding of two new Machine Learning projects into MLflow.

Each project must have its own MLflow experiment instead of using the default experiment. The experiments were created using the **MLflow UI** with the required metadata and ownership tags.

---

## Requirements

| Requirement                   | Details                    |
| ----------------------------- | -------------------------- |
| MLflow Tracking Server        | Running on port `5000`     |
| Registration Method           | MLflow UI                  |
| Existing Experiments          | `Default`, `legacy-models` |
| Existing Experiments Modified | No                         |
| New Experiments Required      | 2                          |

---

## 1. Access MLflow UI

The MLflow dashboard was accessed using the provided MLflow UI button.

MLflow server:

```text
http://<mlflow-server>:5000
```

The existing experiments were verified:

```text
Default
legacy-models
```

These existing experiments were left unchanged.

---

# Experiment 1: fraud-detection

## Creation

A new MLflow experiment was created:

```text
Experiment Name:
fraud-detection
```

## Experiment Description

Added a non-empty project description:

Example:

```text
Machine learning project for detecting fraudulent transactions.
```

## Experiment Tag

Added experiment-level tag:

| Key  | Value       |
| ---- | ----------- |
| team | ml-platform |

---

# Experiment 2: churn-prediction

## Creation

A new MLflow experiment was created:

```text
Experiment Name:
churn-prediction
```

## Experiment Tag

Added experiment-level tag:

| Key  | Value     |
| ---- | --------- |
| team | analytics |

---

## Verification

The MLflow UI experiments list now contains:

| Experiment       | Description               | Team Tag           |
| ---------------- | ------------------------- | ------------------ |
| Default          | Existing                  | Not modified       |
| legacy-models    | Existing                  | Not modified       |
| fraud-detection  | Project description added | `team=ml-platform` |
| churn-prediction | Created successfully      | `team=analytics`   |

---

## Validation Checklist

| Requirement                               | Status |
| ----------------------------------------- | ------ |
| Created `fraud-detection` experiment      | ✅     |
| Added description to `fraud-detection`    | ✅     |
| Added `team=ml-platform` tag              | ✅     |
| Created `churn-prediction` experiment     | ✅     |
| Added `team=analytics` tag                | ✅     |
| Did not modify `Default` experiment       | ✅     |
| Did not modify `legacy-models` experiment | ✅     |

---

## Conclusion

Both MLflow experiments were successfully registered through the MLflow UI with the required project ownership metadata. The ML platform now has separate experiment tracking spaces for:

- `fraud-detection`
- `churn-prediction`
