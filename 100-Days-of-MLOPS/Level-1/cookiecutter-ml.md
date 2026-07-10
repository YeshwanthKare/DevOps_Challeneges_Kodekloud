# Fixing and Generating a Cookiecutter ML Project Template

## Objective

Correct the Cookiecutter template located at:

```bash
/root/code/mlops-template/
```

and generate a new project named:

```bash
churn-model
```

using the `sklearn` machine learning framework.

---

## Prerequisites

Verify Cookiecutter is installed:

```bash
cookiecutter --version
```

---

## Step 1: Update `cookiecutter.json`

Edit:

```bash
/root/code/mlops-template/cookiecutter.json
```

Add the required variables:

```json
{
  "project_name": "my-ml-project",
  "author": "xFusionCorp",
  "python_version": "3.11",
  "ml_framework": ["sklearn", "pytorch", "tensorflow"]
}
```

---

## Step 2: Fix the README Template

Create or edit:

```bash
/root/code/mlops-template/{{cookiecutter.project_name}}/README.md
```

Add the following content:

```md
# {{ cookiecutter.project_name }}

Author: {{ cookiecutter.author }}

Python Version: {{ cookiecutter.python_version }}
```

This ensures the generated README references both:

- `project_name`
- `author`

---

## Step 3: Fix the Requirements Template

Create or edit:

```bash
/root/code/mlops-template/{{cookiecutter.project_name}}/requirements.txt
```

Add:

```jinja2
{% if cookiecutter.ml_framework == "sklearn" %}
scikit-learn
{% elif cookiecutter.ml_framework == "pytorch" %}
torch
{% elif cookiecutter.ml_framework == "tensorflow" %}
tensorflow
{% endif %}
```

This ensures:

| ML Framework | Generated Dependency |
| ------------ | -------------------- |
| sklearn      | scikit-learn         |
| pytorch      | torch                |
| tensorflow   | tensorflow           |

---

## Step 4: Create Required Directories

Ensure the template contains the required folder structure:

```bash
mkdir -p "/root/code/mlops-template/{{cookiecutter.project_name}}"/{data,models,src,tests}
```

Expected template structure:

```text
/root/code/mlops-template/
├── cookiecutter.json
└── {{cookiecutter.project_name}}/
    ├── README.md
    ├── requirements.txt
    ├── data/
    ├── models/
    ├── src/
    └── tests/
```

---

## Step 5: Remove Any Previous Failed Generation

```bash
rm -rf /root/code/churn-model
```

---

## Step 6: Generate the Project

Run:

```bash
cookiecutter /root/code/mlops-template/ \
-o /root/code/ \
--no-input \
project_name=churn-model \
ml_framework=sklearn
```

This generates:

```bash
/root/code/churn-model
```

---

## Step 7: Verify the Generated Project

View the project structure:

```bash
tree /root/code/churn-model
```

Expected output:

```text
churn-model
├── README.md
├── requirements.txt
├── data
├── models
├── src
└── tests
```

---

## Step 8: Verify README Content

```bash
cat /root/code/churn-model/README.md
```

Expected output:

```text
# churn-model

Author: xFusionCorp

Python Version: 3.11
```

---

## Step 9: Verify Requirements File

```bash
cat /root/code/churn-model/requirements.txt
```

Expected output:

```text
scikit-learn
```

---

## Validation Checklist

- [x] `cookiecutter.json` contains `project_name`
- [x] `cookiecutter.json` contains `author`
- [x] `cookiecutter.json` contains `python_version`
- [x] `cookiecutter.json` contains `ml_framework`
- [x] README references both project name and author
- [x] `requirements.txt` renders correct dependency based on framework
- [x] Template contains `data/`, `models/`, `src/`, and `tests/`
- [x] Generated project exists at `/root/code/churn-model`
- [x] Generated `requirements.txt` contains `scikit-learn`
- [x] Generated `README.md` contains `xFusionCorp`
