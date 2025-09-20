<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:000000,100:FFD700&height=140&section=header&text=%F0%9F%90%A7%20LINUX%20TECHNICAL%20QUESTIONS&fontSize=28&fontColor=ffffff" />
</p>


### Question 1: What is kernal in Linux ?
<details>

- In Linux, the kernel is the core part of the operating system that acts as a bridge between the hardware of your computer and the software applications. It manages and controls all the hardware resources like CPU, memory, and storage, allowing programs to use them without having to know how they work at a low level.


</details>


### Question 2: How to Install Cronie and Start crond Service on RHEL ?
<details>

## **1. Install cronie package**

```bash
sudo yum install -y cronie
```

### **Explanation:**

* `yum` is the package manager for RHEL, CentOS, and older Fedora systems.
* `install` tells `yum` to install the package.
* `-y` automatically answers "yes" to any prompts during installation.
* `cronie` is the package that contains the **cron daemon**, which is the service responsible for running scheduled tasks on Linux.

> **Note:** On newer Fedora versions, `dnf` replaces `yum`, so the command would be:

```bash
sudo dnf install -y cronie
```

---

## **2. Enable crond service to start on boot**

```bash
sudo systemctl enable crond
```

### **Explanation:**

* `systemctl` is the command used to manage **systemd services**.
* `enable` tells systemd to automatically start the service at **boot time**.
* `crond` is the cron daemon service that executes scheduled tasks.

**Why this is important:**
Even if you start the cron service manually, it will stop after a reboot unless it is enabled to start automatically.

---

## **3. Start crond service immediately**

```bash
sudo systemctl start crond
```

### **Explanation:**

* `start` tells systemd to **immediately start the service** without rebooting.
* After running this, the cron daemon is actively running in the background, ready to execute scheduled tasks.

---

## **4. Check status of crond service**

```bash
sudo systemctl status crond
```

### **Explanation:**

* This command lets you verify whether the `crond` service is running correctly.
* The output includes:

  * `Active: active (running)` → service is running properly
  * `Loaded:` → shows whether the service is enabled at boot
  * `Main PID:` → the process ID of the cron daemon

**Example Output:**

```
● crond.service - Command Scheduler
   Loaded: loaded (/usr/lib/systemd/system/crond.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2025-09-20 19:00:00 IST; 2min ago
 Main PID: 1234 (crond)
```

---

## **5. Schedule jobs with crontab (optional next step)**

Once the service is running, you can schedule tasks using:

```bash
crontab -e
```

* Opens your personal cron table (list of scheduled tasks) in a text editor.
* Example cron entry:

```
0 5 * * * /home/user/backup.sh
```

* This runs `backup.sh` every day at 5:00 AM.

---
</details>
