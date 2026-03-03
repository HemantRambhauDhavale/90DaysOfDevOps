# Day 19 – Shell Scripting Project: Log Rotation, Backup & Crontab

---

## Task 1 – Log Rotation Script

### log_rotate.sh

#!/bin/bash
set -euo pipefail

LOG_DIR=$1

if [ ! -d "$LOG_DIR" ]; then
  echo "Directory does not exist"
  exit 1
fi

COMPRESSED=$(find "$LOG_DIR" -name "*.log" -mtime +7 -exec gzip {} \; -print | wc -l)
DELETED=$(find "$LOG_DIR" -name "*.gz" -mtime +30 -delete -print | wc -l)

echo "Compressed files: $COMPRESSED"
echo "Deleted old archives: $DELETED"

---

## Task 2 – Backup Script

### backup.sh

#!/bin/bash
set -euo pipefail

SOURCE=$1
DEST=$2
DATE=$(date +%Y-%m-%d)
ARCHIVE="backup-$DATE.tar.gz"

if [ ! -d "$SOURCE" ]; then
  echo "Source directory does not exist"
  exit 1
fi

tar -czf "$DEST/$ARCHIVE" "$SOURCE"

if [ -f "$DEST/$ARCHIVE" ]; then
  echo "Backup created: $ARCHIVE"
  ls -lh "$DEST/$ARCHIVE"
else
  echo "Backup failed"
  exit 1
fi

find "$DEST" -name "backup-*.tar.gz" -mtime +14 -delete

---

## Task 3 – Cron Entries

Run log_rotate.sh daily at 2 AM:
0 2 * * * /path/to/log_rotate.sh /var/log/myapp

Run backup.sh every Sunday at 3 AM:
0 3 * * 0 /path/to/backup.sh /source /backup

Run health check every 5 minutes:
*/5 * * * * /path/to/health_check.sh

---

## Task 4 – Maintenance Script

### maintenance.sh

#!/bin/bash
set -euo pipefail

LOGFILE="/var/log/maintenance.log"

echo "$(date): Starting maintenance" >> $LOGFILE

/path/to/log_rotate.sh /var/log/myapp >> $LOGFILE 2>&1
/path/to/backup.sh /source /backup >> $LOGFILE 2>&1

echo "$(date): Maintenance completed" >> $LOGFILE

Cron entry to run daily at 1 AM:
0 1 * * * /path/to/maintenance.sh

---

## What I Learned

1. Automation reduces manual work.
2. Cron helps schedule tasks without supervision.
3. Error handling prevents silent failures.
