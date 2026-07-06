# Create a Blank File on All Application Servers Using Ansible

## Objective

Use Ansible to create a blank file named:

```text
/home/webdata.txt
```

on all application servers with the following requirements:

- File permissions must be `0655`
- Ownership must be set per host:
  - `stapp01` → `tony:tony`
  - `stapp02` → `steve:steve`
  - `stapp03` → `banner:banner`

---

## Step 1: Create the Playbook Directory

```bash
mkdir -p ~/playbook
```

---

## Step 2: Create the Inventory File

Create the inventory file:

```bash
vi ~/playbook/inventory
```

Add the following content:

```ini
[app_servers]
stapp01 ansible_user=tony ansible_password=Ir0nM@n owner_name=tony
stapp02 ansible_user=steve ansible_password=Am3ric@ owner_name=steve
stapp03 ansible_user=banner ansible_password=BigGr33n owner_name=banner
```

---

## Step 3: Create the Playbook

Create the playbook file:

```bash
vi ~/playbook/playbook.yml
```

Add the following content:

```yaml
---
- name: Create /home/webdata.txt on all app servers
  hosts: app_servers
  become: yes

  tasks:
    - name: Ensure /home/webdata.txt exists with correct permissions and ownership
      file:
        path: /home/webdata.txt
        state: touch
        mode: "0655"
        owner: "{{ owner_name }}"
        group: "{{ owner_name }}"
```

---

## Step 4: Verify the Files

Verify the inventory:

```bash
cat ~/playbook/inventory
```

Verify the playbook:

```bash
cat ~/playbook/playbook.yml
```

---

## Step 5: Execute the Playbook

Navigate to the playbook directory:

```bash
cd ~/playbook
```

Run the playbook:

```bash
ansible-playbook -i inventory playbook.yml
```

---

## Verification

### App Server 1

```bash
ssh tony@stapp01
ls -l /home/webdata.txt
```

Expected output:

```text
-rw-r-xr-x 1 tony tony 0 Jul  6 13:47 /home/webdata.txt
```

### App Server 2

```bash
ssh steve@stapp02
ls -l /home/webdata.txt
```

Expected output:

```text
-rw-r-xr-x 1 steve steve ... /home/webdata.txt
```

### App Server 3

```bash
ssh banner@stapp03
ls -l /home/webdata.txt
```

Expected output:

```text
-rw-r-xr-x 1 banner banner ... /home/webdata.txt
```

---

## Permission Breakdown

```text
0655 = rw-r-xr-x
```

| User Type | Permissions |
| --------- | ----------- |
| Owner     | rw-         |
| Group     | r-x         |
| Others    | r-x         |

---

## Summary

This playbook:

- Creates `/home/webdata.txt` on all application servers.
- Sets file permissions to `0655`.
- Assigns ownership dynamically using host variables.
- Ensures each server has the correct owner and group.

### Ownership Mapping

```text
stapp01 → tony:tony
stapp02 → steve:steve
stapp03 → banner:banner
```

### Final Permissions

```text
-rw-r-xr-x
```
