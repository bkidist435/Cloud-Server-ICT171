# Cloud-Server-ICT171
A personal blog website called Grace and Light, hosted on an Amazon EC2 Ubuntu server using Apache2 and WordPress, with a linked domain name and SSL security. Includes documentation, code, and video explainer.

Grace and Light – Cloud Server Project

Student Info
- Name: Kidist Begashaw
- Student Number: 34363736
- Unit: ICT171

Project Overview
This project is a simple cloud-hosted blog website titled **Grace and Light**, built as part of the ICT171 Cloud Server assessment. It is designed to serve as a personal spiritual and creative space with long-term potential to expand into a full website with resources and digital downloads.

The website is hosted on an **Amazon EC2 Ubuntu server, running **Apache2**, with WordPress installed and linked to a real domain.

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
