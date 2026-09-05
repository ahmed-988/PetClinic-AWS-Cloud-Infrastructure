# ☁️ Spring PetClinic on AWS

A production-style deployment of Spring PetClinic on AWS using a custom VPC, public and private subnets, NAT Gateway, Application Load Balancer, and isolated backend services.

The project demonstrates how to design and deploy a scalable 3-tier architecture on AWS while controlling network access using Route Tables and Security Groups.


<img width="942" height="602" alt="Untitled Diagram drawio (8)" src="https://github.com/user-attachments/assets/ad604920-dfda-431f-9003-924d9acc64c1" />


## 🚀 Project Overview

This project demonstrates how to deploy a Java web application on AWS with:

* 🌐 Custom VPC
* 🔀 Public & Private Subnets
* ⚖️ Application Load Balancer
* 🖥️ EC2 Instances
* 🔒 Security Groups
* 🚪 NAT Gateway
* 🌍 Internet Gateway
* 🗄️ MySQL
* 🐇 RabbitMQ
* ⚡ Memcached

The application and backend services are kept inside **private subnets**, while the ALB is exposed to the internet.

---

## ☁️ AWS Infrastructure

### 🌐 VPC

* CIDR
* Internet Gateway
* Public Route Table
* Private Route Table
* NAT Gateway

### 🌍 Public Subnets
Used for:

* ⚖️ Application Load Balancer
* 🚪 NAT Gateway

### 🖥️ Private Application Subnets
These subnets contain the Spring PetClinic application servers.

---

## 🔐 Security Groups

### ⚖️ ALB Security Group

Allows:

* HTTP `80` from the internet
* HTTPS `443` from the internet

### 🖥️ Application Security Group

Allows:

* TCP `8080` only from the ALB Security Group
* SSH `22` only from the administrator's public IP

### 🗄️ Backend Security Group

Allows application servers to access:

| Service   |    Port |
| --------- | ------: |
| MySQL     |  `3306` |
| RabbitMQ  |  `5672` |
| Memcached | `11211` |

---

## ⚖️ Application Load Balancer

An **Internet-facing ALB** distributes traffic between the two application servers.

### Target Group

* Name: `project-tg`
* Protocol: HTTP
* Port: `8080`
* Health Check: `/`

### Targets

* `app_vm_1`
* `app_vm_2`

---

## 🖥️ EC2 Instances

| Instance   | Role             | Subnet       | Access  |
| ---------- | ---------------- | ------------ | ------- |
| `app_vm_1` | Spring PetClinic | app-subnet-1 | Private |
| `app_vm_2` | Spring PetClinic | app-subnet-2 | Private |
| `db_vm`    | MySQL            | db-subnet    | Private |
| `mq_vm`    | RabbitMQ         | mq-subnet    | Private |
| `cache_vm` | Memcached        | cache-subnet | Private |

All instances run **Ubuntu** and do not have public IPv4 addresses.

---

## 🔄 Traffic Flow

**User → Internet → ALB → Application EC2 → Backend Services**

The application servers communicate with:

* 🗄️ MySQL
* 🐇 RabbitMQ
* ⚡ Memcached

---

## 🚪 NAT Gateway

The NAT Gateway provides **outbound internet access** for private instances.

Private instances can download packages and updates without having public IP addresses.

**Flow:**

`Private EC2 → Private Route Table → NAT Gateway → Internet Gateway → Internet`

---

## 🛠️ Application Stack

### ☕ Application

* 🌱 Spring PetClinic
* ☕ Java / JDK
* 📦 Maven
* 🐈 Apache Tomcat

### 🗄️ Database

* 🐬 MySQL

### 🐇 Message Broker

* 🐇 RabbitMQ

### ⚡ Cache

* ⚡ Memcached

---

## 🧰 AWS Services & Tools

* ☁️ Amazon VPC
* 🖥️ Amazon EC2
* ⚖️ Application Load Balancer
* 🌐 Internet Gateway
* 🚪 NAT Gateway
* 📍 Elastic IP
* 🛣️ Route Tables
* 🔐 Security Groups
* 🐧 Ubuntu Linux
* ☕ Java
* 📦 Maven
* 🐈 Apache Tomcat
* 🐬 MySQL
* 🐇 RabbitMQ
* ⚡ Memcached
* 🔧 Git & GitHub
* 🔑 SSH

---

## 📊 Project Result

The final deployment provides:

* ✅ Secure 3-Tier AWS Architecture
* ✅ Multi-AZ Application Deployment
* ✅ Internet-facing Application Load Balancer
* ✅ Two Private Application Servers
* ✅ Private MySQL Server
* ✅ Private RabbitMQ Server
* ✅ Private Memcached Server
* ✅ NAT Gateway for Outbound Access
* ✅ Security Groups for Network Control
* ✅ No Public IPs on Application/Backend Servers

---

## 👨‍💻 Author

**Ahmed Anany**

☁️ Cloud / DevOps Project

Built with AWS, Linux, Java, and modern cloud networking concepts.
