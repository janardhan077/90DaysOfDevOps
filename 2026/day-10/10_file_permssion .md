🚀 Day 10 Challenge – Linux File Permissions Practice
📁 Files Created

devops.txt

notes.txt

script.sh

project/ (directory)

🔐 Permission Changes
1️⃣ script.sh

Initially: -rw-rw-r--

Changed to: 400 → could not execute ❌

Changed to: 100 → execute only, but still error ❌

Final working permission: 500 → -r-x------ ✅
→ Script executed successfully.

2️⃣ notes.txt

Initially empty file

Changed to: 421 → -r---w---x
→ Could read but could not overwrite ❌

Final permission: 640 → -rw-r----- ✅
→ Owner can read/write, group can read.

3️⃣ devops.txt

Changed to: 421 → -r---w---x

Final permission: 444 → -r--r--r--
→ Read-only for everyone.

4️⃣ project Directory

Initially: drwxrwxr-x

Changed to: 755 → drwxr-xr-x
→ Standard production-style directory permission.

💻 Commands Used
mkdir devops
cd devops
touch devops.txt
cat > notes.txt
cat >> notes.txt
vim script.sh
chmod 400 script.sh
chmod 500 script.sh
chmod 421 notes.txt
chmod 444 devops.txt
chmod 640 notes.txt
mkdir project
chmod 755 project
head -n 5 /etc/passwd
tail -n 5 /etc/passwd
rm -r ]
🧠 What I Learned

1️⃣ File permissions directly control read, write, and execute behavior.

2️⃣ Execute permission (x) is mandatory for running shell scripts.
3️⃣ Directori 
<img width="1926" height="1051" alt="Screenshot from 2026-02-22 13-42-41" src="https://github.com/user-attachments/assets/63d4fd2b-7399-4b9c-a7da-3e197b2f0297" />

