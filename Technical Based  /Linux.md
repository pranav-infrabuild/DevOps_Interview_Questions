<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:000000,100:FFD700&height=170&section=header&text=%F0%9F%90%A7%20LINUX%20TECHNICAL%20QUESTIONS&fontSize=34&fontColor=ffffff&animation=fadeIn" />
</p>

<p align="center">
  <b>🚀 Linux Interview Preparation | DevOps | SRE</b><br>
  <i>Structured notes with practical real-time understanding</i>
</p>

---

## 🧠 Linux Technical Questions

---

### 📌 Question 1: What is kernal in Linux ?
<details>
<summary><b>🔽 View Answer</b></summary>

#### Definition
- In Linux, the kernel is the core part of the operating system that acts as a bridge between the hardware of your computer and the software applications. It manages and controls all the hardware resources like CPU, memory, and storage, allowing programs to use them without having to know how they work at a low level.

</details>

---

### 📌 Question 2: How to Install Cronie and Start crond Service on RHEL ?
<details>
<summary><b>🔽 View Answer</b></summary>

#### Installation

    sudo yum install -y cronie

> Note: On newer Fedora versions, `dnf` replaces `yum`, so the command would be:

    sudo dnf install -y cronie

---

#### Enable Service

    sudo systemctl enable crond

---

#### Start Service

    sudo systemctl start crond

---

#### Check Status

    sudo systemctl status crond

---

#### Schedule Jobs

    crontab -e

Example:

    0 5 * * * /home/user/backup.sh

---

#### Explanation

- `yum` is the package manager for RHEL, CentOS, and older Fedora systems.  
- `install` tells `yum` to install the package.  
- `-y` automatically answers "yes" to any prompts during installation.  
- `cronie` is the package that contains the **cron daemon**, which is the service responsible for running scheduled tasks on Linux.  
- `systemctl` is the command used to manage **systemd services**.  
- `enable` tells systemd to automatically start the service at **boot time**.  
- `start` tells systemd to **immediately start the service** without rebooting.  

---

#### Real-time Note

- Even if you start the cron service manually, it will stop after a reboot unless it is enabled to start automatically.

</details>

---

### 📌 Question 3: Explain the booting process of Linux.
<details>
<summary><b>🔽 View Answer</b></summary>

#### Definition
The sequence of steps a Linux system follows from power-on to a usable state.

---

#### Practical Explanation

- BIOS/UEFI → checks hardware (POST), finds bootable device  
- Bootloader (GRUB) → loads the Linux kernel into memory  
- Kernel → initializes hardware, mounts root filesystem  
- Init/Systemd → starts system services and processes  
- Login prompt → system ready for user  

---

#### Real-time Example

On an Azure VM, when the instance restarts after a patch, systemd brings up services like sshd, docker, and nginx in the correct order based on service dependencies.

</details>

---

### 📌 Question 4: What are services in Linux?
<details>
<summary><b>🔽 View Answer</b></summary>

#### Definition
Background processes (daemons) that run continuously to provide functionality.

---

#### Practical Explanation
Managed by systemd, services start automatically at boot, can be stopped/started/restarted, and have defined dependencies.

---

#### Commands

    systemctl status nginx        # Check nginx service
    systemctl restart docker      # Restart Docker daemon
    systemctl enable kubelet      # Auto-start kubelet on boot
    journalctl -u nginx --since "1 hour ago"  # Check service logs

---

#### Real-time Example

In production, if a deployment causes the app service to crash, I'd run systemctl status and journalctl to quickly diagnose before rollback.

</details>

---

### 📌 Question 5: How do you check open ports in Linux?
<details>
<summary><b>🔽 View Answer</b></summary>

#### Definition
Identifying which network ports are actively listening or connected on a system.

---

#### Practical Explanation & Commands

    ss -tuln
    netstat -tuln
    ss -tuln | grep :8080
    lsof -i :443
    sudo fuser 80/tcp

---

#### Real-time Example

During a Kubernetes pod networking issue, I used ss -tuln on the node to verify if the NodePort 30080 was actually open, then cross-checked with iptables -L to debug the routing.

</details>

---

### 📌 Question 6: Explain the Linux file system structure.
<details>
<summary><b>🔽 View Answer</b></summary>

#### Definition
A hierarchical directory layout where everything starts from root /.

</details>

---

### 📌 Question 7: Command to check OS version in Linux?
<details>
<summary><b>🔽 View Answer</b></summary>

#### Commands

    cat /etc/os-release        # Most reliable — works everywhere  
    lsb_release -a             # Detailed on Debian/Ubuntu  
    uname -r                   # Kernel version  
    hostnamectl                # OS + kernel + architecture together  
    cat /etc/redhat-release    # RHEL/CentOS specific  

---

#### Real-time Example

Before installing a new package or agent (like Datadog or Azure Monitor), I always run cat /etc/os-release first to confirm the distro and version, since package managers and install scripts differ between Ubuntu, RHEL, and Amazon Linux.

</details>
