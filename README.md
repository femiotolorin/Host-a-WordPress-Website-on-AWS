# Host-a-WordPress-Website-on-AWS
---

# WordPress Website Deployment on AWS with DevOps Practices

This project entails the deployment of a WordPress website on Amazon Web Services (AWS) infrastructure using various DevOps practices for enhanced reliability, scalability, and security.

## Project Overview

The project aims to deploy a WordPress website utilizing AWS services and DevOps methodologies. Key components and practices involved in the deployment include:

1. **AWS Infrastructure Setup**:
   - Configured a Virtual Private Cloud (VPC) with public and private subnets across multiple availability zones.
   - Deployed an Internet Gateway to facilitate connectivity between VPC instances and the internet.
   - Established Security Groups as a network firewall mechanism.

2. **High Availability and Fault Tolerance**:
   - Leveraged multiple availability zones (AZs) for enhanced system reliability and fault tolerance.

3. **Resource Placement**:
   - Utilized public subnets for infrastructure components like NAT Gateway and Application Load Balancer (ALB).
   - Positioned web servers (EC2 instances) within private subnets for enhanced security.

4. **Networking and Connectivity**:
   - Enabled instances in private subnets to access the internet via the NAT Gateway.
   - Implemented EC2 Instance Connect Endpoint for secure connections to assets within both public and private subnets.

5. **Auto Scaling and Load Balancing**:
   - Employed an Application Load Balancer (ALB) and a target group for evenly distributing web traffic to an Auto Scaling Group of EC2 instances across multiple availability zones.
   - Utilized an Auto Scaling Group to automatically manage EC2 instances, ensuring website availability, scalability, fault tolerance, and elasticity.

6. **Storage and Database**:
   - Hosted website files on Elastic File System (EFS) for shared file storage.
   - Utilized Amazon RDS for hosting the WordPress database.

7. **Security and Monitoring**:
   - Secured application communications using a Certificate Manager.
   - Configured Simple Notification Service (SNS) to alert about activities within the Auto Scaling Group.

8. **Domain Management**:
   - Registered the domain name and set up a DNS record using Route 53.

## Deployment Scripts

### Installation of WordPress on EC2 Instance
The script `install_wordpress.sh` performs the following tasks:
- Updates software packages on the EC2 instance.
- Creates an HTML directory and mounts the EFS to it.
- Installs Apache web server and PHP along with necessary extensions for WordPress.
- Installs MySQL server and starts the service.
- Downloads and extracts WordPress files, configures the `wp-config.php` file, and restarts the webserver.

### Auto Scaling Group Launch Template Configuration
The script `launch_template_setup.sh` sets up an EC2 instance for use within an Auto Scaling Group. It performs tasks similar to the WordPress installation script but is designed to be used within an Auto Scaling Group launch template.

## Usage
To deploy the WordPress website on AWS infrastructure, follow these steps:
1. Set up AWS CLI and configure credentials.
2. Clone the repository containing deployment scripts.
3. Run the appropriate deployment script (`install_wordpress.sh` for standalone instance or `launch_template_setup.sh` for Auto Scaling Group instance).
4. Access the website using the provided domain name or IP address.

## Contributors
- Olorunfemi Otolorin - https://github.com/femiotolorin

## License
This project is licensed under the [MIT License](LICENSE).

---


