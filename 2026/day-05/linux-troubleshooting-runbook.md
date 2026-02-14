# Day 05 – Linux Troubleshooting Runbook

## Target Service
ssh

---

## Environment Basics

Command:
uname -a  
Observation:
Confirmed kernel version and system architecture.

Command:
cat /etc/os-release  
Observation:
Verified OS version (Ubuntu).

---

## Snapshot: CPU & Memory

Command:
top  
Observation:
CPU usage normal. No abnormal spikes.

Command:
free -h  
Observation:
Memory usage within limits. No swap pressure.

Command:
ps aux | grep ssh  
Observation:
ssh process running with stable memory usage.

---

## Snapshot: Disk & IO

Command:
df -h  
Observation:
Root partition not full.

Command:
du -sh /var/log  
Observation:
Log directory size normal.

---

## Snapshot: Network

Command:
ss -tulpn  
Observation:
ssh listening on port 22.

Command:
ping google.com  
Observation:
Network connectivity working.

---

## Logs Reviewed

Command:
journalctl -u ssh -n 50  
Observation:
No recent errors found.

Command:
journalctl -xe  
Observation:
No critical system-level issues.

---

## Quick Findings
- ssh service active and stable
- No CPU or memory spike
- Logs clean
- Disk usage normal

---

## If This Worsens (Next Steps)
1. Restart ssh service: systemctl restart ssh
2. Increase log review range (journalctl -u ssh -n 200)
3. Check firewall or port blocking issues
