# Day 10 – File Permissions & File Operations Challenge

## Files Created

- devops.txt
- notes.txt
- script.sh
- project/ (directory)

---

## Commands Used

### Create Files
touch devops.txt

echo "This is my notes file" > notes.txt

vim script.sh
# Added content:
# echo "Hello DevOps"

---

### Check Permissions
ls -l devops.txt notes.txt script.sh

---

### Read Files
cat notes.txt
vim -R script.sh
head -n 5 /etc/passwd
tail -n 5 /etc/passwd

---

### Modify Permissions

# Make script executable
chmod +x script.sh

# Make devops.txt read-only
chmod a-w devops.txt

# Set notes.txt to 640
chmod 640 notes.txt

# Create directory with 755 permission
mkdir project
chmod 755 project

---

### Test Execution
./script.sh

# Try writing to read-only file
echo "test" >> devops.txt

---

## Permission Changes (Before → After)

script.sh
-rw-r--r-- → -rwxr-xr-x

devops.txt
-rw-r--r-- → -r--r--r--

notes.txt
-rw-r--r-- → -rw-r-----

project/
drwxr-xr-x

---

## What I Learned

- Meaning of rwxrwxrwx format
- Difference between 755 and 640
- Execute permission is required to run scripts
- Linux security is controlled by permissions
