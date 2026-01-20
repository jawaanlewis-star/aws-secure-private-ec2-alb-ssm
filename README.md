# Secure AWS Web Architecture

Private Amazon Elastic Compute Cloud (EC2) web server behind a public Application Load Balancer (ALB) with secure administration using AWS Systems Manager (SSM) Session Manager and Virtual Private Cloud (VPC) Interface Endpoints.

## What I Built

- Private EC2 instance with no public IP
- Public Application Load Balancer handling all web traffic
- No Secure Shell (SSH) access
- Administration via AWS Systems Manager Session Manager
- VPC Interface Endpoints for private SSM connectivity
- Custom Security Groups and Network Access Control Lists (NACLs)

## Architecture Overview

User → Application Load Balancer → Target Group → Private EC2  
Admin → Systems Manager Session Manager → VPC Endpoints → Private EC2

## Services Used

- Amazon Elastic Compute Cloud (EC2)
- Application Load Balancer (ALB)
- Virtual Private Cloud (VPC)
- VPC Interface Endpoints
- AWS Systems Manager (SSM)
- Security Groups
- Network Access Control Lists (NACLs)
- Identity and Access Management (IAM)

## Screenshots

### Load Balancer Working
![ALB](screenshots/06-alb-dns-working.png)

### Target Group Healthy
![TG](screenshots/05-target-group-healthy.png)

### Private Network ACL
![NACL](screenshots/07-private-nacl-rules.png)

### VPC Endpoints
![Endpoints](screenshots/08-vpc-endpoints-ssm.png)

### Session Manager Access
![SSM](screenshots/10-curl-localhost-proof.png)
