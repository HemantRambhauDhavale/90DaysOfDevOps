# Day 20 – Bash Log Analyzer Project

## Script: log_analyzer.sh

#!/bin/bash
set -euo pipefail

LOGFILE=${1:-}

if [ -z "$LOGFILE" ]; then
  echo "Usage: ./log_analyzer.sh <logfile>"
  exit 1
fi

if [ ! -f "$LOGFILE" ]; then
  echo "Error: File does not exist"
  exit 1
fi

DATE=$(date +%Y-%m-%d)
REPORT="log_report_$DATE.txt"

TOTAL_LINES=$(wc -l < "$LOGFILE")
ERROR_COUNT=$(grep -Ei "ERROR|Failed" "$LOGFILE" | wc -l)

echo "Analyzing log file: $LOGFILE"

echo "Total errors found: $ERROR_COUNT"

echo "--- Critical Events ---"
grep -n "CRITICAL" "$LOGFILE"

echo "--- Top 5 Error Messages ---"

TOP_ERRORS=$(grep "ERROR" "$LOGFILE" \
| awk '{$1=$2=$3=""; print}' \
| sort \
| uniq -c \
| sort -rn \
| head -5)

echo "$TOP_ERRORS"

{
echo "Log Analysis Report"
echo "Date: $DATE"
echo "Log File: $LOGFILE"
echo "Total Lines: $TOTAL_LINES"
echo "Total Errors: $ERROR_COUNT"

echo ""
echo "Top 5 Error Messages:"
echo "$TOP_ERRORS"

echo ""
echo "Critical Events:"
grep -n "CRITICAL" "$LOGFILE"

} > "$REPORT"

echo "Report generated: $REPORT"

mkdir -p archive
mv "$LOGFILE" archive/

echo "Log file moved to archive/"

## Tools Used

grep – search log patterns  
awk – process text fields  
sort – sort data  
uniq – count repeated messages  
wc – count lines

## What I Learned

1. Logs help identify system failures quickly.
2. Linux text processing tools are powerful for analysis.
3. Automation can simplify debugging and monitoring.
