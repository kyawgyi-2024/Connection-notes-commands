# ✅ Sayar Docker: Running Nginx Web Server in a Container

🔹 Goal
Install Docker
Run Ubuntu container
Install Nginx inside container
Access Nginx via mapped port

🔹 Step 1: Check Docker Version
docker --version
✔ Confirms Docker is installed and running

🔹 Step 2: Pull Ubuntu Image
docker pull ubuntu

Explanation:
Downloads official Ubuntu image from Docker Hub
Used as base image for container

🔹 Step 3: Run Container with Port Mapping
docker run -it -p 9000:90 --name=vps ubuntu /bin/bash

Explanation:
Option	Purpose
-it	Interactive terminal
-p 9000:90	Maps container port 90 → host port 9000
--name=vps	Name of container
ubuntu	Image to run
/bin/bash	Start Bash shell inside container
✔ You are now inside the container as root

🔹 Step 4: Update Packages Inside Container
apt update

🔹 Step 5: Install Nginx
apt install nginx -y
✔ Installs Nginx web server in the container

🔹 Step 6: Install systemctl (if not available)
apt install systemd -y

Some minimal Ubuntu images may not have systemctl

🔹 Step 7: Start & Check Nginx Service
systemctl start nginx
systemctl status nginx
✔ Ensure Nginx is running inside the container

🔹 Step 8: Install curl (if needed)
apt install curl -y

🔹 Step 9: Test Nginx Inside Container
curl localhost:90
✔ Should display Nginx default HTML

🔹 Step 10: Access Nginx from Host Browser
Open browser and visit:
http://localhost:9000
✔ Displays Nginx Welcome Page
Port 9000 on host maps to port 90 inside container

🔹 Step 11: Navigate to Web Root Inside Container
cd /var/www/html

Explanation:
Default directory for Nginx HTML files
You can modify index.html here to serve your content

# 📝 Summary
Pulled Ubuntu Docker image
Ran container with interactive shell
Installed & started Nginx
Accessed Nginx via mapped host port
Verified web content inside /var/www/html

# 🎯 DevOps / Docker Tips
Use docker ps → check running containers
Use docker exec -it <container_name> bash → re-enter container
Host port mapping: <host_port>:<container_port>
Use volumes to persist /var/www/html across container restarts
