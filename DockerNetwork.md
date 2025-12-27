# ✅ Sayar Docker: Network Basics & Container Communication

🔹 Goal
Understand Docker default networks
Create custom networks
Enable communication between containers
Test network connectivity and Nginx setup

🔹 Step 1: Check Docker Networks
docker network ls

Explanation:
Lists all Docker networks

Default networks:
bridge → default for standalone containers
host → uses host network
none → no networking

🔹 Step 2: Pull Required Images
docker pull ubuntu
docker pull alpine

✔ Ubuntu → heavier image
✔ Alpine → lightweight Linux image

🔹 Step 3: Run Containers in Separate Terminals
Terminal 1 (Ubuntu):
docker run -it --rm --name=c1 ubuntu /bin/bash

Terminal 2 (Alpine):
docker run -it --rm --name=c2 alpine sh

Explanation:
--rm → removes container when exited
--name → container name for easy reference

🔹 Step 4: Install Networking Tools Inside Containers
Terminal 1 (c1):
apt update
apt install net-tools iputils-ping curl -y


Terminal 2 (c2):
apk add curl

🔹 Step 5: Check IP Address
ifconfig          # eth0 shows container IP
ip add            # alternative to check IP

🔹 Step 6: Test Connectivity Between Containers
ping <other-container-ip>
✔ By default, containers on bridge network cannot communicate unless explicitly connected

🔹 Step 7: Create a Custom Docker Network
docker network create mynet
docker network ls
✔ Shows mynet along with default networks

🔹 Step 8: Run Containers on Custom Network
docker run -it --rm --name=c3 --network=mynet alpine sh

Now containers on mynet can ping each other by IP or name
ip add           # check container IP
ping <c1-ip>     
ping <c2-ip>

🔹 Step 9: Setup Nginx in Ubuntu Container (c1)
apt install nginx -y
service nginx status
service nginx start
curl localhost      # check local Nginx page

🔹 Step 10: Test Nginx Access from Alpine Container (c2)
curl <c1-ip>
From Alpine, you can access Ubuntu container’s Nginx web page
If curl not installed:
apk add curl
curl <c1-ip>

🔹 Step 11: Modify Web Content in c1
apt install vim -y
cd /var/www/html
vim index.html

Inside Vim:
<h1>hello docker</h1>
Save and exit: esc :wq

🔹 Step 12: Re-test from c2
curl <c1-ip>
✔ Should display “hello docker”

# 📝 Summary
Default bridge network isolates containers
Custom network allows container-to-container communication
Nginx installed on Ubuntu container
Web content served and accessible from another container
Verified IP addresses & connectivity using ping and curl

# 🎯 DevOps / Docker Network Tips
Use custom bridge networks for microservices
Use container names instead of IPs for easier communication
docker network inspect mynet → see container connections
Map host ports if you want external access:
docker run -d -p 8080:80 --network=mynet --name=c1 ubuntu