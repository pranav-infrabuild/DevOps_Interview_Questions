# Daily Scripts

## 1 File Backup Script 
<details>


```bash
#!/bin/bash

backup_dir="/path/to/backup"
source_dir="/path/to/source"

tar -czf "$backup_dir/backup_$(date +%Y%m%d_%H%M%S).tar.gz" "$source_dir"
```

### Explanation:
- `backup_dir="/path/to/backup"`: Specifies the directory where you want to save the backup.
- `source_dir="/path/to/source"`: Specifies the directory that you want to back up.
- `tar -czf "$backup_dir/backup_$(date +%Y%m%d_%H%M%S).tar.gz" "$source_dir"`: Creates a compressed tarball of the source directory with a timestamp in the filename and saves it in the backup directory.

### Next, the tar command is used to create a compressed archive of the source_dir. The -czf options are used where:

- c stands for 'create' a new archive,
- z compresses the archive using gzip, and
- f allows you to specify the name of the resulting file.

### Save the Script:
- Save the script with a meaningful name like backup.sh.
- Place the script in a directory like /usr/local/bin/ for easy access.


### Make the Script Executable:
   - Run the following command to make the script executable:
     ```bash
     chmod +x /usr/local/bin/backup.sh
     ```

### Test the Script:
   - Manually run the script to ensure it works as expected:
     ```bash
     /usr/local/bin/backup.sh
     ```
   - Verify that the backup file is created in the specified backup directory.

### Automate the Backup:

### Understanding Cron:
- Cron is a time-based job scheduler in Unix-like operating systems. It allows you to run scripts or commands at scheduled intervals.
   
   - **Schedule with Cron:**
     - Use `cron` to automate the backup process. Open the crontab file for editing:
       ```bash
       crontab -e
       ```
     - Add a cron job to run the backup script at your desired interval. For example, to run the script every day at 2:00 AM:
       ```bash
       0 2 * * * /usr/local/bin/backup.sh
       ```
       Here’s a breakdown of the time fields:

- Minute: (0-59)
- Hour: (0-23)
- Day of Month: (1-31)
- Month: (1-12)
- Day of Week: (0-7, where both 0 and 7 represent Sunday)
     - Save and close the crontab file. This schedules the script to run automatically.


### Verify Backups Regularly:
   - Regularly check the backup files to ensure they are being created correctly and can be restored if needed.
   - Perform periodic restoration tests to verify the integrity of your backups.

### Implement Retention Policies:
   - Cleanup Old Backups:
     - To manage disk space, consider adding a command to delete old backups, e.g., keeping only the last 7 days' worth of backups:
       ```bash
       find "$backup_dir" -type f -name "*.tar.gz" -mtime +7 -exec rm {} \;
       ```


</details>


## 2. System Monitoring Script/Cpu Utilisation 

<details>

 ### **Script Overview:**

```bash
#!/bin/bash

threshold=90
cpu_usage=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d. -f1)

if [ "$cpu_usage" -gt "$threshold" ]; then
    echo "High CPU usage detected: $cpu_usage%"

    # Add alert/notification logic here

fi
```

### **Detailed Script Explanation:**

1. **Shebang (`#!/bin/bash`):**
   - The shebang at the top specifies that the script should be run using the Bash shell.

2. **Threshold Setting (`threshold=90`):**
   - The `threshold` variable is set to `90`, meaning the script will check if the CPU usage exceeds 90%.

3. **CPU Usage Calculation (`cpu_usage=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d. -f1)`):**
   - **`top -bn1`**: Runs the `top` command in batch mode (`-b`) for a single iteration (`-n1`). The `top` command provides a dynamic view of system processes, including CPU usage.
   - **`grep "Cpu(s)"`**: Filters the output of `top` to find the line containing CPU statistics.
   - **`awk '{print $2}'`**: Extracts the second field from the line, which represents the percentage of CPU usage by user processes (e.g., `12.5%us`).
   - **`cut -d. -f1`**: Splits the CPU usage value at the decimal point (`.`) and takes the first part, effectively converting a value like `12.5` to `12`. This simplifies comparison by converting the floating-point number to an integer.

4. **Conditional Check (`if [ "$cpu_usage" -gt "$threshold" ]; then`):**
   - The `if` statement checks if the current CPU usage (`$cpu_usage`) is greater than the specified threshold (`$threshold`).
   - **`-gt`**: Stands for "greater than" and is used to compare integer values in Bash.

5. **Action on High CPU Usage (`echo "High CPU usage detected: $cpu_usage%"`):**
   - If the CPU usage exceeds the threshold, the script prints a warning message, indicating high CPU usage.
   - The line `# Add alert/notification logic here` is a placeholder where you would add additional logic to handle high CPU usage, such as sending an email, triggering an alert in a monitoring tool, or logging the incident.


### **How It Works in Production:**

1. **Deployment:**
   - After storing the script in your Azure Repo and setting it up on your server, you can deploy it across your infrastructure using a CI/CD pipeline in Azure DevOps.

2. **Automation:**
   - The script can be executed periodically via cron to monitor CPU usage continuously. If the threshold is breached, it triggers the predefined actions, such as sending alerts.

3. **Monitoring and Alerts:**
   - Alerts generated by the script are received by the relevant teams or monitoring systems, allowing for quick responses to potential issues before they escalate.


Integrating the system monitoring script with PagerDuty allows you to automatically trigger alerts and notifications when high CPU usage is detected. Here’s how you can implement this:

### **1. Set Up a PagerDuty Service:**

1. **Create a Service in PagerDuty:**
   - Log in to your PagerDuty account.
   - Go to the **Services** tab and click on **+ New Service**.
   - Name your service (e.g., "CPU Monitoring Service").
   - Configure the escalation policy, which determines who gets notified and when.

2. **Create an Integration Key:**
   - In the service configuration, scroll down to the **Integrations** section.
   - Click **+ New Integration** and select **Events API v2**.
   - Name the integration (e.g., "CPU Monitoring Script Integration").
   - PagerDuty will generate an **Integration Key**. Copy this key, as you’ll need it in your script.

### **2. Install `pd-send` or Use a Curl Command:**

There are a couple of ways to send alerts to PagerDuty from your script:

#### **Option 1: Using `pd-send` (PagerDuty CLI tool):**

1. **Install `pd-send`:**
   - Install the PagerDuty CLI tool on your server:
     ```bash
     sudo apt-get install pagerduty-cli
     ```
   - Alternatively, you can use `pip` if you prefer a Python package:
     ```bash
     pip install pagerduty-cli
     ```

2. **Configure `pd-send`:**
   - Use the integration key from your PagerDuty service:
     ```bash
     pd-send -k <YOUR_INTEGRATION_KEY> -t trigger -d "High CPU usage detected: $cpu_usage%"
     ```

```
#!/bin/bash

threshold=90
cpu_usage=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}' | cut -d. -f1)
integration_key="YOUR_INTEGRATION_KEY"

if [ "$cpu_usage" -gt "$threshold" ]; then
    echo "High CPU usage detected: $cpu_usage%"
```
    
