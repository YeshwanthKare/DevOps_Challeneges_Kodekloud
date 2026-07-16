# Nginx SSL Deployment on App Server 2

## Objective

Prepare **App Server 2** for application deployment by installing and configuring **Nginx** with SSL support, deploying a self-signed certificate, creating a test webpage, and validating secure access from the Jump Host.

---

# Requirements

The following requirements were completed:

1. Install and configure Nginx on **App Server 2**.
2. Deploy the provided SSL certificate and key.
3. Create an `index.html` page displaying **Welcome!**
4. Verify HTTPS access from the Jump Host.

---

# Server Information

| Component       | Value                         |
| --------------- | ----------------------------- |
| Service         | Nginx                         |
| Protocol        | HTTPS                         |
| Port            | 443                           |
| Web Root        | `/usr/share/nginx/html`       |
| SSL Certificate | `/etc/nginx/ssl/nautilus.crt` |
| SSL Key         | `/etc/nginx/ssl/nautilus.key` |

---

# Installation

## Connect to App Server 2

```bash
ssh steve@stapp02
```

---

## Install Nginx

For RHEL/CentOS-based systems:

```bash
sudo yum install -y nginx
```

Verify installation:

```bash
nginx -v
```

Expected output:

```text
nginx version: nginx/1.x.x
```

---

# SSL Configuration

## Create SSL Directory

```bash
sudo mkdir -p /etc/nginx/ssl
```

## Move Certificate and Key

```bash
sudo cp /tmp/nautilus.crt /etc/nginx/ssl/
sudo cp /tmp/nautilus.key /etc/nginx/ssl/
```

Verify:

```bash
ls -l /etc/nginx/ssl
```

Expected:

```text
nautilus.crt
nautilus.key
```

---

# Configure Nginx

Edit the Nginx configuration:

```bash
sudo vi /etc/nginx/nginx.conf
```

Add the following server block inside the `http {}` section:

```nginx
server {
    listen 443 ssl;
    server_name _;

    ssl_certificate     /etc/nginx/ssl/nautilus.crt;
    ssl_certificate_key /etc/nginx/ssl/nautilus.key;

    root /usr/share/nginx/html;
    index index.html;
}
```

---

# Deploy Website Content

Create the homepage:

```bash
echo "Welcome!" | sudo tee /usr/share/nginx/html/index.html
```

Verify:

```bash
cat /usr/share/nginx/html/index.html
```

Output:

```text
Welcome!
```

---

# Validate Configuration

Check Nginx configuration syntax:

```bash
sudo nginx -t
```

Expected:

```text
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

---

# Start and Enable Nginx

```bash
sudo systemctl enable nginx
sudo systemctl restart nginx
```

Verify service status:

```bash
sudo systemctl status nginx
```

Expected:

```text
Active: active (running)
```

---

# Local Testing

Test HTTPS access directly from App Server 2:

```bash
curl -Ik https://localhost/
```

Expected response:

```text
HTTP/1.1 200 OK
Server: nginx
```

View page content:

```bash
curl -k https://localhost/
```

Output:

```text
Welcome!
```

---

# Remote Testing from Jump Host

From the Jump Host:

```bash
curl -Ik https://stapp02/
```

or

```bash
curl -Ik https://<APP_SERVER_2_IP>/
```

Expected response:

```text
HTTP/1.1 200 OK
Server: nginx
```

Verify page content:

```bash
curl -k https://stapp02/
```

Output:

```text
Welcome!
```

---

# Verification Checklist

| Task                         | Status |
| ---------------------------- | ------ |
| Nginx Installed              | ✅     |
| SSL Certificate Deployed     | ✅     |
| SSL Key Deployed             | ✅     |
| HTTPS Configured on Port 443 | ✅     |
| Welcome Page Created         | ✅     |
| Nginx Running                | ✅     |
| Accessible via HTTPS         | ✅     |
| Verified from Jump Host      | ✅     |

---

# Result

Nginx was successfully installed and configured on **App Server 2** with SSL enabled using the provided self-signed certificate. A test webpage displaying **Welcome!** was deployed and verified through HTTPS access both locally and from the Jump Host.
