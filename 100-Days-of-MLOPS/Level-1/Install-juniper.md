# ================================

# JUPYTERLAB INSTALLATION GUIDE

# xFusionCorp Industries Setup

# ================================

# 1. Create required directories

mkdir -p /root/code
mkdir -p /root/notebooks

# 2. Create virtual environment

python3 -m venv /root/code/ml-env

# 3. Activate virtual environment

source /root/code/ml-env/bin/activate

# 4. Upgrade pip and install JupyterLab

pip install --upgrade pip
pip install jupyterlab

# (Optional but recommended)

pip install notebook

# 5. Generate Jupyter configuration

jupyter lab --generate-config

# 6. Move config to project directory

mv /root/.jupyter/jupyter_lab_config.py /root/code/jupyter_lab_config.py

# 7. Edit configuration file

# (use nano or vi)

nano /root/code/jupyter_lab_config.py

# REQUIRED CONFIG VALUES:

# c.ServerApp.ip = "0.0.0.0"

# c.ServerApp.port = 8888

# c.ServerApp.root_dir = "/root/notebooks"

# c.ServerApp.token = ""

# c.ServerApp.disable_check_xsrf = True

# 8. Start JupyterLab server

jupyter lab \
 --config=/root/code/jupyter_lab_config.py \
 --allow-root \
 --no-browser &

# 9. Verify service is running

ss -tlnp | grep 8888

# Expected output:

# 0.0.0.0:8888

# 10. Access JupyterLab

# http://<server-ip>:8888/lab

# ================================

# TROUBLESHOOTING

# ================================

# Check port usage

ss -tlnp | grep 8888

# Kill process using port

kill -9 <PID>

# Create missing notebook directory

mkdir -p /root/notebooks

# Recreate config if broken

rm -rf /root/.jupyter
jupyter lab --generate-config
