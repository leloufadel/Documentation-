

## 🚀 Deployment Guide for Next.js CRM Dashboard (App Router + MySQL)

This guide explains how to deploy a production-ready **Next.js CRM Dashboard** using a **Linux server** (Ubuntu), **MySQL**, **PM2**, and **NGINX**.

---

### 📁 Prerequisites

* A fresh Linux server (Ubuntu 20.04+)
* Root access via SSH
* A private GitHub repository with your Next.js App
* MySQL database structure ready (`.sql` file)

---

### 1. 🔑 Access Your Server

SSH into your server:

```bash
ssh root@YOUR_SERVER_IP
```

---

### 2. 📦 Update Packages & Install Node.js

```bash
sudo apt update && sudo apt upgrade -y
```

Install Node.js via `nvm`:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
source ~/.bashrc
nvm install --lts
```

Verify installation:

```bash
node -v
npm -v
```

---

### 3. 🌐 Install & Configure NGINX

Install NGINX:

```bash
sudo apt install nginx -y
sudo systemctl start nginx
```

#### Configure NGINX as a reverse proxy:

Create or edit config:

```bash
sudo nano /etc/nginx/sites-available/crm
```

Paste the following configuration:

```nginx
server {
    listen 80;
    server_name YOUR_SERVER_IP;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Save and close (`CTRL+O`, `ENTER`, then `CTRL+X`).

Link and reload:

```bash
sudo ln -s /etc/nginx/sites-available/crm /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```

---

### 4. 📁 Clone Project & Install Dependencies

```bash
cd ~
git clone git@github.com:your-username/your-private-repo.git
cd your-private-repo
npm install
```

---

### 5. ⚙️ Run with PM2 (Process Manager)

Install PM2 globally:

```bash
npm install -g pm2
```

Start your app with PM2:

```bash
pm2 start npm --name="dashboard-crm" -- start
pm2 startup
pm2 save
```

To restart later:

```bash
pm2 restart dashboard-crm
```

---

### 6. 🛢️ Install MySQL Server

```bash
sudo apt update
sudo apt install mysql-server -y
```

---

### 7. 🧱 Create Database & User

Login to MySQL:

```bash
sudo mysql
```

Run:

```sql
CREATE DATABASE crmdemo;
CREATE USER 'crmadmin'@'localhost' IDENTIFIED BY 'StrongPasswordHere';
GRANT ALL PRIVILEGES ON crmpdemo.* TO 'crmadmin'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
---

### 8. 🔄 Import Database Structure

Upload SQL file from your local machine:

```bash
scp "C:/path/to/crm_fixed.sql" root@YOUR_SERVER_IP:/root/
```

Then import it:

```bash
sudo mysql -u root -p
USE crmprod;
SOURCE /root/crm_fixed.sql;
```
---

### 9. 🔐 Configure Database Connection

In your `.env.production`:

```env
NODE_ENV=production
DATABASE_URL=mysql://crmadmin:StrongPasswordHere@localhost:3306/crmprod
NEXTAUTH_SECRET=your_secret_key
NEXTAUTH_URL=http://YOUR_SERVER_IP
```

---

### 10. 🔁 Restart Services

After making changes:

```bash
pm2 restart dashboard-crm
sudo systemctl restart nginx
```

---
# Handling Vulnerabilities (npm / packages)

npm audit
Apply safe automatic fixes:

bash

``` npm audit fix```
Apply forced fixes (may cause breaking changes):

```npm audit fix --force```
Use this only after testing.

Check outdated packages:

npm outdated
Update packages manually if needed:


npm install <package>@latest

Logs Management 

Using PM2
pm2 logs project-name

 pm2 logs project-name --err

## ✅ Done!
we certbot

Your CRM dashboard should now be live at `http://YOUR_SERVER_IP`.

# Deployment Notes for Next.js + Prisma on DigitalOcean

## 1. Install Node.js LTS
```bash
nvm install --lts

Installs the latest Long Term Support (LTS) version of Node.js.

2. Install Yarn Globally
npm install -g yarn
Installs Yarn as the package manager for the app.

3. Update npm
npm install -g npm@<VERSION>
Prisma Setup & Usage

The Next.js app connects to a PostgreSQL database using Prisma.

After installing dependencies, generate the Prisma client:

yarn prisma generate
Deploy migrations:

DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/DBNAME" yarn prisma migrate deploy



