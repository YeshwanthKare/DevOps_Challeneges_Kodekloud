# Fraud Detection Project Dependency Locking with UV

## Objective

Update the dependency specification and generate a fully pinned lockfile using `uv`.

## Prerequisites

- Project directory: `/root/code/fraud-detection`
- `uv` installed and available in the system path
- Existing dependency specification file: `requirements.in`

## Navigate to the Project Directory

```bash
cd /root/code/fraud-detection
```

## Review the Existing Dependency Specification

```bash
cat requirements.in
```

## Update the Dependency Specification

Ensure that `requirements.in` contains exactly the following four top-level packages:

```text
scikit-learn
mlflow
pandas
numpy
```

Overwrite the file if necessary:

```bash
cat > requirements.in <<'EOF'
scikit-learn
mlflow
pandas
numpy
EOF
```

## Verify the Contents

```bash
cat requirements.in
```

Expected output:

```text
scikit-learn
mlflow
pandas
numpy
```

## Compile the Lockfile

Generate a pinned dependency lockfile:

```bash
uv pip compile requirements.in -o requirements.txt
```

## Verify the Generated Lockfile

Confirm that the top-level packages are pinned to exact versions:

```bash
grep -E "^(scikit-learn|mlflow|pandas|numpy)==" requirements.txt
```

Example output:

```text
mlflow==<version>
numpy==<version>
pandas==<version>
scikit-learn==<version>
```

## Review the Lockfile

```bash
head -50 requirements.txt
```

## Result

- `requirements.in` contains exactly four top-level packages:
  - `scikit-learn`
  - `mlflow`
  - `pandas`
  - `numpy`

- `requirements.txt` contains:
  - Exact pinned versions (`==`) for all four top-level packages.
  - All transitive dependencies resolved by `uv`.
  - A reproducible dependency set for the project.

## Why Does requirements.txt Contain More Packages?

The `requirements.in` file contains only the project's top-level dependencies. When `uv pip compile` is executed, it resolves all required transitive dependencies and pins every package to an exact version.

As a result, `requirements.txt` contains both the top-level packages and all additional packages required for them to function correctly, ensuring a reproducible and consistent Python environment.
