# Copy a File to Multiple Application Servers Using Ansible

## Objective

Use an Ansible playbook to copy the file **`/usr/src/data/index.html`** from the control node to all application servers and place it in **`/opt/data/index.html`**.

The playbook also ensures that the destination directory exists before copying the file.

---

## Environment

- Control Node: `jump-host`
- Inventory Group: `appservers`
- Source File: `/usr/src/data/index.html`
- Destination Directory: `/opt/data`
- Destination File: `/opt/data/index.html`

---

## Playbook

Create a playbook named `playbook.yml` with the following content:

```yaml
---
- name: Copy index.html to application servers
  hosts: appservers
  become: yes
  become_method: sudo

  tasks:
    - name: Ensure destination directory exists
      file:
        path: /opt/data
        state: directory
        owner: root
        group: root
        mode: "0755"

    - name: Copy index.html file to application servers
      copy:
        src: /usr/src/data/index.html
        dest: /opt/data/index.html
        owner: root
        group: root
        mode: "0644"
```

---

## Navigate to the Ansible Working Directory

```bash
cd /home/thor/ansible
```

---

## Run the Playbook

Execute the playbook using the inventory file:

```bash
ansible-playbook -i inventory playbook.yml
```

---

## Sample Output

```text
PLAY [Copy index.html to application servers]

TASK [Gathering Facts]
ok: [stapp01]
ok: [stapp02]
ok: [stapp03]

TASK [Ensure destination directory exists]
ok: [stapp01]
ok: [stapp02]
ok: [stapp03]

TASK [Copy index.html file to application servers]
changed: [stapp01]
changed: [stapp02]
changed: [stapp03]

PLAY RECAP
stapp01 : ok=3 changed=1 unreachable=0 failed=0
stapp02 : ok=3 changed=1 unreachable=0 failed=0
stapp03 : ok=3 changed=1 unreachable=0 failed=0
```

---

## Verification

Verify the file exists on each application server:

```bash
ssh tony@stapp01
ls -l /opt/data/index.html
```

```bash
ssh steve@stapp02
ls -l /opt/data/index.html
```

```bash
ssh banner@stapp03
ls -l /opt/data/index.html
```

Expected output:

```text
-rw-r--r-- 1 root root ... /opt/data/index.html
```

---

## Verify File Contents

```bash
cat /opt/data/index.html
```

The contents should match the source file located at:

```text
/usr/src/data/index.html
```

---

## Summary

This playbook:

- Connects to all hosts in the `appservers` inventory group.
- Creates the `/opt/data` directory if it does not already exist.
- Copies `index.html` from the control node to each application server.
- Sets the correct ownership and permissions on the destination file.
- Uses privilege escalation (`sudo`) to perform administrative tasks.

---

## Result

The file:

```text
/opt/data/index.html
```

is successfully deployed to all application servers with the following permissions:

```text
-rw-r--r--
```

and ownership:

```text
root:root
```
