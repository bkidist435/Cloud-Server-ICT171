# Cloud-Server-ICT171
A personal blog website called Grace and Light, hosted on an Amazon EC2 Ubuntu server using Apache2 and WordPress, with a linked domain name and SSL security. Includes documentation, code, and video explainer.

Grace and Light – Cloud Server Project

Student Info
- Name: Kidist Begashaw
- Student Number: 34363736
- Unit: ICT171

Project Overview
This project is a simple cloud-hosted blog website titled **Grace and Light**, built as part of the ICT171 Cloud Server assessment. It is designed to serve as a personal spiritual and creative space with long-term potential to expand into a full website with resources and digital downloads.

The website is hosted on an Amazon EC2 Ubuntu server, running **Apache2**, with WordPress installed and linked to a real domain.

Server Setup Summary
- Cloud Provider: Amazon Web Services (AWS)
- Instance Type: EC2 (Ubuntu 20.04 LTS)
- Web Server: Apache2
- CMS: WordPress
- Domain Name: graceandlight.space
- SSL: Secured with Let’s Encrypt SSL certificate (HTTPS enabled)
- Scripting:
  - Bash used to automate LAMP installation
  - Cron job set up for weekly file backups

Technologies Used
- Amazon EC2 (AWS)
- Ubuntu Linux
- Apache2
- MySQL 
- PHP
- WordPress
- Certbot (Let’s Encrypt)
- Namecheap (DNS domain registrar)
- Git + GitHub for version control and documentation

Project Structure

/grace-and-light-cloud-project
│
├── README.md # This documentation
├── website-files/ # WordPress core files or HTML content
├── screenshots/ # Terminal, DNS, and browser screenshots
│ ├── apache-running.png
│ ├── wordpress-setup.png
│ └── dns-records.png
└── video-link.txt - Link to my video explanation

---

Project Steps

1. Launching the EC2 Instance

To begin, I launched a **free-tier EC2 instance** using Ubuntu 20.04 on Amazon Web Services.

Steps:

```bash
# Launch EC2 via AWS Console
# Select: Ubuntu Server 20.04 LTS (t2.micro)
# Configure Security Group with these inbound rules:
# - SSH (port 22)
# - HTTP (port 80)
# - HTTPS (port 443)
```

After launching, I downloaded my `.pem` key and connected using:

```bash
chmod 400 my-key.pem
ssh -i my-key.pem ubuntu@<your-ec2-ip>

<img width="1440" alt="ec2 running" src="https://github.com/user-attachments/assets/be1d7dc4-f932-49f3-a9e3-64a537c643c0" />

```

---

2. Installing Apache, MySQL, and PHP (LAMP Stack)
   

How I Installed the LAMP Stack on Ubuntu (EC2)

To host my Grace and Light website, I used a **LAMP stack** on an **Ubuntu EC2 instance**. Here's exactly how I set it up:

1. Update the Package Index

```bash
sudo apt update
```

2. Install Apache Web Server

Apache is the web server that handles HTTP requests.

```bash
sudo apt install apache2
```

To check if it's working, visit your EC2 Public IP (e.g., `http://54.xx.xx.xx`) — you should see the Apache default page.

