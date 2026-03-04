#  Bash Scripting Cheat Sheet

*(Quick Revision — Practical & Job-Ready)*

---

## 🔹 1. Basics

### Shebang

```bash
#!/bin/bash
```

Tells system to run script using Bash.

### Run a Script

```bash
chmod +x script.sh
./script.sh
bash script.sh
```

### Comments

```bash
# Single line comment
echo "Hello"  # Inline comment
```

### Variables

```bash
NAME="DevOps"
echo $NAME
echo "$NAME"   # Safe (recommended)
echo '$NAME'   # Literal output
```

### User Input

```bash
read -p "Enter name: " NAME
echo "Hello $NAME"
```

### Arguments

```bash
$0  # script name
$1  # first argument
$#  # number of args
$@  # all args
$?  # last exit status
```

---

## 🔹 2. Conditionals & Operators

### String

```bash
[ "$a" = "$b" ]
[ "$a" != "$b" ]
[ -z "$a" ]
[ -n "$a" ]
```

### Integer

```bash
[ "$a" -eq 5 ]
[ "$a" -ne 5 ]
[ "$a" -lt 5 ]
[ "$a" -gt 5 ]
[ "$a" -le 5 ]
[ "$a" -ge 5 ]
```

### File Tests

```bash
[ -f file ]  # file
[ -d dir ]   # directory
[ -e file ]  # exists
[ -r file ]  # readable
[ -w file ]  # writable
[ -x file ]  # executable
[ -s file ]  # not empty
```

### If / Else

```bash
if [ -f file ]; then
  echo "Exists"
elif [ -d file ]; then
  echo "Directory"
else
  echo "Not Found"
fi
```

### Logical

```bash
[ -f file ] && echo "Yes"
[ -f file ] || echo "No"
[ ! -f file ]
```

### Case

```bash
case $1 in
  start) echo "Starting" ;;
  stop)  echo "Stopping" ;;
  *)     echo "Invalid" ;;
esac
```

---

## 🔹 3. Loops

### For

```bash
for i in 1 2 3; do
  echo $i
done
```

### C-Style

```bash
for ((i=0;i<5;i++)); do
  echo $i
done
```

### While

```bash
while read line; do
  echo $line
done < file.txt
```

### Until

```bash
until [ $i -gt 5 ]; do
  echo $i
  ((i++))
done
```

### Loop Files

```bash
for file in *.log; do
  echo $file
done
```

---

## 🔹 4. Functions

### Define & Call

```bash
greet() {
  echo "Hello"
}
greet
```

### Arguments

```bash
add() {
  echo $(($1 + $2))
}
add 5 3
```

### Return

```bash
myfunc() { return 1; }
myfunc
echo $?   # return value
```

### Local Variable

```bash
test() {
  local name="DevOps"
}
```

---

## 🔹 5. Text Processing

### Grep

```bash
grep "error" file
grep -i "error" file
grep -r "error" .
grep -c "error" file
grep -n "error" file
grep -v "info" file
grep -E "error|fail" file
```

### Awk

```bash
awk '{print $1}' file
awk -F: '{print $1}' /etc/passwd
awk '/error/ {print $2}' file
```

### Sed

```bash
sed 's/old/new/g' file
sed -i 's/foo/bar/g' file
sed '/error/d' file
```

### Cut / Sort / Uniq

```bash
cut -d: -f1 file
sort -n file
uniq -c file
```

### Tr / Wc / Head / Tail

```bash
tr 'a-z' 'A-Z'
wc -l file
head -n 10 file
tail -f log.txt
```

---

## 🔹 6. Useful One-Liners

```bash
find . -type f -mtime +7 -delete
wc -l *.log
sed -i 's/old/new/g' *.txt
systemctl is-active nginx
tail -f app.log | grep --line-buffered ERROR
```

---

## 🔹 7. Error Handling

```bash
exit 0
exit 1
echo $?

set -e
set -u
set -o pipefail
set -x
```

### Trap

```bash
trap 'echo Cleaning up' EXIT
```

---

# ✅ Best Practice

✔ Always quote variables: `"$VAR"`
✔ Use `set -euo pipefail` in production
✔ Debug with: `bash -x script.sh`
✔ Keep scripts modular and readable

---


