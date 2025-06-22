# Multi-Tier Java Web Application on AWS EC2

This project contains user-data scripts for launching a multi-tier Java Web Application using EC2 instances:
- **App**: Tomcat + Maven + Java
- **Database**: MySQL
- **RabbitMQ**
- **Memcached**

Each component is configured via user-data scripts.

## Directory structure
- app/user_data_app.sh
- db/user_data_db.sh
- mq/user_data_rabbit.sh
- cache/user_data_memcached.sh

## Usage
Copy the relevant user_data script and paste it in the EC2 Advanced User Data field when launching instances.

> ⚡️ Remember to customize Private IPs in \`user_data_app.sh\`.
