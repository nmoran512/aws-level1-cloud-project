# AWS Level 1 Cloud Project: Public EC2 Web Server with S3 Integration

## Overview
Created a custom VPC with a public subnet, and configured internet connectivity through an Internet Gateway. Launched an EC2 instance with the Amazon Linux AMI. Created an S3 bucket, and configured an IAM role allowing the EC2 instance access to the S3 bucket without storing AWS credentials locally. 

Validated the integration using AWS CLI from the EC2 instance to access and upload objects to the S3 bucket. Used Amazon CloudWatch to monitor basic EC2 performance metrics.

## Architecture
The environment is deployed within a custom VPC utilizing a public subnet. The subnet is associated with a route table containing a default route to an Internet Gateway, allowing the EC2 instance to communicate with the internet. The EC2 instance's security group permits inbound HTTP traffic, and restricts SSH access to a specified public IP address. 

The Apache web server and its content are hosted on the EC2 instance, with data stored on the EBS volume. An S3 bucket was also created, and an IAM role was attached to the EC2 instance to provide authorized access to objects stored in the bucket without using locally stored AWS credentials.

<img width="251" height="660" alt="Lab1 drawio" src="https://github.com/user-attachments/assets/28ae4b2d-ae60-4d9a-bb69-93ca4059045f" />

Amazon CloudWatch is used to monitor EC2 performance and health metrics.

## AWS Services Used
<li>Amazon VPC</li>
<li>Amazon EC2</li>
<li>Amazon S3</li>
<li>AWS Identity and Access Management (IAM)</li>
<li>Amazon CloudWatch</li>

## Implementation
<h4>1. Created the network</h4>
<li>Created a custom VPC with CIDR 10.0.0.0/16</li>
<li>Created a public subnet with CIDR 10.0.1.0/24</li>
<li>Attached an Internet Gateway to the VPC</li>
<li>Configured the routing table with a default route 0.0.0.0/0 to the Internet Gateway</li>

<h4>2. Configured security</h4>
<li>Created a security group allowing HTTP on port 80 from the internet</li>
<li>Restricted SSH on port 22 to a specified public IP address</li>

<h4>3. Deployed the EC2 instance</h4>
<li>Launched an Amazon Linux EC2 instance in the public subnet</li>
<li>Connected to the instance using SSH</li>

<h4>4. Configured the web server</h4>
<li>Installed Apache HTTP Server</li>
<li>Started and enabled the httpd service</li>
<li>Created an index.html file in /var/www/html</li>
<li>Verified the webpage was reachable through the EC2 public IP address</li>

<h4>5. Integrated Amazon S3</h4>
<li>Created an S3 bucket</li>
<li>Created an IAM role for EC2 with S3 permissions</li>
<li>Attached the IAM role to the EC2 instance</li>
<li>Used the AWS CLI from the EC2 instance to list the bucket and upload a test object</li>

<h4>6. Monitored the instance</h4>
<li>Used Amazon CloudWatch to review EC2 performance and health metrics, including CPU utilization and network activity</li>

## Validation
<li><strong>Web server:</strong> Opened EC2 public IP in a browser and confirmed the Apache hosted "Hello World" page loaded successfully</li>
<li><strong>SSH Access:</strong> Connected to EC2 instance using SSH from the allowed public IP</li>
<li><strong>S3 access: </strong> Used the AWS CLI from the EC2 instance to list the S3 bucket and confirm access through the attached IAM role</li>
<li><strong>S3 Upload:</strong> Uploaded a test file from the EC2 instance to the S3 bucket and verified the object appeared in the bucket</li>
<li><strong>CloudWatch:</strong> Reviewed EC2 metrics such as CPU utilization, network activity, and instance health</li>
<li><strong>Security:</strong> Confirmed SSH was not open to 0.0.0.0/0 and HTTP was allowed on port `80` in the EC2 instance's security group</li>

## Troubleshooting

### EC2 Instance Did Not Receive a Public IPv4 Address

**Issue:**  
The EC2 instance launched successfully but did not receive a public IPv4 address, preventing SSH access.

**Root Cause:**  
Auto-assign public IPv4 was disabled for the public subnet.

**Resolution:**  
Enabled automatic public IPv4 assignment for the subnet and relaunched the EC2 instance.

**Lesson Learned:**  
A subnet having a route to an Internet Gateway does not automatically assign public IP addresses to instances deployed within it.

### Permission Denied When Creating Web Content

**Issue:**  
Received a permission-denied error when attempting to create `index.html` under `/var/www/html`.

**Root Cause:**  
The Apache document root was owned by the root user.

**Resolution:**  
Used elevated privileges to create and modify the web content.

**Lesson Learned:**  
Linux filesystem permissions remain an important part of managing applications hosted on EC2.

## Security Considerations
<li>No AWS keys stored on EC2</li>
<li>IAM role used for S3 authentication</li>
<li>Broad S3 permissions used for lab validation. Should be replaced with leas-privilege permissions in production.</li>
## What I Learned

## Future Improvements
Future improvements will include private subnets, load balancing, HTTPS, least-privileged S3 policies, CloudWatch Alarms, and Terraform
