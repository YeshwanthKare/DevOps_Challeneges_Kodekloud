# Configure Ruff and Black for Python Code Quality

## Objective

Fix the Python project located at:

```text
/root/code/fraud-detection
```

so that the following commands complete successfully:

```bash
ruff check src/
```

```bash
black --check src/
```

### Requirements

- Ruff and Black must use a line length of **120**
- Ruff rule selection must be configured under:

```toml
[tool.ruff.lint]
```

- Ruff must include the following rule groups:

```toml
["E", "F", "W", "I"]
```

- All linting and formatting issues must be resolved

---

## Step 1: Navigate to the Project Directory

```bash
cd /root/code/fraud-detection
```

---

## Step 2: Review the Existing Configuration

Inspect the current configuration:

```bash
cat pyproject.toml
```

Look for:

- Incorrect Ruff schema
- Wrong line length values
- Missing lint rule selections

---

## Step 3: Update `pyproject.toml`

Edit the configuration file:

```bash
vi pyproject.toml
```

Configure it as follows:

```toml
[project]
name = "fraud-detection"
version = "0.1.0"

[tool.black]
line-length = 120

[tool.ruff]
line-length = 120

[tool.ruff.lint]
select = ["E", "F", "W", "I"]
```

---

## Step 4: Run Ruff

Execute Ruff against the source directory:

```bash
ruff check src/
```

Example output:

```text
I001 Import block is un-sorted or un-formatted
F401 `os` imported but unused
```

---

## Step 5: Automatically Fix Ruff Issues

Use Ruff's automatic fix feature:

```bash
ruff check src/ --fix
```

Run Ruff again:

```bash
ruff check src/
```

Expected output:

```text
All checks passed!
```

---

## Step 6: Format the Source Code with Black

Format all Python files under `src/`:

```bash
black src/
```

Example output:

```text
All done! ✨ 🍰 ✨
5 files left unchanged.
```

---

## Step 7: Validate Black Formatting

Verify that Black has no remaining changes to apply:

```bash
black --check src/
```

Expected output:

```text
All done! ✨ 🍰 ✨
5 files would be left unchanged.
```

---

## Step 8: Final Ruff Validation

Run Ruff one final time:

```bash
ruff check src/
```

Expected output:

```text
All checks passed!
```

---

## Understanding the Configuration

### Black Configuration

```toml
[tool.black]
line-length = 120
```

Sets the maximum allowed line length to 120 characters.

---

### Ruff Configuration

```toml
[tool.ruff]
line-length = 120
```

Ensures Ruff uses the same line length as Black.

---

### Ruff Lint Rules

```toml
[tool.ruff.lint]
select = ["E", "F", "W", "I"]
```

| Rule | Description          |
| ---- | -------------------- |
| E    | pycodestyle errors   |
| F    | Pyflakes checks      |
| W    | pycodestyle warnings |
| I    | Import sorting       |

---

## Why Use `[tool.ruff.lint]`

Modern Ruff versions (0.1+) require:

```toml
[tool.ruff.lint]
```

Older configurations such as:

```toml
[tool.ruff]
select = ["E", "F"]
```

are no longer valid.

---

## Verification Checklist

- Updated `pyproject.toml`
- Configured Black line length to 120
- Configured Ruff line length to 120
- Added Ruff lint rules (`E`, `F`, `W`, `I`)
- Fixed all Ruff linting issues
- Formatted code with Black
- Verified `ruff check src/` passes
- Verified `black --check src/` passes

---

## Final Validation Commands

```bash
ruff check src/
```

```bash
black --check src/
```

Expected result:

```text
All checks passed!
```

```text
All done! ✨ 🍰 ✨
```

The project is now compliant with the team's code quality standards and ready for pull request validation.
