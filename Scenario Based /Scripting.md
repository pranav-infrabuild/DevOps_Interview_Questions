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
