# Dynamic-Web-App-on-AWS


## Background of the Problem

The primary objective of this project was to deploy a highly available, secure, scalable, and fault-tolerant dynamic website on the Amazon Web Services (AWS) cloud platform. The motivation stemmed from the need for a robust architecture that could handle varying web traffic efficiently, maintain high availability across multiple geographic regions, and ensure strong security for both the hosted assets and data communications.

## Approach to the Solution

To achieve these objectives, I designed a cloud-native solution based on AWS best practices. I established a structured AWS architecture leveraging multiple AWS services, including computing, networking, storage, and security services, to meet the targeted goals. Emphasis was placed on automation and scalability to ensure that the infrastructure could dynamically adapt to user demands and maintain reliability without significant manual intervention.


---

## Step-by-Step Project Execution

### 1. AWS CLI Setup
- Installed AWS CLI using Homebrew:

### 2. IAM User & Access Key Creation
- Created an IAM user with necessary permissions.
- Generated an access key to configure AWS CLI authentication.

### 3. AWS CLI Configuration Test
- Verified AWS CLI configuration

### 4. Amazon S3 Bucket Creation & File Upload
- Created an S3 bucket to store application files.

### 5. VPC Configuration
- Created a Virtual Private Cloud (VPC) and enabled DNS hostname settings.

### 6. Internet Gateway & Subnet Configuration
- Created an Internet Gateway and attached it to the VPC.
- Configured subnets across two Availability Zones (AZs) as follows:
  ```
  Public Subnet AZ1:        10.0.0.0/24
  Private App Subnet AZ1:   10.0.2.0/24
  Private Data Subnet AZ1:  10.0.4.0/24

  Public Subnet AZ2:        10.0.1.0/24
  Private App Subnet AZ2:   10.0.3.0/24
  Private Data Subnet AZ2:  10.0.5.0/24
  ```
- Enabled auto-assign public IP addresses for public subnets.

### 7. Route Tables Setup
- Created a route table for public subnets, associating it with the Internet Gateway.

### 8. NAT Gateway Creation
- Deployed a NAT Gateway in the public subnet for internet access by private subnet instances.

### 9. Private Route Table Setup
- Configured route tables for private subnets, pointing internet-bound traffic to the NAT Gateway.

### 10. Security Groups
- Created Security Groups as firewall rules to control inbound and outbound traffic.

### 11. EC2 Instance Connect Endpoint
- Created an EC2 Instance Connect Endpoint to securely SSH into instances.

### 12. EC2 Instance Launch and SSH
- Launched an EC2 instance within the configured subnets.
- SSH-connected securely via AWS CLI:
  ```bash
  aws ec2-instance-connect ssh --instance-id <EC2_INSTANCE_ID>
  ```

### 13. RDS Database Setup
- Launched an RDS instance for application data storage.
- S3 bucket to store the SQL database scripts.
- Uploaded the database SQL script to the bucket.

### 14. IAM Role with S3 Access
- Created an IAM role granting EC2 instances permission to access S3 buckets.

### 15. Data Migration to RDS
- Launched EC2 instance.
- Prepared a migration script using a provided template.
- Executed migration of data to the RDS instance.
- Terminated the migration EC2 instance after successful migration.

### 16. Target Group and Load Balancer Setup
- Created an Application Load Balancer (ALB) Target Group.
- Launched a new EC2 instance and registered it with the Target Group.
- Created an Application Load Balancer associated with the target group.

### 17. Website Deployment on EC2
- Prepared an installation script from a reference.
- Installed and deployed the website on the EC2 instance.

### 18. Domain Registration & SSL/TLS Setup
- Registered a domain name using Route 53.
- Created DNS records to point to the Application Load Balancer.
- Requested and obtained an SSL/TLS certificate via AWS Certificate Manager.
- Configured HTTPS listener on the Application Load Balancer using the obtained certificate.

### 19. AMI and Auto Scaling Setup
- Created an Amazon Machine Image (AMI) from the configured EC2 instance.
- Terminated the original EC2 instance.
- Created a launch template using the AMI.
- Established an Auto Scaling Group to automatically manage EC2 instances based on load.

