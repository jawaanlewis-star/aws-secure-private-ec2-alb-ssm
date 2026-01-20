# Troubleshooting and Lessons Learned

## Issue 1: Target Group Unhealthy – Request Timed Out

### Problem
Target group showed status "Unhealthy" with error "Request timed out".

### Cause
- Security Group blocked inbound HTTP
- Network Access Control List (NACL) blocked return traffic

### Fix
- Updated EC2 Security Group to allow HTTP from ALB Security Group
- Updated NACL to allow:
  - TCP port 80
  - Ephemeral ports (1024–65535)

---

## Issue 2: Web Page Not Loading Through ALB

### Problem
ALB DNS did not display the web page.

### Cause
- Apache not running or not listening on port 80
- Security Group or NACL blocking traffic

### Fix
- Verified Apache status
- Confirmed port 80 listening
- Fixed inbound rules

---

## Issue 3: Systems Manager "Connection Lost"

### Problem
Fleet Manager showed "Ping status: Connection lost".

### Cause
- Private subnet could not reach Systems Manager endpoints
- Missing NAT or blocked HTTPS

### Fix
- Created VPC Interface Endpoints:
  - ssm
  - ec2messages
  - ssmmessages
- Enabled DNS resolution and hostnames on VPC
- Created security group for endpoints allowing HTTPS from VPC CIDR

---

## Issue 4: Session Manager "Instance Not Configured"

### Problem
Session Manager said instance not configured for Session Manager.

### Cause
- Missing IAM permissions on instance role
- Instance profile not refreshed

### Fix
- Attached AmazonSSMManagedInstanceCore to instance role
- Detached and reattached role to force refresh
- Rebooted instance

---

## Issue 5: Session Manager Failed Due to Region Mismatch

### Problem
Session Manager failed even though instance was online.

### Cause
- Console was in wrong region

### Fix
- Switched to correct region where EC2 was running

---

## Lessons Learned

- Networking issues are the most common root cause of cloud failures
- NACLs are stateless and require explicit return traffic rules
- Systems Manager requires:
  - IAM permissions
  - Network path
  - DNS resolution
- Always verify region before troubleshooting
- Secure design requires more planning but reduces risk
