# AWS Three-Tier Web Application

A three-tier web application deployed on AWS with a focus on scalability, high availability, containerization, load balancing, reverse proxy configuration, and database connectivity.

## Project Overview

This project demonstrates the deployment of a web application using a three-tier architecture on AWS.

The application consists of:

- Presentation layer – Web access through Route 53 and CloudFront
- Application layer – Flask application running inside Docker containers on EC2
- Database layer – MariaDB database hosted on EC2

The infrastructure is deployed across multiple Availability Zones using public and private subnets, Application Load Balancers, and Auto Scaling.

---

## Architecture

![AWS Three-Tier Architecture](architecture/architecture-diagram.png)

### Architecture Flow

User
↓
GoDaddy Domain
↓
Route 53
↓
CloudFront
↓
Application Load Balancer
↓
Nginx Reverse Proxy
↓
Internal Application Load Balancer
↓
Application EC2 Instances
↓
Dockerized Flask Application
↓
MariaDB Database

---

## AWS Services Used

- Amazon VPC
- Amazon EC2
- Application Load Balancer (ALB)
- Auto Scaling Groups
- Amazon CloudFront
- Amazon Route 53
- Amazon S3
- Security Groups
- Availability Zones
- Docker
- Nginx
- MariaDB
- Python Flask

---

## Network Architecture

The application is deployed inside a custom VPC with CIDR:

`172.16.0.0/16`

The VPC is distributed across two Availability Zones.

### Public Subnets

Public subnets contain resources that require controlled internet-facing access, including the Application Load Balancer.

### Private Subnets

Private subnets contain:

- Nginx reverse proxy EC2 instances
- Application EC2 instances
- Database resources

The application and database tiers are not directly exposed to the internet.

---

## Application Layer

The backend application is developed using Python Flask.

The application provides a simple web form that accepts:

- Email
- Mobile number

Submitted information is stored in the MariaDB database.

The Flask application retrieves database connection details through environment variables rather than hardcoding credentials.

---

## Dockerization

The Flask application is containerized using Docker.

The Docker image contains:

- Python 3.10
- Flask
- MySQL/MariaDB connector
- Application source code
- HTML template

The application runs inside the container on port `5000`.

The container port is mapped to port `80` on the EC2 instance.

Example:

```bash
docker run -d -p 80:5000 \
  -e DB_HOST="<database-host>" \
  -e DB_USER="<database-user>" \
  -e DB_PASSWORD="<database-password>" \
  <docker-image>
