# Apache Service Recovery and Port 3000 Configuration

## Overview

A monitoring alert indicated that the Apache HTTP service on **App Server 1 (`stapp01`)** was unreachable. The objective was to restore service availability and ensure the application was accessible on **port 3000** from the Jump Host.

---

## Environment

| Component          | Value                      |
| ------------------ | -------------------------- |
| Server             | stapp01                    |
| Service            | Apache HTTPD               |
| Required Port      | 3000                       |
| Validation Command | `curl http://stapp01:3000` |

---

## Initial Problem

Apache failed to start and reported:

```text
(98) Address already in use
AH00072: make_sock: could not bind to address
no listening sockets available, shutting down
```

Service status:

```bash
sudo systemctl status httpd
```

---

## Investigation

### Verify Apache Listening Port

Checked Apache configuration:

```bash
sudo grep -R "^Listen" /etc/httpd/conf /etc/httpd/conf.d
```

Output:

```text
/etc/httpd/conf/httpd.conf:Listen 3000
```

Apache was configured correctly to listen on port **3000**.

---

### Identify Port Conflict

Checked which process was using port 3000:

```bash
sudo ss -tulpn | grep :3000
```

Output:

```text
tcp LISTEN 0 10 127.0.0.1:3000 users:(("sendmail",pid=9753,fd=4))
```

The Sendmail service was already bound to port 3000, preventing Apache from starting.

---

## Resolution

### Stop the Conflicting Service

Stopped Sendmail:

```bash
sudo systemctl stop sendmail
```

Verified port 3000 was free:

```bash
sudo ss -tulpn | grep :3000
```

---

### Start and Enable Apache

Started Apache:

```bash
sudo systemctl start httpd
```

Enabled it at boot:

```bash
sudo systemctl enable httpd
```

Verified status:

```bash
sudo systemctl status httpd
```

Output:

```text
Active: active (running)
Status: Started, listening on: port 3000
```

---

## Firewall Investigation

Even though Apache was running and listening on port 3000, access from the Jump Host failed:

```bash
curl http://stapp01:3000
```

Error:

```text
curl: (7) Failed to connect to stapp01 port 3000: No route to host
```

### Inspect Firewall Rules

Checked iptables:

```bash
sudo iptables -L -n
```

Output showed:

```text
ACCEPT tcp -- anywhere anywhere tcp dpt:22
REJECT all -- anywhere anywhere reject-with icmp-host-prohibited
```

Only SSH (port 22) was allowed. All other incoming connections were being rejected.

---

## Firewall Fix

Inserted a rule allowing TCP traffic on port 3000:

```bash
sudo iptables -I INPUT 5 -p tcp --dport 3000 -j ACCEPT
```

Verified:

```bash
sudo iptables -L -n --line-numbers
```

Result:

```text
1 ACCEPT RELATED,ESTABLISHED
2 ACCEPT icmp
3 ACCEPT lo
4 ACCEPT tcp dpt:22
5 ACCEPT tcp dpt:3000
6 REJECT all -- reject-with icmp-host-prohibited
```

---

## Verification

### Confirm Apache is Listening

```bash
sudo ss -tulpn | grep :3000
```

Output:

```text
tcp LISTEN 0 511 *:3000 *:* users:(("httpd",pid=36232))
```

---

### Test Locally

```bash
curl http://localhost:3000
```

Returned the application HTML successfully.

---

### Test from Jump Host

```bash
curl http://stapp01:3000
```

Returned the expected webpage content successfully.

---

## Final Status

| Check                  | Status    |
| ---------------------- | --------- |
| Apache Installed       | Completed |
| Apache Running         | Completed |
| Listening on Port 3000 | Completed |
| Port Conflict Resolved | Completed |
| Firewall Rule Added    | Completed |
| Local Access Working   | Completed |
| Remote Access Working  | Completed |

---

## Commands Used

```bash
sudo grep -R "^Listen" /etc/httpd/conf /etc/httpd/conf.d
sudo ss -tulpn | grep :3000

sudo systemctl stop sendmail

sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl status httpd

sudo iptables -L -n
sudo iptables -I INPUT 5 -p tcp --dport 3000 -j ACCEPT
sudo iptables -L -n --line-numbers

curl http://localhost:3000
curl http://stapp01:3000
```

## Root Cause

Apache was correctly configured for port **3000**, but:

1. The port was initially occupied by **Sendmail**.
2. The server firewall (**iptables**) only allowed SSH traffic and rejected all other incoming connections.

After freeing the port and allowing TCP traffic on port **3000**, Apache became accessible from the Jump Host and the monitoring alert was resolved.
