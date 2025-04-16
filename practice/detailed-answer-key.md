# AWS Certified Solutions Architect - Detailed Answer Key

This document provides detailed explanations for each question in the practice exam, helping you understand the reasoning behind the correct answers.

## Section 1: Design Secure Architectures

### Question 1
**Correct Answer: C - AWS IAM Identity Center and AWS CloudTrail**

**Detailed Explanation:**
- AWS IAM Identity Center (formerly AWS SSO) provides centralized access management across multiple AWS accounts
  - [IAM Identity Center Documentation](https://docs.aws.amazon.com/singlesignon/latest/userguide/whatis.html)
  - Supports SAML 2.0 and SCIM for identity federation
  - Enables single sign-on and centralized user management
- AWS CloudTrail provides comprehensive audit logging of all API calls
  - [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
  - Logs all API calls across AWS services
  - Provides event history and compliance reporting
- Together, they provide a complete solution for centralized access management and audit logging
- Other options either lack centralized access management (A) or audit logging (B)

### Question 2
**Correct Answer: D - SSE-KMS, SSL/TLS, S3 Access Logs, Versioning**

**Detailed Explanation:**
- SSE-KMS provides server-side encryption using AWS KMS keys
  - [S3 Encryption Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html)
  - Uses AWS KMS for key management
  - Provides better security than SSE-S3 with customer-managed keys
- SSL/TLS ensures encryption in transit
  - [S3 Security Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security.html)
  - Uses HTTPS for all data transfers
  - Provides end-to-end encryption
- S3 Access Logs provide detailed access tracking
  - [S3 Server Access Logging](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html)
  - Logs all bucket access requests
  - Enables security analysis and auditing
- Versioning maintains object history for recovery
  - [S3 Versioning Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html)
  - Preserves all versions of objects
  - Enables recovery from accidental deletions
- This combination provides the most comprehensive security solution

### Question 3
**Correct Answer: A - AWS KMS and SSL/TLS**

**Detailed Explanation:**
- AWS KMS provides encryption at rest with automatic key rotation
  - [KMS Documentation](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)
  - Supports automatic key rotation
  - Provides hardware security module (HSM) protection
- SSL/TLS provides encryption in transit
  - [RDS Security Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html)
  - Uses SSL/TLS for all database connections
  - Provides end-to-end encryption
- Together, they provide end-to-end encryption
- Other options either lack key rotation (B) or are not specifically designed for RDS encryption (C, D)

### Question 4
**Correct Answer: A - AWS IAM Password Policy**

**Detailed Explanation:**
- IAM Password Policy allows setting specific password requirements
  - [IAM Password Policy Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_passwords_account-policy.html)
  - Can enforce minimum length, character types, and expiration
  - Supports password history and reuse prevention
- Other options are for secret management, not password policies
- This is the only service specifically designed for password policy management

### Question 5
**Correct Answer: A - AWS Client VPN and AWS CloudTrail**

**Detailed Explanation:**
- Client VPN provides secure remote access with MFA support
  - [Client VPN Documentation](https://docs.aws.amazon.com/vpn/latest/clientvpn-admin/what-is.html)
  - Supports multi-factor authentication
  - Provides secure remote access to VPC resources
- CloudTrail provides audit logging of all VPN connections
  - [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
  - Logs all VPN connection events
  - Provides detailed audit trail
- Together, they provide secure access with audit capabilities
- Other options either lack MFA support (B) or audit logging (C, D)

### Question 6
**Correct Answer: A - AWS Systems Manager Session Manager**

**Detailed Explanation:**
- Session Manager provides secure SSH access without exposing ports
  - [Session Manager Documentation](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager.html)
  - Eliminates need for bastion hosts
  - Provides secure instance access
- Includes session recording and audit logging
  - [Session Manager Logging](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-logging.html)
  - Records all session activity
  - Enables security auditing
- Provides key rotation capabilities
- Other options are for different management tasks, not secure access

### Question 7
**Correct Answer: A - Amazon API Gateway with AWS WAF**

**Detailed Explanation:**
- API Gateway provides API key authentication and rate limiting
  - [API Gateway Documentation](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)
  - Supports API key authentication
  - Provides request throttling
- WAF provides request validation and protection
  - [WAF Documentation](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html)
  - Protects against common web exploits
  - Provides request filtering
- Together, they provide comprehensive API security
- Other options either lack request validation (B) or rate limiting (C, D)

### Question 8
**Correct Answer: A - AWS IAM and AWS KMS**

**Detailed Explanation:**
- IAM provides fine-grained access control
  - [IAM Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)
  - Supports resource-based policies
  - Enables granular permissions
- KMS provides encryption at rest and in transit
  - [KMS Documentation](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)
  - Supports encryption at rest
  - Provides key management
- Together, they provide comprehensive DynamoDB security
- Other options either lack encryption (B) or are not designed for DynamoDB (C, D)

### Question 9
**Correct Answer: A - AWS Secrets Manager and AWS CloudTrail**

**Detailed Explanation:**
- Secrets Manager provides secure secret management for containers
  - [Secrets Manager Documentation](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)
  - Supports automatic rotation
  - Provides secure storage
- CloudTrail provides audit logging
  - [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
  - Logs all secret access
  - Provides audit trail
- Together, they provide secure container management
- Other options either lack secret management (B) or audit logging (C, D)

### Question 10
**Correct Answer: A - AWS WAF and AWS Shield**

**Detailed Explanation:**
- WAF provides request validation and protection
  - [WAF Documentation](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html)
  - Protects against common web exploits
  - Provides request filtering
- Shield provides DDoS protection
  - [Shield Documentation](https://docs.aws.amazon.com/waf/latest/developerguide/ddos-overview.html)
  - Protects against DDoS attacks
  - Provides automatic mitigation
- Together, they provide comprehensive CloudFront security
- Other options either lack DDoS protection (B) or request validation (C, D)

### Question 11
**Correct Answer: A - AWS KMS and AWS CloudTrail**

**Detailed Explanation:**
- KMS provides encryption for environment variables
  - [KMS Documentation](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)
  - Supports envelope encryption
  - Provides secure key storage
  - Enables automatic key rotation
- CloudTrail provides audit logging
  - [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
  - Logs all Lambda function executions
  - Provides detailed audit trail
  - Supports compliance requirements
- Together, they provide secure Lambda function management
- Other options either lack encryption (B) or audit logging (C, D)

### Question 12
**Correct Answer: A - Amazon API Gateway with AWS WAF and AWS CloudTrail**

**Detailed Explanation:**
- API Gateway provides API key authentication and rate limiting
  - [API Gateway Documentation](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)
  - Supports API key validation
  - Provides request throttling
  - Enables usage plans
- WAF provides request validation
  - [WAF Documentation](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html)
  - Protects against common web exploits
  - Provides request filtering
  - Supports custom rules
- CloudTrail provides audit logging
  - [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
  - Logs all API Gateway events
  - Provides detailed audit trail
  - Supports compliance requirements
- Together, they provide comprehensive API security
- Other options either lack request validation (B) or audit logging (C, D)

### Question 13
**Correct Answer: A - AWS Secrets Manager and AWS CloudTrail**

**Detailed Explanation:**
- Secrets Manager provides secure secret management for EKS
  - [Secrets Manager Documentation](https://docs.aws.amazon.com/secretsmanager/latest/userguide/intro.html)
  - Supports automatic rotation
  - Provides secure storage
  - Integrates with EKS
- CloudTrail provides audit logging
  - [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
  - Logs all secret access
  - Provides detailed audit trail
  - Supports compliance requirements
- Together, they provide secure EKS management
- Other options either lack secret management (B) or audit logging (C, D)

### Question 14
**Correct Answer: A - AWS KMS and AWS CloudTrail**

**Detailed Explanation:**
- KMS provides message encryption
  - [KMS Documentation](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)
  - Supports envelope encryption
  - Provides secure key storage
  - Enables automatic key rotation
- CloudTrail provides audit logging
  - [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
  - Logs all SQS operations
  - Provides detailed audit trail
  - Supports compliance requirements
- Together, they provide secure SQS management
- Other options either lack encryption (B) or audit logging (C, D)

### Question 15
**Correct Answer: A - AWS KMS and AWS CloudTrail**

**Detailed Explanation:**
- KMS provides message encryption
  - [KMS Documentation](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)
  - Supports envelope encryption
  - Provides secure key storage
  - Enables automatic key rotation
- CloudTrail provides audit logging
  - [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
  - Logs all SNS operations
  - Provides detailed audit trail
  - Supports compliance requirements
- Together, they provide secure SNS management
- Other options either lack encryption (B) or audit logging (C, D)

### Question 16
**Correct Answer: A - AWS KMS and AWS CloudTrail**

**Detailed Explanation:**
- KMS provides data encryption
  - [KMS Documentation](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)
  - Supports envelope encryption
  - Provides secure key storage
  - Enables automatic key rotation
- CloudTrail provides audit logging
  - [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
  - Logs all Kinesis operations
  - Provides detailed audit trail
  - Supports compliance requirements
- Together, they provide secure Kinesis management
- Other options either lack encryption (B) or audit logging (C, D)

### Question 17
**Correct Answer: A - AWS KMS and AWS CloudTrail**

**Detailed Explanation:**
- KMS provides event encryption
  - [KMS Documentation](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)
  - Supports envelope encryption
  - Provides secure key storage
  - Enables automatic key rotation
- CloudTrail provides audit logging
  - [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
  - Logs all EventBridge operations
  - Provides detailed audit trail
  - Supports compliance requirements
- Together, they provide secure EventBridge management
- Other options either lack encryption (B) or audit logging (C, D)

### Question 18
**Correct Answer: A - AWS KMS and AWS CloudTrail**

**Detailed Explanation:**
- KMS provides state machine encryption
  - [KMS Documentation](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)
  - Supports envelope encryption
  - Provides secure key storage
  - Enables automatic key rotation
- CloudTrail provides audit logging
  - [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
  - Logs all Step Functions operations
  - Provides detailed audit trail
  - Supports compliance requirements
- Together, they provide secure Step Functions management
- Other options either lack encryption (B) or audit logging (C, D)

### Question 19
**Correct Answer: A - AWS KMS and AWS CloudTrail**

**Detailed Explanation:**
- KMS provides log encryption
  - [KMS Documentation](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)
  - Supports envelope encryption
  - Provides secure key storage
  - Enables automatic key rotation
- CloudTrail provides audit logging
  - [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
  - Logs all CloudWatch operations
  - Provides detailed audit trail
  - Supports compliance requirements
- Together, they provide secure CloudWatch management
- Other options either lack encryption (B) or audit logging (C, D)

### Question 20
**Correct Answer: A - AWS KMS and AWS CloudTrail**

**Detailed Explanation:**
- KMS provides template encryption
  - [KMS Documentation](https://docs.aws.amazon.com/kms/latest/developerguide/overview.html)
  - Supports envelope encryption
  - Provides secure key storage
  - Enables automatic key rotation
- CloudTrail provides audit logging
  - [CloudTrail Documentation](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-user-guide.html)
  - Logs all CloudFormation operations
  - Provides detailed audit trail
  - Supports compliance requirements
- Together, they provide secure CloudFormation management
- Other options either lack encryption (B) or audit logging (C, D)

## Section 2: Design Resilient Architectures

### Question 1
**Correct Answer: B - AWS Lambda with Amazon API Gateway and Amazon CloudFront**

**Detailed Explanation:**
- Lambda provides automatic scaling and high availability
  - [Lambda Documentation](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
  - Supports automatic scaling
  - Provides high availability
  - Enables serverless architecture
- API Gateway manages requests and provides throttling
  - [API Gateway Documentation](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)
  - Provides request throttling
  - Enables API management
  - Supports caching
- CloudFront provides global distribution and caching
  - [CloudFront Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)
  - Provides global distribution
  - Enables edge caching
  - Supports dynamic content
- Together, they provide a highly available, globally distributed solution
- Other options either lack global distribution (A) or automatic scaling (C, D)

### Question 2
**Correct Answer: A - Amazon RDS with Multi-AZ**

**Detailed Explanation:**
- RDS Multi-AZ provides automatic failover
  - [RDS Multi-AZ Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)
  - Supports automatic failover
  - Provides synchronous replication
  - Ensures high availability
- Includes point-in-time recovery
  - [RDS Backup Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithAutomatedBackups.html)
  - Supports automated backups
  - Enables point-in-time recovery
  - Provides data durability
- Ensures 99.99% availability
  - [RDS SLA Documentation](https://aws.amazon.com/rds/sla/)
  - Provides high availability SLA
  - Supports automatic failover
  - Ensures data durability
- Other options either lack automatic failover (B) or point-in-time recovery (C, D)

### Question 3
**Correct Answer: B - Amazon RDS with Multi-AZ**

**Detailed Explanation:**
- RDS Multi-AZ meets the 4-hour RTO requirement
  - [RDS Multi-AZ Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)
  - Provides automatic failover
  - Supports synchronous replication
  - Ensures high availability
- Provides 1-hour RPO with point-in-time recovery
  - [RDS Backup Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithAutomatedBackups.html)
  - Supports automated backups
  - Enables point-in-time recovery
  - Provides data durability
- Offers cost-effective solution with minimal data loss
  - [RDS Pricing Documentation](https://aws.amazon.com/rds/pricing/)
  - Provides cost-effective high availability
  - Supports automatic failover
  - Ensures data durability
- Other options either have longer RTO (A) or higher cost (C, D)

### Question 4
**Correct Answer: B - Amazon Route 53 with geolocation routing and Amazon DynamoDB with global tables**

**Detailed Explanation:**
- Route 53 with geolocation routing provides multi-region deployment
  - [Route 53 Documentation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html)
  - Supports geolocation routing
  - Provides DNS management
  - Enables global distribution
- DynamoDB global tables provide automatic data replication
  - [DynamoDB Global Tables Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GlobalTables.html)
  - Supports multi-region replication
  - Provides automatic failover
  - Ensures data consistency
- Together, they provide a highly available, multi-region solution
- Other options either lack automatic data replication (A) or global distribution (C, D)

### Question 5
**Correct Answer: A - Amazon S3 with cross-region replication and lifecycle policies**

**Detailed Explanation:**
- Cross-region replication provides disaster recovery
  - [S3 Cross-Region Replication Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/replication.html)
  - Supports automatic replication
  - Provides disaster recovery
  - Ensures data durability
- Lifecycle policies optimize storage costs
  - [S3 Lifecycle Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)
  - Supports storage class transitions
  - Enables cost optimization
  - Provides automated management
- Together, they provide comprehensive S3 data protection
- Other options either lack cross-region replication (B) or lifecycle management (C, D)

### Question 6
**Correct Answer: A - Amazon EC2 with Application Load Balancer and Auto Scaling**

**Detailed Explanation:**
- EC2 with ALB provides session persistence
  - [EC2 Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
  - Supports session persistence
  - Provides compute capacity
  - Enables stateful applications
- Auto Scaling ensures high availability
  - [Auto Scaling Documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)
  - Supports automatic scaling
  - Provides high availability
  - Ensures optimal capacity
- Together, they provide a highly available solution with stateful components
- Other options either lack session persistence (B) or automatic scaling (C, D)

### Question 7
**Correct Answer: A - Amazon EBS snapshots with AWS Backup**

**Detailed Explanation:**
- EBS snapshots provide point-in-time recovery
  - [EBS Snapshots Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html)
  - Supports incremental backups
  - Provides point-in-time recovery
  - Ensures data durability
- AWS Backup provides automated backup scheduling
  - [AWS Backup Documentation](https://docs.aws.amazon.com/aws-backup/latest/devguide/what-is-backup.html)
  - Supports automated backups
  - Provides centralized management
  - Enables policy-based backup
- Together, they provide comprehensive EBS protection
- Other options either lack automated backup (B) or point-in-time recovery (C, D)

### Question 8
**Correct Answer: A - Amazon ECS with Application Load Balancer and AWS Cloud Map**

**Detailed Explanation:**
- ECS provides container orchestration
  - [ECS Documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)
  - Supports container management
  - Provides service discovery
  - Enables automatic scaling
- ALB provides load balancing
  - [ALB Documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)
  - Supports path-based routing
  - Provides health checks
  - Enables SSL termination
- Cloud Map provides service discovery
  - [Cloud Map Documentation](https://docs.aws.amazon.com/cloud-map/latest/dg/what-is-cloud-map.html)
  - Supports service registration
  - Provides DNS-based discovery
  - Enables dynamic updates
- Together, they provide a highly available microservices solution
- Other options either lack service discovery (B) or container orchestration (C, D)

### Question 9
**Correct Answer: A - Amazon DynamoDB with global tables**

**Detailed Explanation:**
- Global tables provide cross-region replication
  - [DynamoDB Global Tables Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GlobalTables.html)
  - Supports multi-region replication
  - Provides automatic failover
  - Ensures data consistency
- Includes automatic failover
  - [DynamoDB High Availability Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HighAvailability.html)
  - Supports automatic failover
  - Provides high availability
  - Ensures data durability
- Provides point-in-time recovery
  - [DynamoDB Backup Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/BackupRestore.html)
  - Supports point-in-time recovery
  - Provides automated backups
  - Ensures data protection
- Other options either lack automatic failover (B) or cross-region replication (C, D)

### Question 10
**Correct Answer: A - AWS Lambda with API Gateway and CloudWatch**

**Detailed Explanation:**
- Lambda provides automatic scaling
  - [Lambda Documentation](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
  - Supports automatic scaling
  - Provides high availability
  - Enables serverless architecture
- API Gateway provides load balancing
  - [API Gateway Documentation](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)
  - Supports request throttling
  - Provides API management
  - Enables caching
- CloudWatch provides health monitoring
  - [CloudWatch Documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)
  - Supports metrics collection
  - Provides alarms
  - Enables logging
- Together, they provide a highly available serverless solution
- Other options either lack automatic scaling (B) or health monitoring (C, D)

### Question 11
**Correct Answer: A - AWS Backup with EFS**

**Detailed Explanation:**
- AWS Backup provides automated backup scheduling
  - [AWS Backup Documentation](https://docs.aws.amazon.com/aws-backup/latest/devguide/what-is-backup.html)
  - Supports automated backups
  - Provides centralized management
  - Enables policy-based backup
- EFS provides scalable file storage
  - [EFS Documentation](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html)
  - Supports elastic scaling
  - Provides high availability
  - Ensures data durability
- Together, they provide comprehensive EFS protection
- Other options either lack automated backup (B) or point-in-time recovery (C, D)

### Question 12
**Correct Answer: A - Amazon ECS with Application Load Balancer and CloudWatch**

**Detailed Explanation:**
- ECS provides container orchestration
  - [ECS Documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)
  - Supports container management
  - Provides service discovery
  - Enables automatic scaling
- ALB provides load balancing
  - [ALB Documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)
  - Supports path-based routing
  - Provides health checks
  - Enables SSL termination
- CloudWatch provides health monitoring
  - [CloudWatch Documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)
  - Supports metrics collection
  - Provides alarms
  - Enables logging
- Together, they provide a highly available containerized solution
- Other options either lack container orchestration (B) or health monitoring (C, D)

### Question 13
**Correct Answer: A - Amazon RDS with Multi-AZ and read replicas**

**Detailed Explanation:**
- Multi-AZ provides automatic failover
  - [RDS Multi-AZ Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html)
  - Supports automatic failover
  - Provides synchronous replication
  - Ensures high availability
- Read replicas provide point-in-time recovery
  - [RDS Read Replicas Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html)
  - Supports read scaling
  - Provides point-in-time recovery
  - Enables disaster recovery
- Together, they provide comprehensive RDS protection
- Other options either lack automatic failover (B) or point-in-time recovery (C, D)

### Question 14
**Correct Answer: A - AWS Lambda with API Gateway and CloudWatch**

**Detailed Explanation:**
- Lambda provides automatic scaling
  - [Lambda Documentation](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)
  - Supports automatic scaling
  - Provides high availability
  - Enables serverless architecture
- API Gateway provides load balancing
  - [API Gateway Documentation](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)
  - Supports request throttling
  - Provides API management
  - Enables caching
- CloudWatch provides health monitoring
  - [CloudWatch Documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)
  - Supports metrics collection
  - Provides alarms
  - Enables logging
- Together, they provide a highly available serverless solution
- Other options either lack automatic scaling (B) or health monitoring (C, D)

### Question 15
**Correct Answer: A - Amazon DynamoDB with global tables**

**Detailed Explanation:**
- Global tables provide automated backup
  - [DynamoDB Global Tables Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GlobalTables.html)
  - Supports multi-region replication
  - Provides automatic failover
  - Ensures data consistency
- Includes point-in-time recovery
  - [DynamoDB Backup Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/BackupRestore.html)
  - Supports point-in-time recovery
  - Provides automated backups
  - Ensures data protection
- Provides cross-region replication
  - [DynamoDB High Availability Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HighAvailability.html)
  - Supports automatic failover
  - Provides high availability
  - Ensures data durability
- Other options either lack automated backup (B) or cross-region replication (C, D)

### Question 16
**Correct Answer: A - Amazon ECS with Application Load Balancer and AWS Cloud Map**

**Detailed Explanation:**
- ECS provides container orchestration
  - [ECS Documentation](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html)
  - Supports container management
  - Provides service discovery
  - Enables automatic scaling
- ALB provides load balancing
  - [ALB Documentation](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html)
  - Supports path-based routing
  - Provides health checks
  - Enables SSL termination
- Cloud Map provides service discovery
  - [Cloud Map Documentation](https://docs.aws.amazon.com/cloud-map/latest/dg/what-is-cloud-map.html)
  - Supports service registration
  - Provides DNS-based discovery
  - Enables dynamic updates
- Together, they provide a highly available microservices solution
- Other options either lack service discovery (B) or container orchestration (C, D)

## Section 3: Design High-Performing Architectures

### Question 1
**Correct Answer: A - Amazon CloudFront with Lambda@Edge**

**Detailed Explanation:**
- CloudFront provides global distribution and edge caching
  - [CloudFront Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)
  - Supports global distribution
  - Provides edge caching
  - Enables dynamic content
- Lambda@Edge enables dynamic content processing at the edge
  - [Lambda@Edge Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-at-the-edge.html)
  - Supports edge processing
  - Provides dynamic content handling
  - Enables custom logic
- Together, they provide low latency for global users
- Other options either lack edge processing (B) or global distribution (C, D)

### Question 2
**Correct Answer: A - Amazon DynamoDB**

**Detailed Explanation:**
- DynamoDB provides sub-millisecond latency
  - [DynamoDB Performance Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.Performance.html)
  - Supports single-digit millisecond latency
  - Provides consistent performance
  - Enables automatic scaling
- Includes automatic scaling
  - [DynamoDB Auto Scaling Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/AutoScaling.html)
  - Supports automatic scaling
  - Provides throughput management
  - Ensures optimal performance
- Offers global distribution
  - [DynamoDB Global Tables Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GlobalTables.html)
  - Supports multi-region replication
  - Provides global distribution
  - Ensures low latency
- Other options either have higher latency (B) or lack automatic scaling (C, D)

### Question 3
**Correct Answer: A - Amazon CloudFront with Lambda@Edge**

**Detailed Explanation:**
- CloudFront provides edge caching
  - [CloudFront Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)
  - Supports edge caching
  - Provides global distribution
  - Enables dynamic content
- Lambda@Edge enables dynamic content processing
  - [Lambda@Edge Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/lambda-at-the-edge.html)
  - Supports edge processing
  - Provides dynamic content handling
  - Enables custom logic
- Together, they provide optimized content delivery
- Other options either lack edge processing (B) or dynamic content handling (C, D)

### Question 4
**Correct Answer: A - Amazon Aurora with read replicas and Multi-AZ**

**Detailed Explanation:**
- Read replicas provide read scaling
  - [Aurora Read Replicas Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Replication.html)
  - Supports read scaling
  - Provides high availability
  - Enables disaster recovery
- Multi-AZ provides automatic failover
  - [Aurora Multi-AZ Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Overview.html)
  - Supports automatic failover
  - Provides synchronous replication
  - Ensures high availability
- Together, they provide high database performance
- Other options either lack read scaling (B) or automatic failover (C, D)

### Question 5
**Correct Answer: A - Amazon CloudFront with AWS Global Accelerator**

**Detailed Explanation:**
- CloudFront provides global distribution
  - [CloudFront Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)
  - Supports global distribution
  - Provides edge caching
  - Enables dynamic content
- Global Accelerator provides low latency routing
  - [Global Accelerator Documentation](https://docs.aws.amazon.com/global-accelerator/latest/dg/what-is-global-accelerator.html)
  - Supports low latency routing
  - Provides global distribution
  - Enables traffic management
- Together, they provide optimized network performance
- Other options either lack global distribution (B) or low latency routing (C, D)

### Question 6
**Correct Answer: A - Amazon EC2 with Auto Scaling and CloudWatch**

**Detailed Explanation:**
- EC2 provides high CPU utilization
  - [EC2 Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
  - Supports high CPU utilization
  - Provides compute capacity
  - Enables performance optimization
- Auto Scaling ensures optimal resource usage
  - [Auto Scaling Documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)
  - Supports automatic scaling
  - Provides resource optimization
  - Ensures high availability
- CloudWatch provides performance monitoring
  - [CloudWatch Documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)
  - Supports metrics collection
  - Provides performance monitoring
  - Enables resource optimization
- Together, they provide optimized compute performance
- Other options either lack high CPU utilization (B) or performance monitoring (C, D)

### Question 7
**Correct Answer: A - Amazon EBS with Provisioned IOPS**

**Detailed Explanation:**
- Provisioned IOPS provides high IOPS
  - [EBS Provisioned IOPS Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html)
  - Supports high IOPS
  - Provides consistent performance
  - Enables performance optimization
- Ensures low latency
  - [EBS Performance Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-optimized.html)
  - Supports low latency
  - Provides consistent performance
  - Enables high throughput
- Provides consistent performance
  - [EBS Performance Optimization](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSPerformance.html)
  - Supports consistent performance
  - Provides high throughput
  - Enables performance optimization
- Other options either have lower IOPS (B) or higher latency (C, D)

### Question 8
**Correct Answer: A - Amazon CloudFront with AWS Global Accelerator**

**Detailed Explanation:**
- CloudFront provides global distribution
  - [CloudFront Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)
  - Supports global distribution
  - Provides edge caching
  - Enables dynamic content
- Global Accelerator provides low latency routing
  - [Global Accelerator Documentation](https://docs.aws.amazon.com/global-accelerator/latest/dg/what-is-global-accelerator.html)
  - Supports low latency routing
  - Provides global distribution
  - Enables traffic management
- Together, they provide optimized network performance
- Other options either lack global distribution (B) or low latency routing (C, D)

### Question 9
**Correct Answer: A - Amazon Aurora with read replicas and Multi-AZ**

**Detailed Explanation:**
- Read replicas provide read scaling
  - [Aurora Read Replicas Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Replication.html)
  - Supports read scaling
  - Provides high availability
  - Enables disaster recovery
- Multi-AZ provides automatic failover
  - [Aurora Multi-AZ Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Overview.html)
  - Supports automatic failover
  - Provides synchronous replication
  - Ensures high availability
- Together, they provide high database performance
- Other options either lack read scaling (B) or automatic failover (C, D)

### Question 10
**Correct Answer: A - Amazon EC2 with Auto Scaling and CloudWatch**

**Detailed Explanation:**
- EC2 provides high CPU utilization
  - [EC2 Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
  - Supports high CPU utilization
  - Provides compute capacity
  - Enables performance optimization
- Auto Scaling ensures optimal resource usage
  - [Auto Scaling Documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)
  - Supports automatic scaling
  - Provides resource optimization
  - Ensures high availability
- CloudWatch provides performance monitoring
  - [CloudWatch Documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)
  - Supports metrics collection
  - Provides performance monitoring
  - Enables resource optimization
- Together, they provide optimized compute performance
- Other options either lack high CPU utilization (B) or performance monitoring (C, D)

### Question 11
**Correct Answer: A - Amazon EBS with Provisioned IOPS**

**Detailed Explanation:**
- Provisioned IOPS provides high IOPS
  - [EBS Provisioned IOPS Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html)
  - Supports high IOPS
  - Provides consistent performance
  - Enables performance optimization
- Ensures low latency
  - [EBS Performance Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-optimized.html)
  - Supports low latency
  - Provides consistent performance
  - Enables high throughput
- Provides consistent performance
  - [EBS Performance Optimization](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSPerformance.html)
  - Supports consistent performance
  - Provides high throughput
  - Enables performance optimization
- Other options either have lower IOPS (B) or higher latency (C, D)

### Question 12
**Correct Answer: A - Amazon EC2 with Auto Scaling and CloudWatch**

**Detailed Explanation:**
- EC2 provides high CPU utilization
  - [EC2 Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
  - Supports high CPU utilization
  - Provides compute capacity
  - Enables performance optimization
- Auto Scaling ensures optimal resource usage
  - [Auto Scaling Documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)
  - Supports automatic scaling
  - Provides resource optimization
  - Ensures high availability
- CloudWatch provides performance monitoring
  - [CloudWatch Documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)
  - Supports metrics collection
  - Provides performance monitoring
  - Enables resource optimization
- Together, they provide optimized compute performance
- Other options either lack high CPU utilization (B) or performance monitoring (C, D)

### Question 13
**Correct Answer: A - Amazon EBS with Provisioned IOPS**

**Detailed Explanation:**
- Provisioned IOPS provides high IOPS
  - [EBS Provisioned IOPS Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html)
  - Supports high IOPS
  - Provides consistent performance
  - Enables performance optimization
- Ensures low latency
  - [EBS Performance Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-optimized.html)
  - Supports low latency
  - Provides consistent performance
  - Enables high throughput
- Provides consistent performance
  - [EBS Performance Optimization](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSPerformance.html)
  - Supports consistent performance
  - Provides high throughput
  - Enables performance optimization
- Other options either have lower IOPS (B) or higher latency (C, D)

### Question 14
**Correct Answer: A - Amazon EC2 with Auto Scaling and CloudWatch**

**Detailed Explanation:**
- EC2 provides high CPU utilization
  - [EC2 Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
  - Supports high CPU utilization
  - Provides compute capacity
  - Enables performance optimization
- Auto Scaling ensures optimal resource usage
  - [Auto Scaling Documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)
  - Supports automatic scaling
  - Provides resource optimization
  - Ensures high availability
- CloudWatch provides performance monitoring
  - [CloudWatch Documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/WhatIsCloudWatch.html)
  - Supports metrics collection
  - Provides performance monitoring
  - Enables resource optimization
- Together, they provide optimized compute performance
- Other options either lack high CPU utilization (B) or performance monitoring (C, D)

### Question 15
**Correct Answer: A - Amazon EBS with Provisioned IOPS**

**Detailed Explanation:**
- Provisioned IOPS provides high IOPS
  - [EBS Provisioned IOPS Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html)
  - Supports high IOPS
  - Provides consistent performance
  - Enables performance optimization
- Ensures low latency
  - [EBS Performance Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-optimized.html)
  - Supports low latency
  - Provides consistent performance
  - Enables high throughput
- Provides consistent performance
  - [EBS Performance Optimization](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSPerformance.html)
  - Supports consistent performance
  - Provides high throughput
  - Enables performance optimization
- Other options either have lower IOPS (B) or higher latency (C, D)

### Question 16
**Correct Answer: A - Amazon CloudFront with AWS Global Accelerator**

**Detailed Explanation:**
- CloudFront provides global distribution
  - [CloudFront Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)
  - Supports global distribution
  - Provides edge caching
  - Enables dynamic content
- Global Accelerator provides low latency routing
  - [Global Accelerator Documentation](https://docs.aws.amazon.com/global-accelerator/latest/dg/what-is-global-accelerator.html)
  - Supports low latency routing
  - Provides global distribution
  - Enables traffic management
- Together, they provide optimized network performance
- Other options either lack global distribution (B) or low latency routing (C, D)

## Section 4: Design Cost-Optimized Architectures

### Question 1
**Correct Answer: A - Amazon EC2 Spot Instances with Auto Scaling**

**Detailed Explanation:**
- Spot Instances provide lowest cost for batch processing
  - [EC2 Spot Instances Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html)
  - Supports up to 90% cost savings
  - Provides flexible pricing
  - Enables cost optimization
- Auto Scaling manages variable workloads
  - [Auto Scaling Documentation](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html)
  - Supports automatic scaling
  - Provides workload management
  - Ensures optimal capacity
- Together, they provide cost-effective compute
- Other options either have higher costs (B) or lack workload management (C, D)

### Question 2
**Correct Answer: A - Amazon S3 with S3 Intelligent-Tiering**

**Detailed Explanation:**
- S3 Intelligent-Tiering automatically optimizes costs
  - [S3 Intelligent-Tiering Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html#sc-intelligent-tiering)
  - Supports automatic tiering
  - Provides cost optimization
  - Enables storage management
- Adapts to changing access patterns
  - [S3 Storage Classes Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html)
  - Supports access pattern analysis
  - Provides automatic optimization
  - Enables cost savings
- Provides cost-effective storage
  - [S3 Pricing Documentation](https://aws.amazon.com/s3/pricing/)
  - Supports tiered pricing
  - Provides cost optimization
  - Enables storage management
- Other options either lack automatic optimization (B) or have higher costs (C, D)

### Question 3
**Correct Answer: D - AWS Batch with Spot Instances**

**Detailed Explanation:**
- Batch provides workload management
  - [AWS Batch Documentation](https://docs.aws.amazon.com/batch/latest/userguide/what-is-batch.html)
  - Supports job scheduling
  - Provides resource management
  - Enables workload optimization
- Spot Instances provide lowest cost
  - [EC2 Spot Instances Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html)
  - Supports up to 90% cost savings
  - Provides flexible pricing
  - Enables cost optimization
- Together, they provide cost-effective batch processing
- Other options either have higher costs (A) or lack workload management (B, C)

### Question 4
**Correct Answer: A - Amazon S3 with S3 Glacier Deep Archive**

**Detailed Explanation:**
- Glacier Deep Archive provides lowest cost storage
  - [S3 Glacier Deep Archive Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html#sc-glacier)
  - Supports lowest cost storage
  - Provides long-term archival
  - Enables cost optimization
- Suitable for long-term archival
  - [S3 Storage Classes Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html)
  - Supports archival storage
  - Provides data durability
  - Enables cost savings
- Provides data durability
  - [S3 Durability Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/DataDurability.html)
  - Supports 99.999999999% durability
  - Provides data protection
  - Ensures data availability
- Other options either have higher costs (B) or are not designed for archival (C, D)

### Question 5
**Correct Answer: A - Amazon CloudFront with AWS Global Accelerator**

**Detailed Explanation:**
- CloudFront provides cost-effective global distribution
  - [CloudFront Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)
  - Supports global distribution
  - Provides edge caching
  - Enables cost optimization
- Global Accelerator optimizes routing
  - [Global Accelerator Documentation](https://docs.aws.amazon.com/global-accelerator/latest/dg/what-is-global-accelerator.html)
  - Supports low latency routing
  - Provides traffic management
  - Enables cost optimization
- Together, they provide cost-effective network
- Other options either have higher costs (B) or lack global distribution (C, D)

### Question 6
**Correct Answer: A - Amazon Aurora Serverless**

**Detailed Explanation:**
- Aurora Serverless automatically scales
  - [Aurora Serverless Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless.html)
  - Supports automatic scaling
  - Provides cost optimization
  - Enables workload management
- Optimizes costs based on demand
  - [Aurora Serverless Pricing](https://aws.amazon.com/rds/aurora/pricing/)
  - Supports pay-per-use pricing
  - Provides cost optimization
  - Enables automatic scaling
- Provides high availability
  - [Aurora Serverless Features](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless.how-it-works.html)
  - Supports automatic failover
  - Provides high availability
  - Ensures data durability
- Other options either have fixed costs (B) or lack automatic scaling (C, D)

### Question 7
**Correct Answer: D - AWS Batch with Spot Instances**

**Detailed Explanation:**
- Batch provides workload management
  - [AWS Batch Documentation](https://docs.aws.amazon.com/batch/latest/userguide/what-is-batch.html)
  - Supports job scheduling
  - Provides resource management
  - Enables workload optimization
- Spot Instances provide lowest cost
  - [EC2 Spot Instances Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html)
  - Supports up to 90% cost savings
  - Provides flexible pricing
  - Enables cost optimization
- Together, they provide cost-effective batch processing
- Other options either have higher costs (A) or lack workload management (B, C)

### Question 8
**Correct Answer: A - Amazon S3 with S3 Glacier Deep Archive**

**Detailed Explanation:**
- Glacier Deep Archive provides lowest cost storage
  - [S3 Glacier Deep Archive Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html#sc-glacier)
  - Supports lowest cost storage
  - Provides long-term archival
  - Enables cost optimization
- Suitable for long-term archival
  - [S3 Storage Classes Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html)
  - Supports archival storage
  - Provides data durability
  - Enables cost savings
- Provides data durability
  - [S3 Durability Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/DataDurability.html)
  - Supports 99.999999999% durability
  - Provides data protection
  - Ensures data availability
- Other options either have higher costs (B) or are not designed for archival (C, D)

### Question 9
**Correct Answer: A - Amazon CloudFront with AWS Global Accelerator**

**Detailed Explanation:**
- CloudFront provides cost-effective global distribution
  - [CloudFront Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)
  - Supports global distribution
  - Provides edge caching
  - Enables cost optimization
- Global Accelerator optimizes routing
  - [Global Accelerator Documentation](https://docs.aws.amazon.com/global-accelerator/latest/dg/what-is-global-accelerator.html)
  - Supports low latency routing
  - Provides traffic management
  - Enables cost optimization
- Together, they provide cost-effective network
- Other options either have higher costs (B) or lack global distribution (C, D)

### Question 10
**Correct Answer: A - Amazon Aurora Serverless**

**Detailed Explanation:**
- Aurora Serverless automatically scales
  - [Aurora Serverless Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless.html)
  - Supports automatic scaling
  - Provides cost optimization
  - Enables workload management
- Optimizes costs based on demand
  - [Aurora Serverless Pricing](https://aws.amazon.com/rds/aurora/pricing/)
  - Supports pay-per-use pricing
  - Provides cost optimization
  - Enables automatic scaling
- Provides high availability
  - [Aurora Serverless Features](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless.how-it-works.html)
  - Supports automatic failover
  - Provides high availability
  - Ensures data durability
- Other options either have fixed costs (B) or lack automatic scaling (C, D)

### Question 11
**Correct Answer: D - AWS Batch with Spot Instances**

**Detailed Explanation:**
- Batch provides workload management
  - [AWS Batch Documentation](https://docs.aws.amazon.com/batch/latest/userguide/what-is-batch.html)
  - Supports job scheduling
  - Provides resource management
  - Enables workload optimization
- Spot Instances provide lowest cost
  - [EC2 Spot Instances Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html)
  - Supports up to 90% cost savings
  - Provides flexible pricing
  - Enables cost optimization
- Together, they provide cost-effective batch processing
- Other options either have higher costs (A) or lack workload management (B, C)

### Question 12
**Correct Answer: A - Amazon S3 with S3 Glacier Deep Archive**

**Detailed Explanation:**
- Glacier Deep Archive provides lowest cost storage
  - [S3 Glacier Deep Archive Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html#sc-glacier)
  - Supports lowest cost storage
  - Provides long-term archival
  - Enables cost optimization
- Suitable for long-term archival
  - [S3 Storage Classes Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html)
  - Supports archival storage
  - Provides data durability
  - Enables cost savings
- Provides data durability
  - [S3 Durability Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/DataDurability.html)
  - Supports 99.999999999% durability
  - Provides data protection
  - Ensures data availability
- Other options either have higher costs (B) or are not designed for archival (C, D)

### Question 13
**Correct Answer: A - Amazon CloudFront with AWS Global Accelerator**

**Detailed Explanation:**
- CloudFront provides cost-effective global distribution
  - [CloudFront Documentation](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html)
  - Supports global distribution
  - Provides edge caching
  - Enables cost optimization
- Global Accelerator optimizes routing
  - [Global Accelerator Documentation](https://docs.aws.amazon.com/global-accelerator/latest/dg/what-is-global-accelerator.html)
  - Supports low latency routing
  - Provides traffic management
  - Enables cost optimization
- Together, they provide cost-effective network
- Other options either have higher costs (B) or lack global distribution (C, D)

### Question 14
**Correct Answer: A - Amazon Aurora Serverless**

**Detailed Explanation:**
- Aurora Serverless automatically scales
  - [Aurora Serverless Documentation](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless.html)
  - Supports automatic scaling
  - Provides cost optimization
  - Enables workload management
- Optimizes costs based on demand
  - [Aurora Serverless Pricing](https://aws.amazon.com/rds/aurora/pricing/)
  - Supports pay-per-use pricing
  - Provides cost optimization
  - Enables automatic scaling
- Provides high availability
  - [Aurora Serverless Features](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/aurora-serverless.how-it-works.html)
  - Supports automatic failover
  - Provides high availability
  - Ensures data durability
- Other options either have fixed costs (B) or lack automatic scaling (C, D)

### Question 15
**Correct Answer: D - AWS Batch with Spot Instances**

**Detailed Explanation:**
- Batch provides workload management
  - [AWS Batch Documentation](https://docs.aws.amazon.com/batch/latest/userguide/what-is-batch.html)
  - Supports job scheduling
  - Provides resource management
  - Enables workload optimization
- Spot Instances provide lowest cost
  - [EC2 Spot Instances Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html)
  - Supports up to 90% cost savings
  - Provides flexible pricing
  - Enables cost optimization
- Together, they provide cost-effective batch processing
- Other options either have higher costs (A) or lack workload management (B, C)

### Question 16
**Correct Answer: A - Amazon S3 with S3 Glacier Deep Archive**

**Detailed Explanation:**
- Glacier Deep Archive provides lowest cost storage
  - [S3 Glacier Deep Archive Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html#sc-glacier)
  - Supports lowest cost storage
  - Provides long-term archival
  - Enables cost optimization
- Suitable for long-term archival
  - [S3 Storage Classes Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html)
  - Supports archival storage
  - Provides data durability
  - Enables cost savings
- Provides data durability
  - [S3 Durability Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/DataDurability.html)
  - Supports 99.999999999% durability
  - Provides data protection
  - Ensures data availability
- Other options either have higher costs (B) or are not designed for archival (C, D)

