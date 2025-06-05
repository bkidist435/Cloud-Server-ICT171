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

To begin, I launched a **free-tier EC2 instance** using Ubuntu 20.04 on [Amazon Web Services (AWS)](https://aws.amazon.com/ec2/).

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
```

---

2. Installing Apache, MySQL, and PHP (LAMP Stack)
To download both MYSQL and PHP (LampStack on Ubuntu I followed this site:

(https://www.digitalocean.com/community/tutorials/how-to-install-lamp-stack-on-ubuntu)

Once connected to the EC2 instance via SSH:

---

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

I installed WordPress in the `/var/www/html` directory:

```bash
cd /var/www/html
sudo rm index.html
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xvzf latest.tar.gz
sudo mv wordpress/* .
sudo rm -rf wordpress latest.tar.gz
```

Then, I set the proper file permissions:

```bash
sudo chown -R www-data:www-data /var/www/html
sudo chmod -R 755 /var/www/html
```

---

4. Creating the Database and Configuring WordPress

Inside the MySQL shell:

```bash
sudo mysql
```

```sql
CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'securepassword';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

Then I configured WordPress to connect to this database:

```bash
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php
```

I updated these lines:

```php
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wpuser' );
define( 'DB_PASSWORD', 'securepassword' );
```

Restart Apache:

```bash
sudo systemctl restart apache2
```

Now when I visited my public IP, the WordPress setup screen appeared!

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

After DNS propagated, I could visit my site at:
[https://graceandlight.space](https://graceandlight.space)**

---

6. Installing SSL (HTTPS)

To secure the website with HTTPS, I used **Certbot**:

```bash
sudo apt install certbot python3-certbot-apache
sudo certbot --apache
```

During setup:

* I selected my domain (`graceandlight.space`)
* Chose the option to redirect HTTP to HTTPS

Now my website is fully secure with a green lock! 🔒


