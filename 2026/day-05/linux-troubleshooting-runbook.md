# Linux Troubleshooting Runbook – Nginx

## Target Service
nginx web server

---

## Environment
**Evidence**
Linux kernel: 6.14.0-1018-aws  
OS: Ubuntu 24.04.3 LTS

**Observation**  
The server is running Ubuntu 24.04 LTS on an AWS instance. Environment looks standard and stable.

---

## CPU & Memory Snapshot
**Evidence**
Load average: 0.00  
CPU idle: 100%  
Memory: 914Mi total, 376Mi used, 220Mi free  
Swap: 0

**Observation**  
System is mostly idle. CPU usage is near zero and memory usage is low, so resource exhaustion is not causing any slowdown.

---

## Disk & IO Snapshot
**Evidence**
Root disk usage: 31% used (2.1G / 6.8G)  
vmstat shows no IO wait

**Observation**  
Plenty of disk space available and no disk activity bottleneck detected.

---

## Network Snapshot
**Evidence**
Port 80 LISTENING  
Ping successful (0% packet loss)

**Observation**  
The server is reachable over the network and nginx port is open and accepting connections.

---

## Logs Reviewed
**Evidence**
nginx.service started successfully  
No recent error entries

**Observation**  
No crashes or warning signs found in nginx logs.

---

## Quick Findings
- nginx service is running normally
- server resources are healthy
- disk and network are functioning properly
- logs do not show any failures

Overall the system appears healthy and nginx is operating normally.

---

## If this worsens
1. Restart nginx and monitor resource usage
2. Watch nginx access and error logs in real time
3. Capture process behaviour using `strace` on nginx worker
