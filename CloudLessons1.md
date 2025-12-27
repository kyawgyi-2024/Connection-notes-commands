# ✅ Sayar Cloud Lessons (DigitalOcean)

🔹 Topic: VPS (Virtual Private Server) & Web Server Setup
🔹 What is a VPS?
A VPS (Virtual Private Server) is:
A virtual machine running in the cloud
Has its own IP address
Provides root access
Acts like a real server
On DigitalOcean, when you create a VPS (Droplet), you get:
Public IP address
Root user access
Password or SSH key login

🔹 Step 1: Connect to VPS
From Local Terminal
ssh root@<VPS_IP>

Explanation:
root → admin user of the server
<VPS_IP> → public IP provided by DigitalOcean
This gives you full control over the VPS

🔹 Step 2: Web Server Options
Web Server	Status
Apache	Older, heavier
Nginx	Modern, faster, lightweight
➡️ Nginx is preferred for cloud & DevOps environments.

🔹 Step 3: Update System Packages
sudo apt update

Explanation:
Refreshes package index
Ensures latest versions are installed
Always run before installing new software

🔹 Step 4: Install Nginx
sudo apt install nginx -y

Explanation:
Installs Nginx web server
-y → auto-confirm installation

🔹 Step 5: Enable Nginx at Boot
sudo systemctl enable nginx

Explanation:
Automatically starts Nginx when server reboots

🔹 Step 6: Start Nginx Service
sudo systemctl start nginx

Explanation:
Starts the web server immediately

🔹 Step 7: Check Nginx Status
sudo systemctl status nginx

Explanation:
Confirms:
Nginx is running
No startup errors
✔ Status should show active (running)

🔹 Step 8: Test Web Server via Terminal
curl http://<VPS_IP>

Explanation:
Fetches webpage from the server
If Nginx works, HTML output will appear

🔹 Step 9: Test via Browser
Open your browser and visit:
http://<VPS_IP>
✔ You should see the Nginx Welcome Page

# 📝 Summary
✔ Created DigitalOcean VPS
✔ Connected via SSH
✔ Installed & configured Nginx
✔ Verified via terminal and browser
Your VPS is now a working web server 🎉

# 🎯 DevOps / Cloud Tips
Nginx default web root:
/var/www/html

Main config file:
/etc/nginx/nginx.conf

Service management:
systemctl start|stop|restart nginx

------------------------------------------------------------------------------------------------------
If you want next lessons, I can help with:
🔐 Secure VPS (firewall + SSH keys)
🌍 Host website on domain
🔒 SSL (HTTPS) setup
🐘 PHP + Laravel on DigitalOcean
