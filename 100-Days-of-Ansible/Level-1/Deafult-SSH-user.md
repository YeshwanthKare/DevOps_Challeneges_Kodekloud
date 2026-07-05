# Configure Default SSH User in Ansible (Jump Host)

## Objective

On the jump host, configure Ansible to use **`ammar`** as the default SSH user for all managed hosts by modifying the global Ansible configuration file.

This must be done in the **default Ansible configuration**, not by creating a new config file.

---

## Step 1: Open Ansible Configuration File

Edit the default Ansible configuration:

```bash
sudo vi /etc/ansible/ansible.cfg
```

---

## Step 2: Set Default Remote User

Inside the file, ensure the following configuration exists under the `[defaults]` section:

```ini
[defaults]
remote_user = ammar
```

### Important Notes:

- Do NOT remove existing valid settings.
- Ensure `remote_user` is placed under `[defaults]`.
- Do NOT create a new configuration file.

---

## Step 3: Save and Exit

In `vi` editor:

```text
:wq
```

---

## Step 4: Verify Configuration

Run the following command to confirm the change:

```bash
ansible-config dump | grep -i remote_user
```

---

## Expected Output

```text
DEFAULT_REMOTE_USER(default: ammar) = ammar
```

---

## Summary

- Modified `/etc/ansible/ansible.cfg`
- Set default SSH user to `ammar`
- Ensured configuration is under `[defaults]`
- Verified using `ansible-config dump`

---

## Result

Ansible will now use:

```text
ssh user: ammar
```

for all hosts by default unless overridden in inventory or playbooks.
