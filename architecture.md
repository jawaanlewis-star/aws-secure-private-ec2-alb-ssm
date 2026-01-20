# Architecture Design – Secure AWS Web Application

## Goal

Design a secure Amazon Web Services (AWS) architecture where:
- Web traffic is publicly accessible
- Backend servers are private
- Administrative access is secure and auditable
- No Secure Shell (SSH) access is exposed

## High-Level Flow

User Traffic:
User → Application Load Balancer (ALB) → Target Group → Private Amazon Elastic Compute Cloud (EC2)

Admin Access:
Administrator → AWS Systems Manager (SSM) Session Manager → VPC Interface Endpoints → Private EC2

## Network Layout

- One Virtual Private Cloud (VPC)
- Public subnets:
  - Application Load Balancer
  - Network Address Translation (NAT) Gateway
- Private subnets:
  - EC2 web server
- Route Tables:
  - Public subnet route table: 0.0.0.0/0 → Internet Gateway
  - Private subnet route table: 0.0.0.0/0 → NAT Gateway (for outbound) and VPC Endpoints (for SSM)

## Security Design

### Public Access
- Only the ALB has public exposure
- ALB Security Group:
  - Inbound: Hypertext Transfer Protocol (HTTP) port 80 from 0.0.0.0/0
  - Outbound: Forward traffic to target group

### Private Access
- EC2 has no public Internet Protocol (IP) address
- EC2 Security Group:
  - Inbound: HTTP port 80 only from ALB Security Group
  - No SSH allowed

### Network Access Control Lists (NACLs)
- Public NACL allows:
  - HTTP and HTTPS inbound
  - Ephemeral ports for return traffic
- Private NACL allows:
  - HTTP from VPC
  - HTTPS for Systems Manager
  - Ephemeral ports for stateless return traffic

## Administrative Access

- Administration via AWS Systems Manager Session Manager
- No SSH keys stored
- No inbound Secure Shell ports open
- Access is logged and auditable through AWS CloudTrail and Systems Manager logs

## Why This Design

- Reduces attack surface by removing public access to EC2
- Enforces least-privilege networking
- Separates user traffic from administrative traffic
- Uses managed AWS services for secure operations

## Failure Handling

- If ALB health checks fail, target group shows unhealthy
- If SSM connectivity fails:
  - Check IAM instance role
  - Check VPC endpoints
  - Check Network ACL rules
  - Check DNS settings in VPC
