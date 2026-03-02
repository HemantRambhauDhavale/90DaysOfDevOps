# Day 18 – Shell Scripting: Functions & Intermediate Concepts

---

## Task 1 – Basic Functions

### functions.sh

#!/bin/bash

greet() {
  echo "Hello, $1!"
}

add() {
  SUM=$(( $1 + $2 ))
  echo "Sum is: $SUM"
}

greet "Hemant"
add 5 10

---

## Task 2 – Disk & Memory Check

### disk_check.sh

#!/bin/bash

check_disk() {
  echo "Disk Usage:"
  df -h /
}

check_memory() {
  echo "Memory Usage:"
  free -h
}

check_disk
check_memory

---

## Task 3 – Strict Mode

### strict_demo.sh

#!/bin/bash
set -euo pipefail

echo "Strict mode enabled"

# Uncomment to test:
# echo $UNDEFINED_VAR
# false
# ls non_existing_file | grep test

Explanation:

set -e → Exit if any command fails  
set -u → Exit if using undefined variable  
set -o pipefail → Pipeline fails if any command fails  

---

## Task 4 – Local Variables

### local_demo.sh

#!/bin/bash

test_local() {
  local VAR="Inside Function"
  echo $VAR
}

test_global() {
  VAR2="Global Variable"
}

test_local
echo $VAR   # Empty outside function

test_global
echo $VAR2  # Accessible globally

---

## Task 5 – System Info Reporter

### system_info.sh

#!/bin/bash
set -euo pipefail

print_hostname() {
  echo "Hostname & OS:"
  hostname
  uname -a
}

print_uptime() {
  echo "Uptime:"
  uptime
}

print_disk() {
  echo "Disk Usage:"
  df -h | sort -k5 -r | head -5
}

print_memory() {
  echo "Memory Usage:"
  free -h
}

print_cpu() {
  echo "Top 5 CPU Processes:"
  ps aux --sort=-%cpu | head -6
}

main() {
  print_hostname
  print_uptime
  print_disk
  print_memory
  print_cpu
}

main

---

## What I Learned

1. Functions make scripts modular and reusable.
2. Strict mode prevents hidden failures.
3. Local variables avoid unwanted side effects.
