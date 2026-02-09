
# Refactoring with AWS - vProfile Project

## Project Overview
This project demonstrates how to refactor (re-architect) a traditional EC2-based application
to use AWS managed services (PaaS/SaaS). The goal is to reduce operational overhead,
improve scalability, reliability, and performance while following a pay-as-you-go model.

The application used in this project is **vProfile**, a Java Spring Boot web application
running on Apache Tomcat.

## Architecture Summary

### Frontend Layer
- **AWS Elastic Beanstalk (Tomcat)**  
  - Hosts the vProfile application  
  - Provides:
    - EC2 instances
    - Application Load Balancer
    - Auto Scaling Group
    - CloudWatch monitoring
    - Artifact storage in S3

### Backend Services (Managed by AWS)
- **Amazon RDS (MySQL 8.0)** – Database
- **Amazon ElastiCache (Memcached 1.6)** – Caching layer
- **Amazon MQ (RabbitMQ / ActiveMQ)** – Messaging broker
- **Amazon CloudFront** – Global CDN
- **Route 53 / GoDaddy DNS** – Domain management

## Deployment Flow
1. User requests the application URL.
2. DNS resolves to **CloudFront**.
3. CloudFront forwards the request to **Elastic Load Balancer**.
4. Load balancer routes traffic to **Elastic Beanstalk EC2 instances**.
5. Application communicates with:
   - RDS (MySQL)
   - ElastiCache (Memcached)
   - Amazon MQ (RabbitMQ)

## How to Deploy (High-Level Steps)
1. Create backend security group.
2. Create RDS (MySQL), ElastiCache, and Amazon MQ.
3. Initialize RDS schema using a temporary EC2 instance.
4. Create Elastic Beanstalk environment (Tomcat).
5. Allow Beanstalk security group access to backend security group.
6. Update `application.properties` with service endpoints.
7. Build artifact using Maven:  
   ```bash
   mvn install
   ```
8. Upload `vprofile-v2.war` to Elastic Beanstalk.
9. Attach HTTPS using ACM certificate.
10. Configure DNS and CloudFront distribution.

## Repository Structure
aws-refactoring-vprofile/
├── README.md
├── docs/
│   └── architecture.md
└── config/
    └── application.properties.template
