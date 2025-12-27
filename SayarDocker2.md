# ✅ Sayar Day-2: Push/Pull Docker Image & Run on VPS

🔹 Goal
Build Docker image locally
Push image to Docker Hub
Pull image on DigitalOcean VPS
Run container and access via browser

🔹 Step 1: Build Docker Image Locally
docker build -t mc-rrd-web .

Explanation:
docker build → build image from Dockerfile
-t mc-rrd-web → tag image with name mc-rrd-web
. → current directory is Dockerfile location

🔹 Step 2: Create Docker Hub Account
Visit Docker Hub
Create a free account
Note your username (e.g., koko)

🔹 Step 3: Login to Docker Hub
docker login

Enter Docker Hub username & password
Authenticates local Docker to push images

🔹 Step 4: Tag Image for Docker Hub
docker tag mc-rrd-web koko/mc-rrd-web

Explanation:
Docker Hub requires username/repository format
koko → Docker Hub username
mc-rrd-web → repository name

🔹 Step 5: Push Image to Docker Hub
docker push koko/mc-rrd-web
✔ Image is now available publicly (or privately) on Docker Hub

🔹 Step 6: Prepare DigitalOcean VPS
Create Ubuntu VPS
SSH into VPS:
ssh root@<VPS_IP>

Update packages:
sudo apt update

Install Docker (follow Ubuntu guide):

sudo apt install docker.io -y
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
docker --version
systemctl status docker

✔ Ensures Docker runs as non-root user

🔹 Step 7: Pull Image from Docker Hub
docker pull koko/mc-rrd-web

Problem:
no matching manifest for linux/amd64 in the manifest list entries
Occurs because image was not built for the host platform (linux/amd64)

🔹 Step 8: Build Multi-Platform Image Locally
docker buildx build \
  --platform linux/amd64,linux/arm64 \
  -t koko/mc-rrd-web:1.0.0 \
  -t koko/mc-rrd-web:latest \
  --push .

Explanation:
--platform → build for multiple architectures
--push → directly push to Docker Hub
Tags: 1.0.0 and latest

🔹 Step 9: Pull Image on VPS
docker pull koko/mc-rrd-web
✔ Now works without platform mismatch

🔹 Step 10: Run Container on VPS
docker run -d -p 8000:80 koko/mc-rrd-web

Explanation:
-d → run in detached mode
-p 8000:80 → map container port 80 → host port 8000
koko/mc-rrd-web → image to run

🔹 Step 11: Access Application via Browser
Open:
http://<VPS_IP>:8000
✔ Nginx / application welcome page should load

# 📝 Summary
Built and pushed Docker image to Docker Hub
Solved platform mismatch using docker buildx
Pulled and ran image on DigitalOcean VPS
Accessed container via mapped port in browser

# 🎯 DevOps Tips
Always tag images with version (:1.0.0) + latest
Multi-architecture build ensures cross-platform compatibility
docker buildx → essential for ARM + AMD64 systems
Map container ports carefully to avoid conflicts on VPS