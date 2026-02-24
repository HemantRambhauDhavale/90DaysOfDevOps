# Day 12 – Revision & Reflection (Days 01–11)

---

## Quick Revision Checklist

### Mindset & Plan
- Reviewed Day 01 learning plan
- Goals still aligned
- Will focus more on faster troubleshooting

---

### Processes & Services (Re-ran Commands)

- ps aux
- systemctl status ssh
- journalctl -u ssh -n 20

Observation:
Service active, logs clean, process stable.

---

### File Skills Practice

- echo "test" >> sample.txt
- chmod 755 script.sh
- chown tokyo sample.txt
- ls -l
- mkdir test-folder

Observation:
Permissions and ownership now make more sense.

---

### Cheat Sheet Refresh

Top 5 Commands I’d Use in Incident:
1. top
2. ps aux --sort=-%cpu
3. systemctl status
4. journalctl -u service -n 50
5. df -h

---

## Mini Self-Check

### 1. 3 Commands That Save Me Time
- systemctl status → Quick service health
- journalctl -u → Log debugging
- ls -l → Permission check

### 2. How to Check Service Health?
- systemctl status <service>
- journalctl -u <service> -n 50
- ps aux | grep <service>

### 3. How to Safely Change Ownership & Permissions?
Example:
sudo chown professor:planners file.txt
chmod 640 file.txt

Verify using:
ls -l file.txt

### 4. Focus for Next 3 Days
- Improve speed with troubleshooting
- Practice real scenarios again
- Get comfortable with log reading

---

## Key Takeaways

- Fundamentals are becoming stronger.
- Repetition improves confidence.
- Troubleshooting is about process, not panic.
