# Ansible Privilege Escalation Configuration

This repository contains the dedicated Ansible configuration for the Nautilus DevOps team. It resolves task failures related to superuser privileges (such as package installations) by forcing Ansible to prompt the operator for a `sudo` password at runtime.

## Requirements Met

- Automatically enables privilege escalation (`become`) for playbooks.
- Utilizes `sudo` to elevate privileges to the `root` user.
- Prompts the operator interactively for the `sudo` password on execution.
- Restricts changes exclusively to the existing project configuration file.

## Configuration Details

The adjustments are located in the project's central configuration file:
`#/home/thor/ansible-t5q4/ansible-t5q4.cfg`

The `[privilege_escalation]` block is configured as follows:

```ini
[privilege_escalation]
become = True
become_method = sudo
become_user = root
become_ask_pass = True
```

### Parameter Breakdown

- **`become = True`**: Activates privilege escalation globally for all plays and tasks.
- **`become_method = sudo`**: Specifies `sudo` as the mechanism for elevation.
- **`become_user = root`**: Escalates operations to the superuser (`root`) account.
- **`become_ask_pass = True`**: Triggers an interactive prompt for the remote `sudo` password when the playbook initializes.

## Usage Instructions

Because this project utilizes a custom configuration file path (`/home/thor/ansible-t5q4/ansible-t5q4.cfg`), you must explicitly point Ansible to it before execution.

### 1. Set the Configuration Variable

Export the `ANSIBLE_CONFIG` environment variable in your terminal session:

```bash
export ANSIBLE_CONFIG=/home/thor/ansible-t5q4/ansible-t5q4.cfg
```

### 2. Execute the Playbook

Run your playbook normally. Ansible will immediately pause and prompt you for the password:

```bash
ansible-playbook site.yml
```

_Note: You no longer need to append the `-K` or `--ask-become-pass` flags to your command._