### 20. Resource Cleanup
- Cleaned up all AWS resources created during the project after completion to avoid incurring unnecessary charges.

---


## Implementation of Technology Used

The AWS cloud environment and website deployment involved these core technologies and services:

### Network Infrastructure:
- **Virtual Private Cloud (VPC)**:
  - Configured to include both public and private subnets across two AWS Availability Zones, providing isolation and protection of resources.
- **Internet Gateway**:
  - Enabled VPC instances to communicate securely with the internet.
- **Public and Private Subnets**:
  - Utilized public subnets for services that required external internet access, such as NAT Gateway and Application Load Balancer.
  - Deployed web servers within private subnets to ensure enhanced security by restricting direct external internet access.

### Compute and Scalability:
- **EC2 Instances**:
  - Hosted the dynamic website, ensuring a secure and performant computing environment.
- **Auto Scaling Group**:
  - Configured to dynamically manage the EC2 instances, automatically adjusting capacity in response to real-time demand, ensuring high availability, scalability, fault tolerance, and cost efficiency.
- **Application Load Balancer**:
  - Implemented to evenly distribute incoming web traffic to EC2 instances, enabling efficient load management and improved user experience.

### Storage:
- **Amazon S3**:
  - Stored application code and associated assets, providing a highly durable and scalable storage solution with easy access management.

### Security:
- **Security Groups**:
  - Established virtual firewalls to strictly control inbound and outbound network traffic.
- **AWS Certificate Manager (ACM)**:
  - Secured application communications through SSL/TLS encryption, ensuring the confidentiality and integrity of data in transit.
- **EC2 Instance Connect Endpoint**:
  - Enabled secure SSH access to EC2 instances within both public and private subnets, significantly reducing exposure to security risks.

### Monitoring and Notification:
- **Simple Notification Service (SNS)**:
  - Set up to promptly alert administrators regarding changes and activities within the Auto Scaling Group, ensuring proactive monitoring and timely incident responses.

### Domain Management:
- **Route 53**:
  - Managed DNS records effectively, providing users seamless access to the website through a registered custom domain name.


## Lessons Learned from Challenges

During the implementation, I encountered several key challenges:

- **Coding and Automation**:
  - As someone who wasn't initially very proficient with coding, scripting, and automation posed significant hurdles. However, facing these challenges motivated me to deepen my understanding of scripting languages and Infrastructure as Code (IaC). Gradually, through consistent practice, referencing AWS documentation, and leveraging existing community scripts, I became more comfortable and confident with automating complex deployments.

- **Configuration Complexity**:
  - Setting up complex configurations with multiple AWS services initially posed difficulties. Leveraging Infrastructure as Code (IaC) via automated scripts helped mitigate errors and significantly streamlined deployment processes. 
  
- **Security Management**:
  - Ensuring secure and controlled access to the EC2 instances required careful configuration of Security Groups and EC2 Instance Connect. The challenge was resolved by adhering strictly to the principle of least privilege, limiting access only to necessary resources and connections.


- **SSL/TLS Certification**:
  - Initially encountered issues configuring ACM to integrate seamlessly with the Load Balancer. Rigorous documentation review and meticulous troubleshooting led to effective certificate deployment, ensuring secure and encrypted web traffic.

These experiences underscored the importance of iterative testing, detailed documentation, and systematic troubleshooting in managing cloud infrastructure effectively.

## Emphasis on Security

Security considerations were central to every stage of the project:

- **Network Isolation**: Utilizing private subnets effectively isolated the web servers from external threats.
- **Traffic Control**: Security Groups tightly regulated network traffic, minimizing the potential attack surface.
- **Encrypted Communication**: The use of AWS Certificate Manager for SSL/TLS certificates secured all user interactions and sensitive data exchanges.
- **Secure Remote Management**: EC2 Instance Connect Endpoint provided secure, controlled access to EC2 instances, significantly reducing vulnerability risks.
- **Automated Alerts**: SNS notifications ensured rapid detection and response to any unusual activity or configuration changes.

By adopting AWS security best practices and continually reviewing and improving security measures, the infrastructure was resilient against various security threats and vulnerabilities.
