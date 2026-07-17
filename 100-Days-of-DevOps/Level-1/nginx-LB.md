# Nginx Load Balancer Configuration for Nautilus Application

## Overview

This document describes the steps performed to configure the Load Balancer (`stlb01`) for the Nautilus application using Nginx. The objective is to distribute incoming HTTP traffic across multiple Apache application servers while preserving the existing Apache configuration.

## Environment

| Host    | Role                  |
| ------- | --------------------- |
| stlb01  | Load Balancer (Nginx) |
| stapp01 | Application Server    |
| stapp02 | Application Server    |
| stapp03 | Application Server    |

## Prerequisites

- SSH access to all servers.
- Sudo privileges on the load balancer and application servers.
- Apache (`httpd`) installed on all application servers.
- Nginx installed on the load balancer.

## Step 1: Verify Apache Service on Application Servers

Check that Apache is running on each application server:

```bash
sudo systemctl status httpd
```

Start the service if required:

```bash
sudo systemctl start httpd
sudo systemctl enable httpd
```

## Step 2: Verify Apache Listening Port

Determine the Apache listening port on each application server:

```bash
sudo grep -R "^Listen" /etc/httpd/conf*
```

Example output:

```text
/etc/httpd/conf/httpd.conf:Listen 5003
```

**Note:** Do not modify the Apache port configuration. The load balancer must use the existing Apache port.

## Step 3: Install Nginx on Load Balancer

If Nginx is not already installed:

```bash
sudo yum install -y nginx
```

Start and enable the service:

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```

## Step 4: Configure Nginx Load Balancing

Edit the main Nginx configuration file:

```bash
sudo vi /etc/nginx/nginx.conf
```

Configure the `http` section with all application servers:

```nginx
events {
}

http {

    upstream app_servers {
        server stapp01:5003;
        server stapp02:5003;
        server stapp03:5003;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://app_servers;

            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

If hostname resolution is unavailable, use the application server IP addresses instead.

## Step 5: Validate Configuration

Verify the Nginx configuration syntax:

```bash
sudo nginx -t
```

Expected output:

```text
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

## Step 6: Restart Nginx

Apply the configuration:

```bash
sudo systemctl restart nginx
```

Verify service status:

```bash
sudo systemctl status nginx
```

## Step 7: Test Backend Connectivity

From the load balancer, confirm that each application server is reachable:

```bash
curl http://stapp01:5003
curl http://stapp02:5003
curl http://stapp03:5003
```

Each command should return the application or Apache response page.

## Step 8: Test Load Balancer

Verify access through the load balancer:

```bash
curl http://stlb01:80
```

Expected result:

- The application page is returned successfully.
- Requests are distributed across the configured application servers.

## Troubleshooting

### Nginx 50x Error Page

Check the Nginx error log:

```bash
sudo tail -100 /var/log/nginx/error.log
```

Common error:

```text
connect() failed (111: Connection refused) while connecting to upstream
```

Possible causes:

- Apache service is not running on one or more application servers.
- Incorrect upstream port configured in Nginx.
- Network connectivity issues between the load balancer and application servers.

### Verify Listening Ports

```bash
sudo ss -lntp | grep httpd
```

### Verify Nginx Configuration

```bash
sudo nginx -T
```

## Result

Nginx is configured as a load balancer on `stlb01`, distributing HTTP requests across `stapp01`, `stapp02`, and `stapp03` while preserving the existing Apache configuration and listening port.
