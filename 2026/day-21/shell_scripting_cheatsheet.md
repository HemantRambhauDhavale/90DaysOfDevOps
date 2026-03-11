# Shell Scripting Cheat Sheet

## Quick Reference Table

| Topic | Syntax | Example |
|------|------|------|
| Variable | VAR="value" | NAME="DevOps" |
| Argument | $1 $2 | ./script.sh arg1 |
| If condition | if [ condition ]; then | if [ -f file ]; then |
| For loop | for i in list | for i in 1 2 3 |
| Function | name() { } | greet() { echo "Hi"; } |
| Grep | grep pattern file | grep -i "error" log.txt |
| Awk | awk '{print $1}' | awk -F: '{print $1}' /etc/passwd |
| Sed | sed 's/old/new/g' | sed -i 's/foo/bar/g' config.txt |

---

# 1. Basics

### Shebang
Defines which interpreter runs the script.

Example
#!/bin/bash

### Run Script

chmod +x script.sh  
./script.sh  
bash script.sh  

### Comments

# This is a comment

### Variables

NAME="Hemant"  
echo $NAME

Double quotes expand variables  
Single quotes print literal value

### User Input

read -p "Enter name: " NAME

### Arguments

$0 → script name  
$1 → first argument  
$# → number of arguments  
$@ → all arguments  

---

# 2. Conditionals

### String Comparison

[ "$a" = "$b" ]  
[ "$a" != "$b" ]  

### Integer Comparison

- eq equal  
- ne not equal  
- lt less than  
- gt greater than  

Example

if [ $num -gt 10 ]; then  
echo "Greater than 10"  
fi

### File Tests

-f file exists  
-d directory exists  
-r readable  
-w writable  

Example

if [ -f file.txt ]; then  
echo "File exists"  
fi

---

# 3. Loops

### For Loop

for i in 1 2 3  
do  
echo $i  
done

### While Loop

while [ $num -gt 0 ]  
do  
echo $num  
done

### Loop Files

for file in *.log  
do  
echo $file  
done

---

# 4. Functions

Define function

greet() {  
echo "Hello $1"  
}

Call function

greet Hemant

Local variables

local var="value"

---

# 5. Text Processing

grep search text  
grep -i ignore case  
grep -n show line number  

awk print columns  

awk '{print $1}'

sed replace text  

sed 's/old/new/g'

cut extract column  

cut -d ":" -f1

sort sorting  

sort -n

uniq remove duplicates  

uniq -c

wc count lines  

wc -l file.txt

head / tail  

head -n 10 file  
tail -n 10 file  

---

# 6. Useful One-Liners

Find files older than 7 days

find /var/log -mtime +7

Count lines in all logs

wc -l *.log

Replace string in multiple files

sed -i 's/old/new/g' *.txt

Check service running

systemctl status nginx

Monitor disk usage

df -h

---

# 7. Error Handling

Exit code

echo $?

Exit script

exit 1

Strict mode

set -e  
set -u  
set -o pipefail  

Debug mode

set -x

Trap example

trap "echo Script exited" EXIT
