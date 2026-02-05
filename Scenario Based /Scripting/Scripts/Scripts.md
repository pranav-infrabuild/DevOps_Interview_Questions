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


### 2. Backup Script
<details>


```bash
#!/bin/bash
tar -czf backup_$(date +%F).tar.gz /path/to/directory
```

---

## 🔍 What Does It Do?

Creates a compressed backup of a directory with today's date in the filename.

---

## 🛠️ Command Breakdown

| Part | Symbol | What It Does |
|------|--------|--------------|
| `tar` | 📦 | Tool to archive files |
| `-c` | ➕ | **C**reate a new archive |
| `-z` | 🗜️ | Compress using **g**zip |
| `-f` | 📝 | Specify the **f**ile name |
| `$(date +%F)` | 📅 | Adds today's date (YYYY-MM-DD) |
| `/path/to/directory` | 📂 | Folder you want to back up |

---

## 📤 Example Output

```
backup_2026-01-29.tar.gz
```

---

## 📍 Where Is The File Stored?

⚠️ The backup file is created in the **current directory** where you run the script.

### 💡 Save to a Specific Location

```bash
tar -czf /backup/backup_$(date +%F).tar.gz /path/to/directory
```

This saves the backup to `/backup/` instead.

---

## 🔄 Auto-Delete Old Backups

Add this line to keep only the last 7 days of backups:

```bash
find /backup -name "backup_*.tar.gz" -mtime +7 -delete
```

**Full script with cleanup:**

```bash
#!/bin/bash
tar -czf /backup/backup_$(date +%F).tar.gz /path/to/directory
find /backup -name "backup_*.tar.gz" -mtime +7 -delete
```

---

## ⏰ Schedule with Cron

Run backup automatically every day at 2 AM:

```bash
crontab -e
```

Add this line:

```
0 2 * * * /path/to/your/backup_script.sh
```

### 📋 Cron Schedule Examples

| Schedule | Cron Expression | Description |
|----------|----------------|-------------|
| Daily at 2 AM | `0 2 * * *` | Every day |
| Every 6 hours | `0 */6 * * *` | 4 times a day |
| Weekly (Sunday 3 AM) | `0 3 * * 0` | Once a week |
| Monthly (1st, 2 AM) | `0 2 1 * *` | Once a month |

---

## ✅ Quick Start Checklist

- [ ] Replace `/path/to/directory` with your actual folder
- [ ] Make the script executable: `chmod +x backup_script.sh`
- [ ] Test it: `./backup_script.sh`
- [ ] Check the backup file was created
- [ ] (Optional) Set up cron scheduling
- [ ] (Optional) Add auto-cleanup for old backups

---

## 🆘 Troubleshooting

| Problem | Solution |
|---------|----------|
| Permission denied | Run with `sudo` or check folder permissions |
| File not found | Verify the path to your directory is correct |
| No space left | Check disk space with `df -h` |
| Cron not running | Check logs: `grep CRON /var/log/syslog` |

---

**🎯 Pro Tip:** Always test your backup and practice restoring from it! A backup is only good if you can restore it.

To extract your backup:
```bash
tar -xzf backup_2026-01-29.tar.gz
```
</details>


### 3. Docker Health Check Script

<details>



## Overview

A lightweight bash script for monitoring Docker container health status. This script checks if a specified container is running and returns appropriate exit codes for monitoring systems.

## Script

```bash
#!/bin/bash
CONTAINER="my_container"
if docker inspect -f '{{.State.Running}}' "$CONTAINER" 2>/dev/null | grep -q true; then
    echo "OK: Container is running"
    exit 0
else
    echo "CRITICAL: Container is down"
    exit 1
fi
```

## How It Works

1. **Container Variable**: Defines the target container name in `CONTAINER` variable
2. **Docker Inspect**: Uses `docker inspect` to check the container's running state
3. **Error Handling**: Redirects errors to `/dev/null` to suppress error messages
4. **Status Check**: Greps for `true` to verify the container is running
5. **Exit Codes**: Returns `0` for success, `1` for failure (standard monitoring convention)

## Usage

### Basic Setup

1. Save the script to a file:
   ```bash
   nano docker-health-check.sh
   ```

2. Make it executable:
   ```bash
   chmod +x docker-health-check.sh
   ```

3. Update the container name:
   ```bash
   CONTAINER="your_container_name"
   ```

4. Run the script:
   ```bash
   ./docker-health-check.sh
   ```


- Docker installed and running
- Appropriate permissions to execute Docker commands
- Bash shell environment

## Best Practices

1. **Use environment variables** for container names in production
2. **Log output** to a file for troubleshooting
3. **Set up alerts** for critical failures
4. **Test thoroughly** before deploying to production
5. **Consider using Docker's native health checks** in `docker-compose.yml`

## Troubleshooting

### Permission Denied
Add user to docker group:
```bash
sudo usermod -aG docker $USER
```

### Container Not Found
Verify container name:
```bash
docker ps -a --format "{{.Names}}"
```

### Script Not Executing
Check file permissions and shebang line:
```bash
ls -l docker-health-check.sh
head -n 1 docker-health-check.sh
```


</details>
