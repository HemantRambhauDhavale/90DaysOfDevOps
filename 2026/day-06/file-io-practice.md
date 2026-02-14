# Day 06 – Linux File I/O Practice

## File Created
notes.txt

---

## Commands Used

touch notes.txt  
- Created an empty file.

echo "Learning DevOps Day 06" > notes.txt  
- Added first line (overwrite mode).

echo "Practicing file read and write" >> notes.txt  
- Appended second line.

echo "Linux fundamentals are important" | tee -a notes.txt  
- Appended third line and displayed it.

cat notes.txt  
- Displayed full file content.

head -n 2 notes.txt  
- Showed first 2 lines.

tail -n 2 notes.txt  
- Showed last 2 lines.

---

## What I Observed

- `>` overwrites file content.
- `>>` appends new content.
- `tee -a` appends and displays output.
- Reading files using cat, head, and tail is very useful for logs.
