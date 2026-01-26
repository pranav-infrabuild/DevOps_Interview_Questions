### 1. Monitor Disk Usage

<details>

# 💾 Bash Script: Disk Usage Report

## 📄 The Script

```bash
#!/bin/bash
df -h > disk_usage_report.txt
```

This script checks how much disk space is used and saves that information to a file.

---

## 🔍 Breaking Down the Script

### 1️⃣ **`#!/bin/bash`**

It tells the system:  
👉 *"Run this script using the Bash shell"*

This is called a **shebang** or **hashbang** - it specifies which interpreter to use.

---

### 2️⃣ **`df` → Disk Free**

Shows:
- ✅ Total disk size
- ✅ Used space
- ✅ Available space
- ✅ Mount points

**Example output:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   28G  42% /
/dev/sdb1       100G   75G   25G  75% /data
```

---

### 3️⃣ **`-h` option**

Means: **human-readable**

Displays sizes in:
- 📊 **KB** (Kilobytes)
- 📊 **MB** (Megabytes)
- 📊 **GB** (Gigabytes)

Instead of raw bytes

**Without `-h`:**
```
Filesystem     1K-blocks      Used Available Use% Mounted on
/dev/sda1       52428800  20971520  29360128  42% /
```

**With `-h`:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   28G  42% /
```

---

### 4️⃣ **`>` operator**

Means:  
👉 *"Take the output of the command and write it to a file"*

**Important:**
- ✅ `>` **overwrites** the file if it exists
- ✅ `>>` **appends** to the file instead of overwriting

**Examples:**
```bash
# Overwrites the file each time
df -h > disk_usage_report.txt

# Appends to the file (adds to the end)
df -h >> disk_usage_report.txt
```

---

### 5️⃣ **`disk_usage_report.txt`**

This is the **file name** where the output will be saved.

📍 **Default location:**  
👉 It will be stored in the **current working directory** from where you run the script

**Example:**
```bash
# If you run the script from /home/user/
# File will be created at:
/home/user/disk_usage_report.txt
```

---

## ✅ Best Practice: Use an Absolute Path

Instead of relying on the current directory, specify the **full path**:

```bash
#!/bin/bash
df -h > /var/log/disk_usage_report.txt
```

Now the file will **always** be stored in:
```
/var/log/disk_usage_report.txt
```

**Benefits:**
- 🎯 Consistent location regardless of where script runs
- 🔍 Easier to find and monitor
- 📋 Better for automation and logging

---

## 🚀 Enhanced Script Examples

### 📅 **Add timestamp to filename:**
```bash
#!/bin/bash
DATE=$(date +%Y-%m-%d_%H-%M-%S)
df -h > /var/log/disk_usage_$DATE.txt
```

**Result:**
```
/var/log/disk_usage_2026-01-26_14-30-45.txt
```

---

### 📊 **Add header and timestamp to report:**
```bash
#!/bin/bash
REPORT_FILE="/var/log/disk_usage_report.txt"

echo "Disk Usage Report - $(date)" > $REPORT_FILE
echo "================================" >> $REPORT_FILE
df -h >> $REPORT_FILE
```

**Output in file:**
```
Disk Usage Report - Mon Jan 26 14:30:45 UTC 2026
================================
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   28G  42% /
```

---

### ⚠️ **Alert if disk usage exceeds threshold:**
```bash
#!/bin/bash
THRESHOLD=80
USAGE=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')

if [ $USAGE -gt $THRESHOLD ]; then
    echo "WARNING: Disk usage is at ${USAGE}%"
    df -h > /var/log/disk_usage_alert.txt
fi
```

---

## 📋 Common Use Cases

| Scenario | Command |
|----------|---------|
| One-time report | `df -h > report.txt` |
| Daily append | `df -h >> daily_report.txt` |
| Timestamped | `df -h > disk_$(date +%F).txt` |
| Specific filesystem | `df -h /dev/sda1 > sda1_report.txt` |
| Email report | `df -h \| mail -s "Disk Report" admin@example.com` |

---

## 🎯 Interview Tips

> *"This script uses `df -h` to generate a human-readable disk usage report and redirects the output to a file. In production, I would use an absolute path like `/var/log/disk_usage.txt` and add timestamping. I'd also implement monitoring to alert when disk usage exceeds thresholds, and use `>>` if I want to track usage over time."*
</details>
