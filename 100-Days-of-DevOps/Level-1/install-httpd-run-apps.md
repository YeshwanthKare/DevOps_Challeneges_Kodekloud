## Here is a complete, production-ready README.md for this setup. It documents the exact steps you followed, the architecture, and how to verify or troubleshoot the deployment. [1, 2, 3, 4]

## Static Website Hosting on Apache (Port 8082)

This repository contains the configuration and deployment steps for hosting two static applications (media and apps) on Apache (httpd) under a single port using path-based routing.

## Architecture Overview

- Server: App Server 2 (stapp02)
- Web Server: Apache HTTP Server (httpd)
- Port: 8082
- Application Paths:
- http://localhost:8082/media/ → /home/thor/media/
  - http://localhost:8082/apps/ → /home/thor/apps/

---

## Deployment Steps## 1. File Transfer (from Jump Host)

Because direct upload to /home/thor/ via non-root users fails path canonicalization, files are staged through the user's home directory first.

# Run on jump_host to copy backups to the staging area

scp -r /home/thor/media steve@stapp02:/home/steve/
scp -r /home/thor/apps steve@stapp02:/home/steve/

## 2. File Placement & Permissions

Log into App Server 2, move files to their destination, and grant Apache (apache user) directory traversal privileges:

# Log into App Server 2 and switch to root

ssh steve@stapp02
sudo su

# Create target home directory if missing

mkdir -p /home/thor

# Move folders out of staging

mv /home/steve/media /home/thor/
mv /home/steve/apps /home/thor/

# Fix permissions for Apache traversal and reading

chmod +x /home/thor
chmod -R +r /home/thor/media
chmod -R +r /home/thor/apps

## 3. Package Installation

Install Apache and its standard dependencies:

yum install -y httpd

## 4. Apache Configuration

Append the custom port and virtual host blocks to the bottom of the main configuration file /etc/httpd/conf/httpd.conf:

# Custom Port Configuration for xFusionCorp Task

Listen 8082

<VirtualHost \*:8082>
DocumentRoot /var/www/html

    # Setup for /media/
    Alias /media/ "/home/thor/media/"
    <Directory "/home/thor/media/">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    # Setup for /apps/
    Alias /apps/ "/home/thor/apps/"
    <Directory "/home/thor/apps/">
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

</VirtualHost>

## 5. Service Activation

Validate the configuration formatting, start the service, and enable it to persist across system reboots:

# Verify configuration syntax (Should output: Syntax OK)

httpd -t

# Start and enable the service

systemctl start httpd
systemctl enable httpd

---

## Verification

Test endpoint functionality directly from the application server command line using curl:

# Test media site

curl http://localhost:8082/media/

# Test apps site

curl http://localhost:8082/apps/

---

## Troubleshooting

- 403 Forbidden Error: Apache cannot access the path. Ensure /home/thor has execution rights (chmod +x /home/thor) and the nested directories are globally readable (chmod -R +r).
- Address already in use Error: Another service is already utilizing port 8082, or Listen 8082 was defined twice in httpd.conf. Check active ports using ss -tulnp | grep 8082.
- 404 Not Found Error: The trailing slashes in your URL must match the trailing slashes defined in the Apache Alias directives. Ensure you request http://localhost:8082/media/ instead of http://localhost:8082/media.

---

Would you like me to add a shell script automation section to this README so you can execute the entire server configuration block in a single copy-paste command?

[1] [https://github.com](https://github.com/NotHarshhaa/DevOps-Projects/blob/master/README.md)
[2] [https://wengerk.medium.com](https://wengerk.medium.com/why-having-a-readme-on-your-internal-project-is-essential-c85cb9dd8e65)
[3] [https://mcpmarket.com](https://mcpmarket.com/tools/skills/automated-readme-generator-1)
[4] [https://github.com](https://github.com/johnkingzy/DocumentManagementSys)
