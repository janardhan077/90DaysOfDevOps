# Day 07 — Linux File System & Troubleshooting (Human Notes)

## 1) File System Hierarchy (Where things live in Linux)

I explored the system from `/` and moved around manually to understand what each folder actually does.

### `/`

Root of the system. Everything starts here.
When you don’t know where you are — you can always come back here.

---

### `/home`

Users live here.
I checked:

```
cd /home
ls
jana  janardhan
```

Mistake I did:

```
cd janna   -> No such file
```

Learned: Linux spelling must be exact.

---

### `/etc`

System configuration files.
Basically the *brain* of Linux.
Contains settings for network, users, services, ssh, cron etc.

I first typed wrong:

```
cd /ect   -> error
```

Then correct:

```
cd /etc
ls
```

Many service configs live here (nginx, ssh, network, passwd).

---

### `/var`

Changing data (logs, cache, runtime files)
Most important for DevOps debugging.

```
cd /var/log
ls
```

I saw logs like:

* syslog
* auth.log
* dpkg.log
* nginx/

---

### `/var/log`

This is the **debugging gold mine**.
Whenever something fails → check logs here first.

---

### `/bin` & `/usr`

Programs live here (commands like ls, cat, mkdir, etc.)
System binaries are stored here.

---

### `/proc`

Live system info (CPU, memory, processes)
Not real files — kernel information.

---

### `/tmp`

Temporary files (deleted after reboot usually)

---

### `/root`

Home directory of root user (not same as `/` root folder)

---

## 2) Scenario Practice (Real troubleshooting)

### Scenario 1 — Directory not opening

Problem:

```
cd janna
No such file or directory
```

Cause: Wrong name
Fix:

```
ls
cd jana
```

Lesson: Always list before cd.

---

### Scenario 2 — Wrong path

Problem:

```
cd /ect
```

Fix:

```
cd /etc
```

Lesson: Linux is strict with paths.

---

### Scenario 3 — Reading a directory like a file

Problem:

```
cat etc
Is a directory
```

Fix:

```
cd etc
ls
```

Lesson: cat is only for files, not folders.

---

### Scenario 4 — Reading compressed logs

Problem:

```
cat dpkg.log.2.gz
(gibberish output)
```

Cause: compressed file
Fix:

```
zcat dpkg.log.2.gz
```

Lesson: `.gz` files must be uncompressed while reading.

---

### Scenario 5 — Understanding system activity

From dpkg logs I learned the system records installs and upgrades automatically.
This helps track what changed if server breaks after update.

---

## Final Understanding

Linux troubleshooting flow I learned today:

1. Identify location
2. Check files using ls
3. Check configs in /etc
4. Check logs in /var/log
5. Read compressed logs properly
6. Fix step by step (never guess)

Today I understood Linux is not memorizing commands — it is knowing *where to look*.
