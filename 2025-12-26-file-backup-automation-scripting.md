# **Notes: Fixing and Running rsync Backup Script via Cron on Linux (HTB Pwnbox)**

Goal:

  Synchronize files from /home/htb-ac-1948075/to_backup/ to /home/htb-ac-1948075/synced_backup/ using rsync via a scheduled cron job.

1. Initial Errors Observed

  - rsync: [receiver] mkstemp ".../.testfile.txt.*" failed: Permission denied (13)
  - rsync: [generator] failed to set times on ".../synced_backup/.": Operation not permitted (1)
  - Exit code 23: Some files/attributes were not transferred.

2. Fixes and Modifications Made

    A. Identified Permissions Issue
  - Checked directory permissions:
    ls -ld /home/htb-ac-1948075/synced_backup
  - Determined the current user lacked write access to synced_backup/.

    B. Fixed Permissions
  - Allowed write access by running:
    sudo chmod u+w /home/htb-ac-1948075/synced_backup

3. Running the Script

    A. Made the Script Executable
  - Ensured the script was runnable:
    sudo chmod +x /home/htb-ac-1948075/RSYNC_Backup.sh
  
    B. Ran the Script Manually for Testing
  - Executed the script with:
    sudo bash /home/htb-ac-1948075/RSYNC_Backup.sh
  - Verified that:
    - Files were copied successfully on the first run
    - No files were copied on subsequent runs (as expected)

4. Cron Job (Optional Verification)

    To verify or monitor the cron job:
  
  - Check what’s scheduled:
    crontab -l
  
  - Add logging to track output (optional):
    * * * * * /home/htb-ac-1948075/RSYNC_Backup.sh >> /home/htb-ac-1948075/cron.log 2>&1

Conclusion

  Your rsync backup setup now:

  - Runs without errors
  - Handles permissions correctly
  - Syncs only when changes occur
  - Can be run manually or automatically via cron

Bash Script written:

  #!/bin/bash
  /usr/bin/rsync -avz /home/htb-ac-1948075/to_backup/ /home/htb-ac-1948075/synced_backup/ >> /home/htb-ac-1948075/rsync_cron.log 2>&1

Directories and file creation:
  mkdir /home/htb-ac-1948075/to_backup/
  mkdir /home/htb-ac-1948075/synced_backup/

  touch /home/htb-ac-1948075/to_backup/testfile.txt
