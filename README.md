You can also access my site through here:
www.graceandlight.space

Link to my video explainer: 
https://youtu.be/XVccU4TcdJY

Cloud-Server-ICT171
A personal blog site called Grace and Light hosted on an Amazon EC2 Ubuntu server using Apache2 and WordPress, that has a domain name linked and also has added SSL security. With documentation, code, and a video explainer.

Info
- Name: Kidist Begashaw
- Student Number: 34363736
- Unit: ICT171

Project Overview
This project is a basic blog website hosted in the cloud named Grace and Light created for the ICT171 Cloud Server assessment. It is intended as a long-term personal space for spirituality and creativity, with the potential to develop into a full website with resources and digital downloads.

The website is hosted on an Amazon EC2 Ubuntu server, running Apache2, with WordPress installed and linked to a real domain.

Server Setup Summary
- [Project Description](#project-description)
- [EC2 Setup](#ec2-setup)
- [Installing LAMP Stack](#installing-lamp-stack)
- [Installing WordPress](#installing-wordpress)
- [Connecting Domain](#connecting-domain)
- [SSL with Let’s Encrypt](#ssl-with-lets-encrypt)
- [Final Notes](#final-notes)

Softwares Used
- Amazon EC2 (AWS)
- Ubuntu Linux
- Apache2
- MySQL 
- PHP
- WordPress
- Certbot (Let’s Encrypt)
- Namecheap (DNS domain registrar)
- GitHub documentation

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

To begin, I launched a free-tier EC2 instance using Ubuntu 20.04 on Amazon AWS

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

To host my Grace and Light website, I used a LAMP stack on an Ubuntu EC2 instance. Here's exactly how I set it up:

1. Update the Package

```bash
sudo apt update
```

2. Install Apache Web Server

Apache is the web server that handles HTTP requests.

```bash
sudo apt install apache2
```

To check if it's working, visit your EC2 Public IP — you should see the Apache default page.

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

I followed the steps to set a root password and secure the database setup.

5. Install PHP

PHP is what WordPress is built with.

```bash
sudo apt install php libapache2-mod-php php-mysql
```

Check version to confirm install:

```bash
php -v
```

6. Test Apache with PHP

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

![phpinfo-page-example](https://github.com/user-attachments/assets/6db5c258-73d3-49de-a34f-c2cc5e3df917)

---

3. Setting Up WordPress

After setting up the LAMP stack, I installed WordPress manually to set up my Grace and Light site. 

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

5. Complete WordPress Setup via Browser

I visited `http://graceandlight.space` in the browser.

![wordpress set up screen](https://github.com/user-attachments/assets/cb53338e-d4ed-403f-a693-84d1c6632dc4)

This opened the WordPress setup wizard, where I entered:

* Site title: `Grace and Light`
* Username: (admin user I chose)
* Password: (secure password)
* Email: (mine)

WordPress was installed and my site was live.

---

5. Connecting a Domain Name

I purchased the domain graceandlight.space via Namecheap.

To point it to my server:

* I added an A Record in Namecheap’s DNS settings:

  * Host: `@`
  * Value: `<my-ec2-public-ip>`
  * TTL: Automatic

Optional:

* Added a NAME:

  * Host: `www`
  * Value: `graceandlight.space`
    
 <img width="1440" alt="dns set up on namecheap" src="https://github.com/user-attachments/assets/7b863bdd-d215-4aaf-acaa-fec500b03777" />


After DNS propagated, I could visit my site at:
[https://graceandlight.space](https://graceandlight.space)**

---

6. Installing SSL (HTTPS)

To secure the website with HTTPS, I used Certbot

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

I was requested to:

* Choose the domain to secure (I selected `graceandlight.space`)
* Redirect HTTP to HTTPS — I chose the option to force secure HTTPS (recommended)

---

3. Verify the SSL Certificate

I visited:
`https://graceandlight.space`
It loaded securely with the padlock sign

I also tested it via SSL Labs:
[https://www.ssllabs.com/ssltest/](https://www.ssllabs.com/ssltest/)

---

Now my website is fully secure with a green lock.

---

Suggested README Additions for Your Project

1. Custom JavaScript Usage

I added a custom JavaScript script to enhance the user experience on the blog. This script \[briefly explain what it does, e.g., smooth scrolling, interactive menu, dynamic date display, etc.].

* Location: The script is located in `/path/to/js/script.js` (or within the WordPress theme folder).
* Purpose: It improves \[usability, engagement, visual effect, etc.].
* How to modify: To update or expand this script, edit the file and upload it via FTP or through the WordPress theme editor.

---

2. DNS Configuration

The website uses a custom domain `graceandlight.space`. Here’s how I configured DNS to point to my AWS EC2 server:

* A Records:

   `@` (root) points to the public IPv4 address of my EC2 instance (e.g., `54.157.0.236`).
   `www` points to the same IP address `@`.
* TTL: I used the automatic TTL provided by the domain registrar for fast propagation.
* Registrar: I managed DNS records via my domain registrar’s dashboard (specify which one, e.g., Namecheap, GoDaddy). 

---

3. SSL/TLS Setup with Let’s Encrypt**

To secure the website with HTTPS, I installed a free SSL certificate using Let’s Encrypt:

* Installed Certbot on the Ubuntu server to request and manage certificates.
* Used Certbot’s Apache plugin to automatically configure Apache with SSL.
* Set up automatic certificate renewal with a cron job to keep the certificate valid without manual intervention.
* After setup, the website is accessible securely via `https://www.graceandlight.space` without any certificate errors.

---


The website runs on WordPress, making content management easy:

Adding Posts: Log in to the WordPress admin panel at `https://www.graceandlight.space/wp-admin`.
- Creating Posts: Use the "Add New Post" feature to write devotionals or upload PDFs (e.g., the reflection journal).
* Media Library: Upload images, PDFs, or other media to the WordPress Media Library for easy insertion into posts.
- Editing Content: Posts can be edited or updated anytime via the admin dashboard.
- Plugins: Installed useful plugins to optimize performance, SEO, and security.

---

Custom JavaScript 
I added a custom JavaScript script to enhance the user experience. This script 
The script is located in the reflection corner page

It improves usability, engagement and visual effect.

---


This project illustrates the complete procedure for building a personal WordPress blog on a cloud-hosted AWS EC2 Ubuntu server from the ground up, including setting up the server, configuring the domain, offering SSL security, etc. The site Grace and Light is fully functional, secure, and easy to maintain in simply WordPress. All the documentation, scripts, and video provide instructions on this being replicated.

I have ensured to document everything from the server configuration to the DNS and SSL management for transparency and easy use. This project demonstrates basic cloud server skills and practical web development, while providing a good foundation for expanding the website later on.

  
