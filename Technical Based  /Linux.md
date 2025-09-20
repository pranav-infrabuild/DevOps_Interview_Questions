## Linux Technical Questions 

### Question 1: What is kernal in Linux ?
<details>

- In Linux, the kernel is the core part of the operating system that acts as a bridge between the hardware of your computer and the software applications. It manages and controls all the hardware resources like CPU, memory, and storage, allowing programs to use them without having to know how they work at a low level.


</details>

### Question2: How to Install Cronie and Start crond Service on RHEL ?

<details>

### 1. Install cronie package
```bash
sudo yum install -y cronie

```

### 2. Enable crond service to start on boot
```bash
sudo systemctl enable crond

```

### 3. Start crond service immediately
```bash
sudo systemctl start crond

```

### 4. Check status of crond service
```bash
sudo systemctl status crond

```

</details>
