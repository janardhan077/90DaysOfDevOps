# Day 13 – Nginx & Networking Basics 🚀

## What I Did

- Installed and started Nginx
- Checked service status using systemctl
- Verified open ports using ss -tulpn
- Tested server response using curl

---

## Commands Practiced

### Check Service Status
systemctl status nginx

### Check Open Ports
ss -tulpn

### Test Local Server
curl -I http://localhost

### Test Public IP
curl -I http://<your-public-ip>

---

## Key Learnings

- Understood difference between incorrect and correct systemctl usage
- Verified that port 80 was listening
- Confirmed server response returned HTTP 200 OK
- Learned how to test production server from terminal

---

## Output Example

HTTP/1.1 200 OK
Server: nginx/1.24.0 (Ubuntu)

---

Step by step becoming comfortable with real server environments 💻🔥
