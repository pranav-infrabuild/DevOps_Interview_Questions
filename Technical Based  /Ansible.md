## Ansible

### What is Ansible?

Ansible is an open-source automation tool used for IT tasks like configuration management, application deployment, and task automation. It helps you manage systems by defining how they should be configured and automates the process, reducing manual work.

### Advantages of Ansible

1. **Simple and Easy to Learn**: Ansible uses simple YAML language for its configurations, making it easy to write and understand.
2. **Agentless**: Ansible doesn’t require any software or agent installed on the target machines, reducing overhead.
3. **Flexible**: You can automate nearly anything, from cloud provisioning to software deployment.
4. **Idempotent**: Ansible ensures that your tasks are executed only if needed, meaning it won’t repeat actions if they’re already done.
5. **Scalable**: It’s easy to manage a large number of systems with Ansible, from a few servers to thousands.

### What are Playbooks?

Playbooks are files where Ansible’s instructions are written. These instructions are in YAML format, and they define the tasks you want to execute on your managed systems. Playbooks are the heart of Ansible’s automation, as they allow you to describe the desired state of your systems.

### How to Write Playbooks

Writing an Ansible playbook is simple and consists of:

1. **Hosts**: Define which machines the playbook will run on.
2. **Tasks**: Describe the actions you want to take, like installing software or copying files.
3. **Modules**: Use built-in Ansible modules (pre-defined commands) for tasks like installing packages, managing files, or configuring services.

**Example of a Simple Playbook:**

```yaml
---
- name: Install and start Apache
  hosts: webservers
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
    - name: Start Apache service
      service:
        name: apache2
        state: started
```

### What are Ad-hoc Commands?

Ad-hoc commands are simple, one-line commands that you can run without writing a playbook. They are useful for quick tasks like checking the status of a service, copying files, or rebooting servers.

**Example of an Ad-hoc Command:**

```bash
ansible webservers -m ping
```

This command checks if the `webservers` are reachable using the `ping` module.

### 10 Common Ansible Commands Asked in DevOps Interviews

1. **Check Connectivity:**
   ```bash
   ansible all -m ping
   ```
   Tests connectivity to all hosts.

2. **Gather Facts:**
   ```bash
   ansible all -m setup
   ```
   Collects and displays system information (facts).

3. **Copy Files:**
   ```bash
   ansible webservers -m copy -a "src=/path/to/source dest=/path/to/dest"
   ```
   Copies files to remote hosts.

4. **Install a Package:**
   ```bash
   ansible webservers -m apt -a "name=nginx state=present"
   ```
   Installs the `nginx` package on Debian-based systems.

5. **Run a Command:**
   ```bash
   ansible all -m shell -a "uptime"
   ```
   Runs a shell command on all hosts.

6. **Restart a Service:**
   ```bash
   ansible webservers -m service -a "name=apache2 state=restarted"
   ```
   Restarts the Apache service.

7. **Change File Permissions:**
   ```bash
   ansible all -m file -a "path=/etc/myfile mode=644"
   ```
   Changes permissions of a file.

8. **Create a User:**
   ```bash
   ansible all -m user -a "name=john state=present"
   ```
   Creates a user named `john`.

9. **Check Disk Space:**
   ```bash
   ansible all -a "df -h"
   ```
   Displays disk space usage on all hosts.

10. **Reboot Hosts:**
    ```bash
    ansible all -m reboot
    ```
    Reboots all hosts.

These commands and concepts are essential for working with Ansible in DevOps roles, helping you automate and manage infrastructure efficiently.
