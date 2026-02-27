# Day 15 – Networking Concepts

## Challenge Tasks

---

### Task 1: DNS – How Names Become IPs

**What happens when you type `google.com` in a browser?**  
When you type `google.com`, your system first checks its local cache. If not found, it queries the DNS server to resolve the domain name to an IP address. The browser then connects to that IP to fetch the website.

**DNS Record Types:**  
- **A** – Maps a domain to an IPv4 address.  
- **AAAA** – Maps a domain to an IPv6 address.  
- **CNAME** – Alias for another domain name.  
- **MX** – Mail exchange server for email delivery.  
- **NS** – Authoritative name server for the domain.

**Command:**  
```bash
dig google.comKey Learnings

DNS is critical for translating human-friendly domain names into machine-readable IPs.

Understanding CIDR and subnetting helps optimize network design and IP allocation.

Ports let multiple services coexist on the same device, each reachable individually.Common Ports:

Port	Service
22	SSH
80	HTTP
443	HTTPS
53	DNS
3306	MySQL
6379	Redis
27017	MongoDB
