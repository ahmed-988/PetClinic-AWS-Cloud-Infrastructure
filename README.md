☁️ PetClinic on AWS — Secure 3-Tier Cloud Architecture

A production-style deployment of Spring PetClinic on AWS using a custom VPC, public and private subnets, NAT Gateway, Application Load Balancer, and isolated backend services.

The project demonstrates how to design and deploy a scalable 3-tier architecture on AWS while controlling network access using Route Tables and Security Groups.


<img width="942" height="602" alt="Untitled Diagram drawio (8)" src="https://github.com/user-attachments/assets/ad604920-dfda-431f-9003-924d9acc64c1" />



📌 Overview

This project deploys the Spring PetClinic application on an AWS VPC using the following architecture:


🏗️ AWS Infrastructure
VPC

A custom VPC was created with:

VPC CIDR: 10.0.0.0/16
Internet Gateway
Public Route Table
Private Route Table
NAT Gateway
Public Subnets
Subnet	Availability Zone	CIDR
public-subnet-1	AZ-A	10.0.6.0/24
public-subnet-2	AZ-B	10.0.7.0/24

The public subnets host the:

Application Load Balancer
NAT Gateway
Private Application Subnets
Subnet	Availability Zone	CIDR
app-subnet-1	AZ-A	10.0.1.0/24
app-subnet-2	AZ-B	10.0.5.0/24

These subnets contain the application EC2 instances running Spring PetClinic.

Private Backend Subnets
Service	Subnet	CIDR
MySQL	db-subnet	10.0.2.0/24
RabbitMQ	mq-subnet	10.0.3.0/24
Memcached	cache-subnet	10.0.4.0/24

All backend services remain private and are not directly exposed to the internet.

🔐 Security Groups
Senior-ALB-SG

Allows public HTTP/HTTPS traffic:

HTTP   : 80   ← 0.0.0.0/0
HTTPS  : 443  ← 0.0.0.0/0

Senior-App-SG

Allows application traffic only from the ALB:

TCP : 8080 ← Senior-ALB-SG


SSH access is restricted to the administrator's public IP:

TCP : 22 ← YOUR_PUBLIC_IP/32

Senior-Back-SG

Backend services accept traffic from the application layer:

MySQL      : 3306 ← Senior-App-SG
RabbitMQ   : 5672 ← Senior-App-SG
Memcached  : 11211 ← Senior-App-SG


Backend-to-backend communication is also allowed where required.

🌐 Routing Architecture
Public Route Table

The public route table contains:

Destination       Target

0.0.0.0/0   →   Internet Gateway


It is associated with:

public-subnet-1
public-subnet-2

Private Route Table

The private route table contains:

Destination       Target

0.0.0.0/0   →   NAT Gateway


It is associated with:

app-subnet-1
app-subnet-2
db-subnet
mq-subnet
cache-subnet


This allows private EC2 instances to access the internet for outbound operations such as package installation and software updates without assigning public IP addresses.

🚪 NAT Gateway

The NAT Gateway is deployed inside:

public-subnet-1


with an Elastic IP.

Its purpose is to provide outbound internet connectivity to resources located in private subnets.

Private instances follow this path:

Private EC2
     │
     ▼
Private Route Table
     │
     ▼
NAT Gateway
     │
     ▼
Internet Gateway
     │
     ▼
Internet


The application and backend instances therefore remain private while still being able to download packages and communicate with external services when required.

⚖️ Application Load Balancer

An Internet-facing Application Load Balancer was created across both public subnets.

Target Group
Name: project-tg
Protocol: HTTP
Port: 8080
Health Check: /


Targets:

app_vm_1
app_vm_2


Traffic flow:

User
 │
 ▼
Application Load Balancer
 │
 ├──► App EC2 #1 :8080
 │
 └──► App EC2 #2 :8080


The ALB distributes incoming requests between the two application servers.

🖥️ EC2 Instances

The infrastructure contains five EC2 instances:

Instance	Role	Subnet	Access
app_vm_1	Spring PetClinic	app-subnet-1	Private
app_vm_2	Spring PetClinic	app-subnet-2	Private
db_vm	MySQL	db-subnet	Private
mq_vm	RabbitMQ	mq-subnet	Private
cache_vm	Memcached	cache-subnet	Private

All instances use Ubuntu and do not receive public IPv4 addresses.

🧩 Application Stack
Application
Spring PetClinic
Java
Maven
Apache Tomcat
Database
MySQL
Message Broker
RabbitMQ
Cache
Memcached
Cloud Infrastructure
Amazon VPC
Amazon EC2
Internet Gateway
NAT Gateway
Elastic IP
Application Load Balancer
Route Tables
Security Groups
🛠️ Tools

The project was built using:

AWS Management Console
Amazon EC2
Amazon VPC
Application Load Balancer
Ubuntu Linux
Java / JDK
Maven
Apache Tomcat
MySQL
RabbitMQ
Memcached
Git & GitHub
SSH
🔄 Application Request Flow

        MySQL    RabbitMQ  Memcached

🌍 Private Instance Internet Access

Private instances do not have public IP addresses.

When they need outbound internet connectivity:

Private EC2
     │
     ▼
Private Route Table
     │
     ▼
NAT Gateway
     │
     ▼
Public Route Table
     │
     ▼
Internet Gateway
     │
     ▼
Internet


This architecture provides internet access without making the private instances directly reachable from the public internet.

📊 Result

The final deployment provides:

A custom AWS VPC
Segmented public and private subnets
Multi-AZ application deployment
Internet-facing Application Load Balancer
Two private application servers
Private MySQL database server
Private RabbitMQ server
Private Memcached server
NAT Gateway for outbound internet access
Security Groups controlling service-to-service communication
No public IP addresses on application/backend EC2 instances

The application can be accessed through the ALB DNS name:

http://<ALB-DNS-NAME>/

🔒 Security Considerations

This project intentionally keeps application and backend servers private.

Recommended production improvements include:

Store credentials in AWS Secrets Manager or SSM Parameter Store
Use HTTPS with an SSL/TLS certificate
Use Amazon RDS instead of self-managed MySQL
Use Amazon ElastiCache instead of self-managed Memcached
Use Amazon MQ or another managed messaging solution where appropriate
Deploy NAT Gateways in both Availability Zones for higher availability
Use IAM roles instead of static credentials
Use Systems Manager Session Manager instead of exposing SSH where possible
Restrict security group rules to the minimum required ports
Enable monitoring and logging with CloudWatch
Use infrastructure-as-code such as Terraform or CloudFormation
📁 Suggested Repository Structure
petclinic-aws/
│
├── README.md
│
├── architecture/
│   └── aws-architecture.png
│
├── docs/
│   ├── infrastructure.md
│   ├── security.md
│   └── deployment.md
│
├── app/
│   └── README.md
│
└── screenshots/
    ├── vpc.png
    ├── subnets.png
    ├── route-tables.png
    ├── security-groups.png
    ├── ec2.png
    ├── alb.png
    └── petclinic.png


Important: Never upload .pem files, passwords, private keys, database credentials, or other secrets to GitHub.

👨‍💻 Author

Your Name

Cloud / DevOps Project

Built with AWS, Linux, Java, and modern cloud networking concepts.