![apache default page](https://github.com/user-attachments/assets/515a5311-c311-47d9-bdde-606ac452a76b)


3. Adjust the Firewall

I allowed HTTP traffic on the server:

```bash
sudo ufw allow in "Apache Full"
```

4. Install MySQL

MySQL is the database where WordPress stores its data.

```bash
sudo apt install mysql-server
```

Then run the secure installation script:

```bash
sudo mysql_secure_installation
```

I followed the prompts to set a root password and secure the database setup.

5. Install PHP

PHP is the language WordPress is built with.

```bash
sudo apt install php libapache2-mod-php php-mysql
```

Check version to confirm install:

```bash
php -v
```

### 6. Test Apache with PHP

I created a PHP test file:

```bash
sudo nano /var/www/html/info.php
```

Add this inside:

```php
<?php
phpinfo();
?>
```

Then visit: `http://your-ec2-ip/info.php` to confirm PHP is working.

---

Let me know when you're ready to add the next part (like WordPress install, domain connection, or SSL).

---

3. Setting Up WordPress

After setting up the LAMP stack, I installed WordPress manually to power my **Grace and Light** site. Here's how I did it:

1. Create a MySQL Database and User for WordPress

I logged into MySQL:

```bash
sudo mysql
```

Then I created the database and user:

```sql
CREATE DATABASE graceandlight_db DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
CREATE USER 'graceuser'@'localhost' IDENTIFIED BY 'your-strong-password';
GRANT ALL PRIVILEGES ON graceandlight_db.* TO 'graceuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

2. Download and Configure WordPress

I moved to the `/tmp` directory and downloaded WordPress:

```bash
cd /tmp
curl -O https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
```

Then I copied it to the web root:

```bash
sudo cp -a wordpress/. /var/www/html
```

3. Set the Correct Permissions

I gave ownership of the WordPress files to the Apache user:

```bash
sudo chown -R www-data:www-data /var/www/html
sudo find /var/www/html/ -type d -exec chmod 755 {} \;
sudo find /var/www/html/ -type f -exec chmod 644 {} \;
```

4. Configure Apache for WordPress

I created a config file for my domain:

```bash
sudo nano /etc/apache2/sites-available/graceandlight.space.conf
```

Then added:

```apache
<VirtualHost *:80>
    ServerName graceandlight.space
    ServerAlias www.graceandlight.space
    DocumentRoot /var/www/html

    <Directory /var/www/html>
        AllowOverride All
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Enabled the site and rewrite module:

```bash
sudo a2ensite graceandlight.space.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
```

### 5. Complete WordPress Setup via Browser

I visited `http://graceandlight.space` in the browser.

![wordpress set up screen](https://github.com/user-attachments/assets/cb53338e-d4ed-403f-a693-84d1c6632dc4)

This opened the **WordPress setup wizard**, where I entered:

* Site title: `Grace and Light`
* Username: (admin user I chose)
* Password: (secure password)
* Email: (mine)

Done! WordPress was installed and my site was live.

---

5. Connecting a Domain Name

I purchased the domain graceandlight.space via Namecheap.

To point it to my server:

* I added an **A Record** in Namecheap’s DNS settings:

  * Host: `@`
  * Value: `<my-ec2-public-ip>`
  * TTL: Automatic

Optional:

* Added a **CNAME**:

  * Host: `www`
  * Value: `graceandlight.space`
    
 <img width="1440" alt="dns set up on namecheap" src="https://github.com/user-attachments/assets/7b863bdd-d215-4aaf-acaa-fec500b03777" />


After DNS propagated, I could visit my site at:
[https://graceandlight.space](https://graceandlight.space)**

---

6. Installing SSL (HTTPS)

To secure the website with HTTPS, I used **Certbot**:

Here's the **Let’s Encrypt SSL setup section** from DigitalOcean, rewritten and personalized for your GitHub documentation based on your **Grace and Light** website hosted on an EC2 Ubuntu server.

---

## 🔐 How I Secured My Site with Let’s Encrypt SSL (Grace and Light)

---

1. Install Certbot and the Apache Plugin

```bash
sudo apt update
sudo apt install certbot python3-certbot-apache
```

---

2. Get and Install the SSL Certificate

I ran this command to automatically configure SSL for my Apache server and domain:

```bash
sudo certbot --apache
```

I was prompted to:

* Choose the domain to secure (I selected `graceandlight.space`)
* Redirect HTTP to HTTPS — I chose the option to force **secure HTTPS** (recommended)

---

3. Verify the SSL Certificate

I visited:
`https://graceandlight.space`
✅ It loaded securely with the padlock icon!

You can also test it via SSL Labs:
[https://www.ssllabs.com/ssltest/](https://www.ssllabs.com/ssltest/)

---

4. Auto-Renewal Check

Let’s Encrypt certificates expire every 90 days. To make sure mine will auto-renew, I tested the renewal process:

```bash
sudo certbot renew --dry-run
```

Now my website is fully secure with a green lock! 

<img width="1440" alt="final live website" src="https://github.com/user-attachments/assets/df720493-920c-474d-8592-58de478e3068" />



