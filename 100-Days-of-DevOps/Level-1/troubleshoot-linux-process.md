# Apache Service Recovery – Stratos DC

## Objective

Investigate the application servers in Stratos DC, identify the host where the Apache (`httpd`) service is unavailable, fix the issue, and ensure Apache is running on port **6400** on all application servers.

## Investigation

1. Logged into the affected application server.
2. Checked Apache service status:

```bash
systemctl status httpd
```

3. Observed Apache startup failure with the error:

```text
Address already in use
make_sock: could not bind to address
```

4. Verified Apache configuration:

```bash
grep -n "Listen" /etc/httpd/conf/httpd.conf
```

Output:

```text
Listen 6400
```

5. Identified the process using port 6400:

```bash
sudo ss -tulpn | grep :6400
```

Output showed:

```text
sendmail listening on 127.0.0.1:6400
```

## Root Cause

The `sendmail` service was already bound to port **6400**, preventing Apache from starting because Apache was configured to listen on the same port.

## Resolution

1. Stop the conflicting service:

```bash
sudo systemctl stop sendmail
```

2. (Optional for persistence) Disable the service:

```bash
sudo systemctl disable sendmail
```

3. Start Apache:

```bash
sudo systemctl start httpd
```

4. Verify Apache status:

```bash
sudo systemctl status httpd
```

5. Confirm Apache is listening on port 6400:

```bash
sudo ss -tulpn | grep :6400
```

Expected result:

```text
httpd listening on 0.0.0.0:6400
```

## Validation

Verify on all application servers:

```bash
systemctl is-active httpd
```

Expected output:

```text
active
```

Verify Apache port configuration:

```bash
grep "^Listen" /etc/httpd/conf/httpd.conf
```

Expected output:

```text
Listen 6400
```

## Result

Apache service was restored successfully. The port conflict caused by `sendmail` was resolved, and Apache is now running on port **6400** on all application servers.
