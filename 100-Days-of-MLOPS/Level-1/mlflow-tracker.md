# README - Configure MLflow Tracking Server

## Objective

Set up a local MLflow Tracking Server for experiment tracking with:

- Port `5000`
- Access on all network interfaces
- SQLite backend database
- Local artifact storage
- Support for any host header
- Support for any origin (CORS)
- Background execution that survives terminal closure

---

## Prerequisites

MLflow is pre-installed on the controlplane node.

---

## Step 1: Create Required Directories

MLflow requires the backend and artifact directories to exist before startup.

```bash
mkdir -p /root/code/mlflow-backend
mkdir -p /root/code/mlflow-artifacts
```

---

## Step 2: Start the MLflow Tracking Server

Launch MLflow in the background using `nohup`:

```bash
nohup mlflow server \
  --host 0.0.0.0 \
  --port 5000 \
  --backend-store-uri sqlite:////root/code/mlflow-backend/mlflow.db \
  --default-artifact-root /root/code/mlflow-artifacts \
  --allowed-hosts "*" \
  --cors-allowed-origins "*" \
  > /root/mlflow-server.log 2>&1 &
```

### Configuration Details

| Parameter       | Value                                           |
| --------------- | ----------------------------------------------- |
| Host            | `0.0.0.0`                                       |
| Port            | `5000`                                          |
| Backend Store   | `sqlite:////root/code/mlflow-backend/mlflow.db` |
| Artifact Root   | `/root/code/mlflow-artifacts`                   |
| Allowed Hosts   | `*`                                             |
| Allowed Origins | `*`                                             |

---

## Step 3: Verify the Server

Check that MLflow is listening on port `5000`:

```bash
ss -tulpn | grep 5000
```

Example output:

```text
tcp LISTEN 0 128 0.0.0.0:5000
```

Verify the MLflow process:

```bash
ps -ef | grep mlflow
```

---

## Step 4: Check Server Logs

View startup logs:

```bash
tail -f /root/mlflow-server.log
```

---

## Access the MLflow UI

Use the **MLflow UI** button provided in the lab environment.

Expected result:

- MLflow UI loads successfully.
- The **Default** experiment is visible.
- No experiment runs are present initially.

---

## Useful Commands

### Stop MLflow

```bash
pkill -f mlflow
```

### Restart MLflow

```bash
nohup mlflow server \
  --host 0.0.0.0 \
  --port 5000 \
  --backend-store-uri sqlite:////root/code/mlflow-backend/mlflow.db \
  --default-artifact-root /root/code/mlflow-artifacts \
  --allowed-hosts "*" \
  --cors-allowed-origins "*" \
  > /root/mlflow-server.log 2>&1 &
```

### Verify Database Creation

```bash
ls -lh /root/code/mlflow-backend/mlflow.db
```

### Verify Artifact Directory

```bash
ls -la /root/code/mlflow-artifacts
```

---

## Result

A persistent MLflow Tracking Server is running on port `5000`, using a SQLite backend database and local artifact storage, while allowing access through the lab proxy with unrestricted host headers and CORS origins.
