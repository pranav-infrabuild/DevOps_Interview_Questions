## Linux Technical Questions 

### Question 1: What is kernal in Linux ?
<details>

- In Linux, the kernel is the core part of the operating system that acts as a bridge between the hardware of your computer and the software applications. It manages and controls all the hardware resources like CPU, memory, and storage, allowing programs to use them without having to know how they work at a low level.


</details>

### Question 2. What is `sshd` in Linux, and how can you disable direct root login for better security?

<details>

### 🔹 What ✓is `sshd`?
- `sshd` stands for **Secure Shell Daemon**.  
- It is the background service that runs on a Linux server and **listens for incoming SSH connections**.  
- When you connect using `ssh user@server`, the request is handled by `sshd`.

---

### 🔹 Why disable root login?
- Logging in directly as root is risky because attackers often target the root account.  
- Best practice: **log in as a normal user** and use `sudo` for admin tasks.

---

### 🔹 Steps to disable root login in SSH:

1. **Open the sshd configuration file:**
   ```bash
   sudo vi /etc/ssh/sshd_config

   ```

2. PermitRootLogin yes change it to no
3. Save the file and restart service
   ```bash
   sudo systemctl restart sshd

   ```

</details>
