# README - Install and Configure Tomcat Server on App Server 1

## Task Overview

The Nautilus application development team completed the beta version of a Java-based application and decided to deploy it using Apache Tomcat on App Server 1.

### Requirements

- Install Tomcat on App Server 1 (`stapp01`)
- Configure Tomcat to run on port **8087**
- Deploy the `ROOT.war` file located on the Jump Host at `/tmp/ROOT.war`
- Ensure the application is accessible directly from the base URL:

```bash
curl http://stapp01:8087
```

---

# Environment Details

| Component        | Value              |
| ---------------- | ------------------ |
| App Server       | stapp01            |
| User             | tony               |
| Tomcat Location  | /opt/tomcat/tomcat |
| Application Port | 8087               |
| WAR File         | /tmp/ROOT.war      |

---

# Step 1: Verify Operating System

Connect to App Server 1:

```bash
ssh tony@stapp01
```

Check OS information:

```bash
cat /etc/os-release
```

Since the server is based on a RHEL/CentOS distribution, use `yum` for package management.

---

# Step 2: Install Java

Tomcat requires Java to run.

Install OpenJDK:

```bash
sudo yum install java-17-openjdk -y
```

Verify:

```bash
java -version
```

Example output:

```text
openjdk version "17.x.x"
OpenJDK Runtime Environment
OpenJDK 64-Bit Server VM
```

---

# Step 3: Install Tomcat

Create installation directory:

```bash
sudo mkdir -p /opt/tomcat
```

Download Tomcat:

```bash
cd /opt/tomcat
sudo wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.80/bin/apache-tomcat-9.0.80.tar.gz
```

Extract:

```bash
sudo tar -xzf apache-tomcat-9.0.80.tar.gz
sudo mv apache-tomcat-9.0.80 tomcat
```

Verify:

```bash
ls -l /opt/tomcat
```

Expected:

```text
apache-tomcat-9.0.80.tar.gz
tomcat/
```

---

# Step 4: Configure Tomcat Port

Edit Tomcat configuration:

```bash
sudo vi /opt/tomcat/tomcat/conf/server.xml
```

Locate:

```xml
<Connector port="8080" protocol="HTTP/1.1"
```

Change:

```xml
<Connector port="8087" protocol="HTTP/1.1"
```

Save and exit.

Verify:

```bash
grep Connector /opt/tomcat/tomcat/conf/server.xml
```

---

# Step 5: Start Tomcat

Start Tomcat manually:

```bash
sudo /opt/tomcat/tomcat/bin/startup.sh
```

Expected:

```text
Tomcat started.
```

Verify process:

```bash
ps -ef | grep tomcat
```

Example:

```text
org.apache.catalina.startup.Bootstrap start
```

---

# Step 6: Deploy ROOT.war

## Initial Attempt

From Jump Host:

```bash
scp /tmp/ROOT.war tony@stapp01:/opt/tomcat/tomcat/webapps/
```

Error:

```text
scp: dest open "/opt/tomcat/tomcat/webapps/ROOT.war": Permission denied
```

Reason:

```bash
ls -ld /opt/tomcat/tomcat/webapps
```

Output:

```text
drwxr-x--- root root /opt/tomcat/tomcat/webapps
```

The `tony` user did not have write permission.

---

## Successful Deployment Method

Copy WAR file to `/tmp` first:

```bash
scp /tmp/ROOT.war tony@stapp01:/tmp/
```

Connect to App Server:

```bash
ssh tony@stapp01
```

Deploy using sudo:

```bash
sudo cp /tmp/ROOT.war /opt/tomcat/tomcat/webapps/
```

Verify:

```bash
sudo ls -l /opt/tomcat/tomcat/webapps/ROOT.war
```

---

# Step 7: Restart Tomcat

## Initial Attempt

```bash
sudo systemctl restart tomcat
```

Error:

```text
Failed to restart tomcat.service: Unit tomcat.service not found.
```

---

## Investigation

Check for Tomcat service:

```bash
sudo systemctl list-unit-files | grep -i tomcat
```

No output.

Check running process:

```bash
ps -ef | grep tomcat
```

Output confirmed Tomcat was running directly through Java:

```text
org.apache.catalina.startup.Bootstrap start
```

This indicated Tomcat was installed manually and was not managed by systemd.

---

## Correct Restart Procedure

Stop Tomcat:

```bash
sudo /opt/tomcat/tomcat/bin/shutdown.sh
```

Start Tomcat:

```bash
sudo /opt/tomcat/tomcat/bin/startup.sh
```

Output:

```text
Tomcat started.
```

---

# Step 8: Verify Deployment

Check application response:

```bash
curl http://localhost:8087
```

or

```bash
curl http://stapp01:8087
```

Expected output:

```html
<html>
  <head>
    <title>SampleWebApp</title>
  </head>
  <body>
    <h2>Welcome to xFusionCorp Industries!</h2>
  </body>
</html>
```

---

# Troubleshooting Performed

## Issue 1: Permission Denied During SCP

Command:

```bash
scp /tmp/ROOT.war tony@stapp01:/opt/tomcat/tomcat/webapps/
```

Error:

```text
Permission denied
```

Resolution:

```bash
scp /tmp/ROOT.war tony@stapp01:/tmp/
sudo cp /tmp/ROOT.war /opt/tomcat/tomcat/webapps/
```

---

## Issue 2: Unable to List webapps Directory

Command:

```bash
ls /opt/tomcat/tomcat/webapps
```

Error:

```text
Permission denied
```

Cause:

```text
webapps directory owned by root:root
```

Resolution:

Used sudo when accessing the directory.

---

## Issue 3: Tomcat Service Not Found

Command:

```bash
sudo systemctl restart tomcat
```

Error:

```text
Unit tomcat.service not found
```

Cause:

Tomcat was installed manually rather than through a systemd package.

Resolution:

Used Tomcat scripts:

```bash
sudo /opt/tomcat/tomcat/bin/shutdown.sh
sudo /opt/tomcat/tomcat/bin/startup.sh
```

---

# Final Validation

Verify Tomcat process:

```bash
ps -ef | grep tomcat
```

Verify deployed WAR:

```bash
sudo ls -l /opt/tomcat/tomcat/webapps/ROOT.war
```

Verify application:

```bash
curl http://stapp01:8087
```

Expected result:

```html
<h2>Welcome to xFusionCorp Industries!</h2>
```

The application is successfully deployed and accessible on:

```text
http://stapp01:8087
```
