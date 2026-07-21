I created a clean **README.md** based on the uploaded notes, structured as an operational documentation guide for installing and configuring **Nginx + PHP-FPM 8.1 with Unix Socket integration**.

```markdown
# Nginx + PHP-FPM 8.1 Configuration with Unix Socket

## Overview

This document explains how to configure **Nginx** as a web server and **PHP-FPM 8.1** as the PHP processing engine using a **Unix socket** for communication.

The setup requirements:

- Install Nginx on App Server 3
- Configure Nginx to listen on port `8099`
- Configure document root as `/var/www/html`
- Install PHP-FPM version `8.1`
- Configure PHP-FPM to use Unix socket:
```

/var/run/php-fpm/default.sock

````
- Configure Nginx and PHP-FPM communication
- Validate using:
```bash
curl http://stapp03:8099/index.php
````

---

# Architecture Overview

```
Client
  |
  | HTTP Request
  |
  v
Nginx
  |
  | FastCGI Request
  |
  | Unix Socket
  | /var/run/php-fpm/default.sock
  |
  v
PHP-FPM
  |
  | Executes PHP Script
  |
  v
HTML Response
```

---

# Components

## Nginx

Nginx is a web server responsible for:

- Handling HTTP requests
- Serving static files
- Forwarding PHP requests to PHP-FPM

Nginx cannot execute PHP directly, therefore PHP requests are passed to PHP-FPM.

---

## PHP-FPM

PHP-FPM (FastCGI Process Manager):

- Executes PHP scripts
- Manages PHP worker processes
- Communicates with Nginx through FastCGI

---

## Unix Socket

A Unix socket is an inter-process communication mechanism used between applications running on the same server.

Benefits:

- Faster than TCP communication
- Local communication only
- Permission controlled using Linux ownership

Socket used in this setup:

```
/var/run/php-fpm/default.sock
```

---

# Installation Steps

## 1. Install Nginx

SSH into application server:

```bash
ssh steve@stapp03
```

Install nginx:

```bash
sudo yum install -y nginx
```

---

# 2. Configure Nginx

Edit nginx configuration:

```bash
sudo vi /etc/nginx/nginx.conf
```

Update the server block:

```nginx
server {

    listen 8099;
    listen [::]:8099;

    server_name stapp03;

    root /var/www/html;

    index index.php index.html;


    location / {

        try_files $uri $uri/ =404;

    }


    location ~ \.php$ {

        include fastcgi_params;

        fastcgi_pass unix:/var/run/php-fpm/default.sock;

        fastcgi_index index.php;

        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

    }


    error_page 404 /404.html;

    error_page 500 502 503 504 /50x.html;

}
```

---

# Nginx PHP Configuration Explanation

## PHP Location Block

```nginx
location ~ \.php$
```

Handles all PHP file requests.

---

```nginx
include fastcgi_params;
```

Provides standard FastCGI request parameters.

---

```nginx
fastcgi_pass unix:/var/run/php-fpm/default.sock;
```

Forwards PHP requests to PHP-FPM through Unix socket.

---

```nginx
fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
```

Provides the complete PHP script path to PHP-FPM.

Example:

```
/var/www/html/index.php
```

---

# Start Nginx Service

Enable nginx:

```bash
sudo systemctl enable nginx
```

Start nginx:

```bash
sudo systemctl start nginx
```

Check status:

```bash
sudo systemctl status nginx
```

Validate listening port:

```bash
sudo ss -tulpn | grep nginx
```

Expected:

```
LISTEN 0 511 0.0.0.0:8099
```

---

# 3. Install PHP-FPM 8.1

Install required repositories:

```bash
sudo dnf install epel-release -y
```

Install Remi repository:

```bash
sudo dnf install https://rpms.remirepo.net/enterprise/remi-release-9.rpm -y
```

Check available PHP versions:

```bash
sudo dnf module list php
```

Enable PHP 8.1:

```bash
sudo dnf module enable php:remi-8.1 -y
```

Install PHP packages:

```bash
sudo dnf install php-fpm php php-cli php-common \
php-mysqlnd php-gd php-xml php-mbstring \
php-pdo php-opcache -y
```

---

# 4. Configure PHP-FPM

Edit PHP-FPM configuration:

```bash
sudo vi /etc/php-fpm.d/www.conf
```

Update:

## Change User

From:

```
user = apache
```

To:

```
user = nginx
```

---

## Change Group

From:

```
group = apache
```

To:

```
group = nginx
```

---

## Configure Unix Socket

Change:

```
listen = /run/php-fpm/www.sock
```

To:

```
listen = /var/run/php-fpm/default.sock
```

---

## Configure Socket Permissions

Uncomment and update:

```
listen.owner = nginx

listen.group = nginx

listen.mode = 0660
```

---

# 5. Create Socket Directory

Create required directory:

```bash
sudo mkdir -p /var/run/php-fpm
```

Set ownership:

```bash
sudo chown -R nginx:nginx /var/run/php-fpm
```

---

# 6. Start PHP-FPM

Enable service:

```bash
sudo systemctl enable php-fpm
```

Restart:

```bash
sudo systemctl restart php-fpm
```

Check status:

```bash
sudo systemctl status php-fpm
```

---

# Validate PHP-FPM Socket

Check socket:

```bash
ls -l /var/run/php-fpm/default.sock
```

Expected:

```
srw-rw---- nginx nginx default.sock
```

---

# Validate PHP Version

```bash
php -v
```

Expected:

```
PHP 8.1.x
```

---

# Application Validation

The application files are already available:

```
/var/www/html/index.php
/var/www/html/info.php
```

Do not modify these files.

Test application:

```bash
curl http://stapp03:8099/index.php
```

Expected response:

```
Welcome to xFusionCorp Industries!
```

---

# Troubleshooting

## Issue: PHP Socket Does Not Exist

Example:

```
ls: cannot access '/var/run/php-fpm/default.sock':
No such file or directory
```

### Solution

Restart PHP-FPM:

```bash
sudo systemctl restart php-fpm
```

Verify:

```bash
ls -l /var/run/php-fpm/default.sock
```

---

## Issue: Nginx Returns 404

Example:

```
404 Not Found
nginx/1.20.1
```

Check:

1. Document root:

```bash
ls -l /var/www/html
```

2. PHP-FPM socket:

```bash
ls -l /var/run/php-fpm/default.sock
```

3. PHP-FPM status:

```bash
systemctl status php-fpm
```

---

# Final Verification Checklist

| Component       | Validation                           | Expected     |
| --------------- | ------------------------------------ | ------------ |
| Nginx Installed | `nginx -v`                           | Installed    |
| Nginx Running   | `systemctl status nginx`             | Active       |
| Nginx Port      | `ss -tulpn`                          | 8099         |
| PHP Version     | `php -v`                             | PHP 8.1      |
| PHP-FPM Running | `systemctl status php-fpm`           | Active       |
| Unix Socket     | `ls /var/run/php-fpm/default.sock`   | Exists       |
| Application     | `curl http://stapp03:8099/index.php` | PHP Response |

---

# Summary

The final architecture uses:

- **Nginx** → Handles HTTP requests
- **PHP-FPM 8.1** → Executes PHP scripts
- **Unix Socket** → Provides fast local communication

Nginx receives PHP requests and forwards them to PHP-FPM through:

```
/var/run/php-fpm/default.sock
```

This provides an efficient and secure PHP web server setup.

```

This README is ready to be committed as `README.md` in a Git repository or used directly in an internal Wiki.
```
