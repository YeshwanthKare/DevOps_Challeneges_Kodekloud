# Install Ansible 4.10.0 Using pip3 on the Jump Host

## Objective

The Nautilus DevOps team selected **Ansible** as their first configuration management and automation tool because of its agentless architecture, simple setup, and minimal requirements on managed nodes.

The goal is to configure the **jump host** as the Ansible controller by installing **Ansible version 4.10.0** using **pip3 only** and ensuring it is available globally for all users on the system.

---

## Requirements

- Install **Ansible 4.10.0**
- Use **pip3 only**
- Perform the installation on the **jump host**
- Make Ansible accessible to **all users**

---

## Why Ansible?

Ansible is a popular automation tool because:

- It is agentless and communicates over SSH.
- It requires minimal setup on managed nodes.
- It uses human-readable YAML playbooks.
- It integrates easily with CI/CD pipelines and automation workflows.
- It simplifies configuration management at scale.

---

## Why Install Using pip3?

Installing Ansible with `pip3` provides:

- Precise version control
- Easy upgrades and rollbacks
- Consistent behavior across environments
- Better suitability for lab and certification scenarios

Version pinning ensures that the exact required release is installed.

---

## Understanding Ansible Versions

Ansible consists of two major components:

### ansible-core

Contains the execution engine and core functionality.

### Ansible Package

Bundles `ansible-core` together with supported collections and plugins.

Installing:

```bash
sudo pip3 install ansible==4.10.0
```

automatically installs the compatible version of `ansible-core` required by Ansible 4.10.0.

---

## Step 1: Verify Python and pip3

Check the Python version:

```bash
python3 --version
```

Check the pip3 version:

```bash
pip3 --version
```

If pip3 is not available, install it:

```bash
sudo yum install -y python3-pip
```

---

## Step 2: Upgrade pip3

Upgrade pip to avoid dependency and compatibility issues:

```bash
sudo pip3 install --upgrade pip
```

---

## Step 3: Install Ansible 4.10.0 Globally

Install the required Ansible version:

```bash
sudo pip3 install ansible==4.10.0
```

### Explanation

- `pip3` uses Python 3.
- `ansible==4.10.0` installs the exact required version.
- `sudo` performs a system-wide installation.

This makes Ansible available globally, typically through:

```text
/usr/local/bin/ansible
```

and allows all users on the system to execute Ansible commands.

---

## Step 4: Verify the Installation

Check the installed version:

```bash
ansible --version
```

Expected output:

```text
ansible [core x.x.x]
  ansible version 4.10.0
```

This confirms that Ansible 4.10.0 has been installed successfully.

---

## Step 5: Validate Global Availability

Switch to another user:

```bash
su - thor
```

Verify Ansible access:

```bash
ansible --version
```

If the command executes successfully, the installation is available system-wide and satisfies the requirement.

---

## Common Pitfalls

### Installing Without sudo

```bash
pip3 install ansible==4.10.0
```

This installs Ansible only for the current user and does not meet the global availability requirement.

### Mixing Package Managers

Avoid installing Ansible using both:

```bash
yum install ansible
```

and

```bash
pip3 install ansible
```

Mixing installation methods can lead to version conflicts.

### Not Pinning the Version

Using:

```bash
sudo pip3 install ansible
```

may install the latest release rather than the required version.

Always specify:

```bash
sudo pip3 install ansible==4.10.0
```

---

## Best Practices

- Use pip when a specific Ansible version is required.
- Pin package versions for reproducibility.
- Use a dedicated jump host as the Ansible controller.
- Avoid mixing system package managers and pip installations.
- Verify installations after deployment.

---

## Verification Commands

```bash
python3 --version
pip3 --version

sudo pip3 install --upgrade pip
sudo pip3 install ansible==4.10.0

ansible --version
```

---

## Conclusion

The jump host has been successfully configured as an Ansible controller by installing **Ansible 4.10.0** using **pip3**. The installation is performed globally, ensuring that all users on the system can access and execute Ansible commands. This provides a solid foundation for infrastructure automation and configuration management within the Nautilus DevOps environment.
