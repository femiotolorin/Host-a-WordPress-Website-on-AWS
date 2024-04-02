[Alt text](/Host_a_WordPress_Website_on_AWS_outline.png)
---

# WordPress Website Deployment on AWS with DevOps Practices

This project entails the deployment of a WordPress website on Amazon Web Services (AWS) infrastructure using various DevOps practices for enhanced reliability, scalability, and security.

## Project Overview

The project aims to deploy a WordPress website utilizing AWS services and DevOps methodologies. Key components and activities involved in the deployment include:

1. **AWS Infrastructure Setup**:
   - Configure a Virtual Private Cloud (VPC) with public and private subnets across multiple availability zones.
   - Deploy an Internet Gateway to facilitate connectivity between VPC instances and the internet.
   - Establish Security Groups as a network firewall mechanism.

2. **High Availability and Fault Tolerance**:
   - Leverage multiple availability zones (AZs) for enhanced system reliability and fault tolerance.

3. **Resource Placement**:
   - Utilize public subnets for infrastructure components like NAT Gateway and Application Load Balancer (ALB).
   - Position web servers (EC2 instances) within private subnets for enhanced security.

4. **Networking and Connectivity**:
   - Enable instances in private subnets to access the internet via the NAT Gateway.
   - Implement EC2 Instance Connect Endpoint for secure connections to assets within both public and private subnets.

5. **Auto Scaling and Load Balancing**:
   - Employ an Application Load Balancer (ALB) and a target group for even distribution of web traffic to an Auto Scaling Group of EC2 instances across multiple availability zones.
   - Utilize an Auto Scaling Group to automatically manage EC2 instances, ensuring website availability, scalability, fault tolerance, and elasticity.

6. **Storage and Database**:
   - Host website files on Elastic File System (EFS) for shared file storage.
   - Utilize Amazon RDS for hosting the WordPress database.

7. **Security and Monitoring**:
   - Secure application communications in transit using a Certificate Manager.
   - Configure Simple Notification Service (SNS) to alert about activities within the Auto Scaling Group.

8. **Domain Management**:
   - Register the domain name and set up a DNS record using Route 53.
     
---

## Deployment Strategy

#### Manual Installation of WordPress on EC2 Instance
The following activities need to be carried out when performing a manual deployment of WordPress on an aws ec2 instance.
- Update software packages on the EC2 instance.
- Create an HTML directory and mount the EFS access point to it.
- Install Apache web server and PHP along with the necessary extensions for WordPress.
- Installs MySQL server and start the service.
- Download, extract and unzip WordPress files, change file permissions / ownership, configure the `wp-config.php` file, and restart the webserver.


#### Automated Installation of WordPress on EC2 Instance / Auto-Scaling Group
The following activities need to be carried out when performing an automated deployment of WordPress on an auto-scaling group.
- Develop desired Launch Template with configurations of network, security, cpu resources, startup scripts, etc.
- Develop script to be utilised as custom user data during instance launch to automate the following basic tasks :
   - Installation of website dependencies eg. Apache web server, PHP, MySql along with necessary extensions for WordPress.
   - Mounting of network based storage to local storage of each initialized instance.
   - Modification of file/folder permissions, ownership and locations of resources needed for WordPress operations.
   - Startup / restart / enablement of critical services required for WordPress operations.


#### Script for Auto scaling group launch template

```bash
#!/bin/bash
# update the software packages on the ec2 instance
sudo yum update -y
**install the apache web server, enable it to start on boot, and then start the server immediately
sudo yum install -y httpd
sudo systemctl enable httpd
sudo systemctl start httpd
**install php 8 along with several necessary extensions for wordpress to run
sudo dnf install -y \
php \
php-cli \
php-cgi \
php-curl \
php-mbstring \
php-gd \
php-mysqlnd \
php-gettext \
php-json \
php-xml \
php-fpm \
php-intl \
php-zip \
php-bcmath \
php-ctype \
php-fileinfo \
php-openssl \
php-pdo \
php-tokenizer
**install the mysql version 8 community repository
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm

**install the mysql server
sudo dnf install -y mysql80-community-release-el9-1.noarch.rpm
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
sudo dnf repolist enabled | grep "mysql.*-community.*"
sudo dnf install -y mysql-community-server

**start and enable the mysql server
sudo systemctl start mysqld
sudo systemctl enable mysqld
**environment variable
EFS_DNS_NAME=fs-02d3268559aa2a318.efs.us-east-1.amazonaws.com
**mount the efs to the html directory
echo "$EFS_DNS_NAME:/ /var/www/html nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2 0 0" >> /etc/fstab
mount -a
**set permissions
chown apache:apache -R /var/www/html
**restart the webserver
```

## Contributors
- Olorunfemi Otolorin - https://github.com/femiotolorin

## License
This project is licensed under the [MIT License](LICENSE).

---


