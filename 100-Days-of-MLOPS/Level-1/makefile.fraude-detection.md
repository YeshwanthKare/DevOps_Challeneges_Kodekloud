# Fraud Detection

A Python-based machine learning project for detecting fraudulent transactions. The project includes data preprocessing, model training, automated testing, and workflow automation using Make.

## Features

- Data preprocessing pipeline
- Machine learning model training
- Automated testing with Pytest
- Reproducible development environment
- Simple workflow management with Makefile

## Project Structure

```text
fraud-detection/
├── src/
│   ├── data/
│   │   └── process_data.py
│   └── models/
│       └── train.py
├── tests/
├── requirements.txt
├── Makefile
└── README.md
```

## Requirements

- Python 3.12+
- pip
- Git

Verify your Python installation:

```bash
python3 --version
```

## Installation

Clone the repository:

```bash
git clone <repository-url>
cd fraud-detection
```

Set up the virtual environment and install dependencies:

```bash
make setup
```

This command creates a virtual environment named `mlops-venv` and installs all required packages from `requirements.txt`.

## Usage

### 1. Process Data

Run the data preprocessing pipeline:

```bash
make data
```

This executes:

```bash
python3 src/data/process_data.py
```

### 2. Train the Model

Train the fraud detection model:

```bash
make train
```

This executes:

```bash
python3 src/models/train.py
```

### 3. Run Tests

Execute the test suite:

```bash
make test
```

This executes:

```bash
pytest tests/
```

### 4. Run the Complete Pipeline

Execute the full workflow:

```bash
make all
```

The `all` target runs the following steps in order:

1. Setup environment and install dependencies
2. Process data
3. Train the model
4. Run tests

## Makefile Targets

| Target       | Description                                         |
| ------------ | --------------------------------------------------- |
| `make setup` | Create virtual environment and install dependencies |
| `make data`  | Run data preprocessing                              |
| `make train` | Train the machine learning model                    |
| `make test`  | Execute all tests                                   |
| `make clean` | Remove cache files                                  |
| `make all`   | Run setup, data processing, training, and testing   |

## Makefile Configuration

```make
.PHONY: setup data train test clean all

setup:
	python3 -m venv mlops-venv && mlops-venv/bin/pip install -r requirements.txt

data:
	python3 src/data/process_data.py

train:
	python3 src/models/train.py

test:
	pytest tests/

clean:
	find . -type d -name "__pycache__" -exec rm -rf {} +
	find . -type d -name ".pytest_cache" -exec rm -rf {} +
	rm -rf models/*

all: setup data train test
```

## Development Workflow

1. Clone the repository.
2. Run `make setup`.
3. Run `make data`.
4. Run `make train`.
5. Run `make test`.
6. Commit and push your changes.

## Testing

Run all tests:

```bash
pytest tests/
```

For detailed output:

```bash
pytest -v tests/
```

## Contributing

Contributions are welcome.

1. Fork the repository.
2. Create a feature branch.
3. Make your changes.
4. Run tests and verify functionality.
5. Submit a pull request.

## License

Add your preferred open-source license before publishing.

## Author

Fraud Detection Machine Learning Project.
