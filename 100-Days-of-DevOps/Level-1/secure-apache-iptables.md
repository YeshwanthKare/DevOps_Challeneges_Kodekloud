# Secure Apache Port Using iptables

## Objective

Configure firewall rules on all application servers to:

1. Install `iptables` and required dependencies.
2. Allow access to the Apache port only from the Load Balancer (LBR) host.
3. Block access from all other hosts.
4. Ensure firewall rules persist after reboot.

---

## Step 1: Identify the LBR IP Address

Login to the LBR host and run:

```bash
ip addr show eth0
```

Example:

```text
inet 10.244.235.23/32 scope global eth0
```

LBR IP:

```text
10.244.235.23
```

---

## Step 2: Login to Each App Server

Examples:

```bash
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03
```

---

## Step 3: Install iptables

```bash
sudo dnf install -y iptables iptables-services
```

Enable and start the service:

```bash
sudo systemctl enable iptables
sudo systemctl start iptables
```

---

## Step 4: Configure Firewall Rules

Variables:

```bash
LBR_IP="10.244.235.23"
APP_PORT="5003"
```

Allow traffic from the LBR host:

```bash
sudo iptables -I INPUT 1 -p tcp -s $LBR_IP --dport $APP_PORT -j ACCEPT
```

Block everyone else:

```bash
sudo iptables -I INPUT 2 -p tcp --dport $APP_PORT -j DROP
```

---

## Step 5: Persist Rules Across Reboots

Save the configuration:

```bash
sudo iptables-save > /etc/sysconfig/iptables
```

Restart the service:

```bash
sudo systemctl restart iptables
```

---

## Step 6: Verify Configuration

Check the rules:

```bash
sudo iptables -L INPUT -n --line-numbers
```

Expected output:

```text
1 ACCEPT tcp -- 10.244.235.23 0.0.0.0/0 tcp dpt:5003
2 DROP   tcp -- 0.0.0.0/0     0.0.0.0/0 tcp dpt:5003
```

Check service status:

```bash
sudo systemctl status iptables
```

---

## Automation Script

```bash
#!/bin/bash

LBR_IP="10.244.235.23"
APP_PORT="5003"

dnf install -y iptables iptables-services

systemctl enable iptables
systemctl start iptables

iptables -I INPUT 1 -p tcp -s "$LBR_IP" --dport "$APP_PORT" -j ACCEPT
iptables -I INPUT 2 -p tcp --dport "$APP_PORT" -j DROP

iptables-save > /etc/sysconfig/iptables

systemctl restart iptables

iptables -L INPUT -n --line-numbers
```

Run:

```bash
chmod +x secure_port.sh
sudo ./secure_port.sh
```

---

## Quick Template for Future Tasks

Replace only these values:

```bash
LBR_IP="<LB_IP>"
APP_PORT="<PORT>"
```

Everything else remains the same.
