LINUX BASIC TROUBLESHOOTING NOTES

---

## PROCESS MONITORING

ps
Shows running processes (snapshot view)
Example:
ps aux

top
Live monitoring of CPU and RAM usage
Press q to exit

kill <PID>
Kill a process using process ID

kill -9 <PID>
Force kill a process

NOTE:
If CPU usage suddenly becomes high:

1. Check processes → ps or top
2. Check disk usage → df -h

---

## SERVICE MANAGEMENT (systemctl)

Used to control service lifecycle

systemctl status <service>
systemctl start <service>
systemctl stop <service>
systemctl restart <service>
systemctl enable <service>
systemctl disable <service>

---

## FILE SYSTEM & PERMISSIONS

df -h
Shows disk space usage

ls -l
Shows:
permissions
owner
group
file size
last modified time

chmod
Change file permissions

chmod 755 file.sh
chmod +x file.sh

---

## READ / WRITE FILES

cat file.txt
Read file content

nano file.txt
vim file.txt
vi file.txt
Edit files

Execute scripts:
./file.sh
python file.py

---

## NETWORKING COMMANDS

ping google.com
Check connectivity

curl http://example.com
Check website response

curl -I http://example.com
Check HTTP headers

ip addr
Show IP address (Linux)

ipconfig
Show IP address (Windows)

nslookup google.com
Check DNS resolution

---

## QUICK DEBUG RULE

High CPU        → top
Process issue   → ps
Service issue   → systemctl
Disk full       → df -h
Permission      → ls -l
Website down    → curl
Network issue   → ping / nslookup
---------------------------------
