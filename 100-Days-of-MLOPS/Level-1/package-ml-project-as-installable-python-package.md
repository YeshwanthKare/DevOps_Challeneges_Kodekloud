# Build and Package the Fraud Detection Python Module

## Objective

Validate the `fraud-detection` project by:

- Creating unit tests for the `predict()` function
- Fixing the Python packaging configuration
- Building a distributable wheel package

Project location:

```text
/root/code/fraud-detection
```

---

# Step 1: Navigate to the Project Directory

```bash
cd /root/code/fraud-detection
```

---

# Step 2: Create Unit Tests

Create the tests directory if it does not already exist:

```bash
mkdir -p tests
```

Create the test file:

```bash
vi tests/test_predict.py
```

Add the following content:

```python
from fraud_detection import predict


def test_fraudulent_transaction():
    assert predict([[150]]) == 1


def test_legitimate_transaction():
    assert predict([[100]]) == 0
```

These tests verify:

| Transaction Amount | Expected Result |
| ------------------ | --------------- |
| 150 (>100)         | 1 (Fraud)       |
| 100 (<=100)        | 0 (Legitimate)  |

---

# Step 3: Fix pyproject.toml

Open the file:

```bash
vi pyproject.toml
```

Replace the contents with:

```toml
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name = "fraud_detection"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = [
    "scikit-learn",
    "pandas",
    "numpy"
]

[tool.pytest.ini_options]
pythonpath = ["src"]

[tool.setuptools]
package-dir = {"" = "src"}

[tool.setuptools.packages.find]
where = ["src"]
```

---

# Step 4: Run Unit Tests

Execute:

```bash
pytest
```

Expected output:

```text
=================== test session starts ===================

tests/test_predict.py ..                               [100%]

=================== 2 passed ===================
```

---

# Step 5: Build the Package

Run:

```bash
python3 -m build
```

Expected output:

```text
Successfully built
```

Terraform-like build artifacts will be generated under:

```text
dist/
```

---

# Step 6: Verify the Wheel File

List the generated artifacts:

```bash
ls -l dist/
```

Expected output:

```text
fraud_detection-0.1.0-py3-none-any.whl
fraud_detection-0.1.0.tar.gz
```

Verify specifically:

```bash
ls dist/fraud_detection-0.1.0-*.whl
```

Expected:

```text
dist/fraud_detection-0.1.0-py3-none-any.whl
```

---

# Verify Package Metadata

Check the package name:

```toml
name = "fraud_detection"
```

Version:

```toml
version = "0.1.0"
```

Python requirement:

```toml
requires-python = ">=3.10"
```

Dependencies:

```toml
dependencies = [
    "scikit-learn",
    "pandas",
    "numpy"
]
```

---

# Final Validation Commands

Run:

```bash
pytest
```

Expected:

```text
2 passed
```

Build:

```bash
python3 -m build
```

Verify wheel:

```bash
ls dist/fraud_detection-0.1.0-*.whl
```

Expected:

```text
dist/fraud_detection-0.1.0-py3-none-any.whl
```

---

# Summary

This task accomplishes the following:

- Creates unit tests for the `predict()` function
- Validates fraud detection behavior
- Configures setuptools packaging correctly
- Enables pytest imports from `src/`
- Builds a compliant Python wheel package
- Produces:

```text
dist/fraud_detection-0.1.0-*.whl
```

The project is now tested, packaged, and ready for distribution.
