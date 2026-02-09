
# AWS Refactored Architecture Documentation

## Objective
Replace self-managed EC2 services with AWS managed services to achieve:
- Better scalability
- Lower operational cost
- Higher reliability
- Automated backups
- Easier deployments

## Components

### Elastic Beanstalk (Tomcat)
- Hosts Java application
- Automatically manages EC2, ALB, ASG, and monitoring

### RDS (MySQL 8.0)
- Managed relational database
- Automated backups
- High availability options
- Secure private networking

### ElastiCache (Memcached)
- In-memory caching
- Reduces database load
- Improves performance

### Amazon MQ (RabbitMQ)
- Managed message broker
- Replaces self-hosted RabbitMQ on EC2

### CloudFront
- Global CDN
- Caches static and dynamic content
- Reduces latency for global users

## Security Model
- Backend services are placed in a dedicated security group.
- Elastic Beanstalk instances are allowed access via security group rules.
- No public access to RDS, ElastiCache, or Amazon MQ.

## Deployment Strategy
- Rolling deployment (50% batch size)
- Minimum 2 instances
- Health checks enabled on /login
- Sticky sessions enabled
