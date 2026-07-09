# Setting Up Pre-Commit Hooks

This project uses **pre-commit** to automatically enforce code quality, formatting, and consistency checks before code is committed to Git.

## Prerequisites

Ensure Python and pip are installed:

```bash
python --version
pip --version
```

Install pre-commit:

```bash
pip install pre-commit
```

Verify the installation:

```bash
pre-commit --version
```

## Pre-Commit Configuration

Create a file named `.pre-commit-config.yaml` in the root of the repository:

```yaml
repos:
  - repo: https://github.com/pre-commit/pre-commit-hooks
    rev: v6.0.0
    hooks:
      - id: trailing-whitespace
      - id: end-of-file-fixer
      - id: check-yaml

  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.15.14
    hooks:
      - id: ruff

  - repo: https://github.com/psf/black-pre-commit-mirror
    rev: 26.5.1
    hooks:
      - id: black
```

## Install the Hooks

Install the hooks into your local Git repository:

```bash
pre-commit install
```

You should see:

```text
pre-commit installed at .git/hooks/pre-commit
```

## Run Hooks Manually

Run all configured hooks against every file in the repository:

```bash
pre-commit run --all-files
```

Example output:

```text
Trim Trailing Whitespace.................................................Passed
Fix End of Files.........................................................Passed
Check Yaml...............................................................Passed
ruff.....................................................................Passed
black....................................................................Passed
```

## What Each Hook Does

### trailing-whitespace

Removes unnecessary whitespace at the end of lines.

### end-of-file-fixer

Ensures files end with a single newline character.

### check-yaml

Validates YAML files and reports syntax errors.

### ruff

Performs fast Python linting and identifies code quality issues.

### black

Automatically formats Python code according to Black's style guidelines.

## Common Issues and Fixes

### Missing `rev`

Error:

```text
Missing required key: rev
```

Cause:

A repository entry in `.pre-commit-config.yaml` does not specify a revision.

Fix:

```yaml
- repo: https://github.com/psf/black-pre-commit-mirror
  rev: 26.5.1
  hooks:
    - id: black
```

### Invalid Black Revision

Error:

```text
error: pathspec 'v26.5.1' did not match any file(s) known to git
```

Cause:

The revision value does not exist in the Black pre-commit mirror repository.

Incorrect:

```yaml
rev: v26.5.1
```

Correct:

```yaml
rev: 26.5.1
```

### Trailing Whitespace Hook Failed

Error:

```text
Trim Trailing Whitespace.................................................Failed
files were modified by this hook
```

Cause:

The hook found and automatically removed trailing whitespace.

View the changes:

```bash
git diff
```

Run the hooks again:

```bash
pre-commit run --all-files
```

## Updating Hooks

To update all hook revisions to their latest compatible versions:

```bash
pre-commit autoupdate
```

After updating:

```bash
pre-commit clean
pre-commit run --all-files
```

## Typical Workflow

Run checks before committing:

```bash
pre-commit run --all-files
```

Stage changes:

```bash
git add .
```

Create a commit:

```bash
git commit -m "Configure pre-commit hooks"
```

Once installed, pre-commit hooks run automatically before every commit, helping maintain consistent code quality across the project.
