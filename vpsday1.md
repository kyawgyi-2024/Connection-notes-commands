## DigitalOcean Day-1: Laravel Project Hosting on VPS + Cloudflare SSL (One-Sheet)

### 🔹 Server Access (Mac + VS Code)
ssh root@<VPS_IP>                   # Login to DigitalOcean VPS as root user

**VS Code SSH Setup**
* Open VS Code → Remote Explorer → Add New SSH Host
* Config file: `~/.ssh/config`
* Click connect and enter password
✔ VPS successfully connected via VS Code terminal

### 🔹 Laravel Hosting Requirements
Laravel requires:
1. PHP
2. PHP Extensions
3. MySQL
4. Composer

### 🔹 Update System & Check Packages
apt update                          # Update package index

apt list nginx                      # Check available nginx versions
apt list php                        # Check available PHP versions
apt list nodejs                     # Check Node.js availability

apt install nginx -y                # Install Nginx web server

### 🔹 Install PHP 8.3 + Extensions (Laravel)
apt install -y php8.3 php8.3-cli php8.3-fpm php8.3-mysql php8.3-xml php8.3-mbstring php8.3-curl php8.3-zip php8.3-bcmath
# Install PHP 8.3 and required Laravel extensions

### 🔹 Install Composer (Official Method)
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

php composer-setup.php              # Install Composer locally
sudo mv composer.phar /usr/local/bin/composer   # Make composer global
composer --version                  # Verify Composer installation

### 🔹 Install & Configure MySQL (Ubuntu)
sudo apt install -y mysql-server    # Install MySQL server

sudo systemctl enable mysql         # Enable MySQL at boot
sudo systemctl start mysql          # Start MySQL service
sudo systemctl status mysql         # Verify MySQL is running

mysql -u root                       # Login to MySQL as root

CREATE USER 'koko'@'%' IDENTIFIED BY 'koko@2026';
GRANT ALL PRIVILEGES ON *.* TO 'koko'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;

mysql -u koko -p                    # Login as application DB user
CREATE DATABASE koko_db;             -- Create Laravel database
EXIT;

### 🔹 Deploy Laravel Project
cd /var/www
mkdir api.ygnsh.com                 # Create project directory
cd api.ygnsh.com

git clone <PROJECT_URL> .           # Clone Laravel project

cp .env.example .env                # Create environment file

composer install                    # Install Laravel dependencies

php artisan key:generate            # Generate application key

php artisan migrate --seed          # Run migrations and seed database

### 🔹 Verify Database
mysql -u root
SHOW DATABASES;
USE koko_db;
SHOW TABLES;
SELECT * FROM products;
✔ Database verified (can also recheck using TablePlus)

### 🔹 Laravel Configuration Updates
```env
APP_URL=https://api.ygnsh.com        # Set application domain
APP_DEBUG=false                     # Disable debug for production
SESSION_DRIVER=file                 # Use file-based sessions

Laravel Request Flow:
`public/index.php → application`

### 🔹 Cloudflare SSL (Origin Certificate)
* Cloudflare → SSL/TLS → Origin Certificate
* Copy **Origin Certificate** and **Private Key**

mkdir -p /etc/ssl/ygnsh

code /etc/ssl/ygnsh/origin.crt       # Paste origin certificate
code /etc/ssl/ygnsh/origin.key       # Paste private key

### 🔹 Nginx Virtual Host Configuration
code /etc/nginx/sites-available/api.ygnsh.com

(Paste Nginx server block configuration)
============================================================================================================
server {
    listen 80;
    server_name example.com www.example.com;

    root /var/www/example.com/public;
    index index.php index.html;

    # Redirect all HTTP to HTTPS (optional)
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name example.com www.example.com;

    # SSL (Let's Encrypt or manual)
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    root /var/www/example.com/public;
    index index.php index.html;

    # Main Laravel handling
    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    # PHP-FPM handling
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.2-fpm.sock; # adjust version if needed
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    # Security: deny access to sensitive files
    location ~ /\.(?!well-known).* {
        deny all;
    }

    # Optional: increase upload size (e.g., for file uploads)
    client_max_body_size 50M;
}

===============================================================================================================
# Enable the Site

ln -s /etc/nginx/sites-available/api.ygnsh.com /etc/nginx/sites-enabled/

chmod -R 777 storage/                # Temporary permission fix (Dev only)

systemctl restart nginx              # Restart Nginx

### 🔹 Final Verification
curl https://api.ygnsh.com/api/v1     # Test Laravel API endpoint

✔ Laravel application successfully hosted on DigitalOcean VPS with Cloudflare SSL
---

### 📝 Summary
* VPS connected via SSH & VS Code
* Nginx, PHP 8.3, Composer, MySQL installed
* Laravel project deployed and configured
* Database connected and verified
* Cloudflare Origin SSL configured
* Secure HTTPS Laravel API live

✔ Production-ready DevOps deployment guide
================================================================================================================