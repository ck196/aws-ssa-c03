# AWS Certified Solutions Architect - Question Banks

This document contains practice questions organized by exam domains to help you prepare for the AWS Certified Solutions Architect - Associate (SAA-C03) exam.

## Domain 1: Design Secure Architectures

### Question Bank 1: IAM and Access Management

**Question 1:**
A company needs to implement secure access to their AWS resources. They have:
- Multiple AWS accounts
- Need for centralized access management
- Requirement for audit logging
- Need for least privilege access

Which AWS service combination would best meet these requirements?

A) AWS IAM and AWS CloudTrail
B) AWS Organizations and AWS IAM
C) AWS IAM Identity Center and AWS CloudTrail
D) AWS Control Tower and AWS CloudTrail

**Correct Answer: C**

**Explanation:**
- IAM Identity Center provides centralized access management
- CloudTrail enables audit logging
- This combination supports least privilege access
- Better suited than Organizations or Control Tower for access management

**Question 2:**
A company needs to implement a secure password policy. Requirements:
- Minimum password length of 12 characters
- Require at least one number
- Require at least one special character
- Password expiration every 90 days

Which AWS service would best meet these requirements?

A) AWS IAM Password Policy
B) AWS Secrets Manager
C) AWS KMS
D) AWS SSM Parameter Store

**Correct Answer: A**

**Explanation:**
- IAM Password Policy allows setting password requirements
- Can enforce minimum length and character types
- Supports password expiration
- No additional cost for basic password policy

### Question Bank 2: Data Protection

**Question 1:**
A company needs to secure their S3 buckets. They require:
- Encryption at rest
- Encryption in transit
- Access logging
- Versioning

Which combination of S3 features would best meet these requirements?

A) SSE-S3, SSL/TLS, S3 Access Logs, Versioning
B) SSE-KMS, SSL/TLS, CloudTrail, Versioning
C) SSE-S3, SSL/TLS, CloudTrail, Versioning
D) SSE-KMS, SSL/TLS, S3 Access Logs, Versioning

**Correct Answer: D**

**Explanation:**
- SSE-KMS provides encryption at rest
- SSL/TLS provides encryption in transit
- S3 Access Logs track bucket access
- Versioning maintains object versions

**Question 2:**
A company needs to implement encryption for their RDS database. Requirements:
- Encryption at rest
- Encryption in transit
- Key rotation
- Audit logging

Which AWS service combination would best meet these requirements?

A) AWS KMS and SSL/TLS
B) AWS Secrets Manager and SSL/TLS
C) AWS KMS and AWS Certificate Manager
D) AWS Secrets Manager and AWS Certificate Manager

**Correct Answer: A**

**Explanation:**
- KMS provides encryption at rest with key rotation
- SSL/TLS provides encryption in transit
- This combination supports audit logging
- Most cost-effective solution

## Domain 2: Design Resilient Architectures

### Question Bank 1: High Availability

**Question 1:**
A company needs to implement a highly available web application. Requirements:
- 99.99% availability
- Automatic scaling
- Global distribution
- Cost optimization

Which AWS service combination would best meet these requirements?

A) Amazon EC2 with Auto Scaling and Application Load Balancer
B) AWS Lambda with Amazon API Gateway and Amazon CloudFront
C) Amazon ECS with Application Load Balancer and Amazon Route 53
D) Amazon EC2 with Network Load Balancer and Amazon Route 53

**Correct Answer: B**

**Explanation:**
- Lambda provides automatic scaling and high availability
- API Gateway manages API requests
- CloudFront provides global distribution
- This serverless architecture is more cost-effective

**Question 2:**
A company needs to implement a highly available database solution. Requirements:
- 99.99% availability
- Automatic failover
- Point-in-time recovery
- Multi-AZ deployment

Which AWS service would best meet these requirements?

A) Amazon RDS with Multi-AZ
B) Amazon DynamoDB
C) Amazon Aurora
D) Amazon ElastiCache

**Correct Answer: A**

**Explanation:**
- RDS Multi-AZ provides automatic failover
- Supports point-in-time recovery
- Can achieve 99.99% availability
- Better suited than DynamoDB for relational data

### Question Bank 2: Disaster Recovery

**Question 1:**
A company needs to implement disaster recovery. Requirements:
- RTO of 4 hours
- RPO of 1 hour
- Cost-effective solution
- Minimal data loss

Which AWS service combination would best meet these requirements?

A) Amazon RDS with read replicas
B) Amazon RDS with Multi-AZ
C) Amazon DynamoDB with global tables
D) Amazon Aurora with cross-region replication

**Correct Answer: B**

**Explanation:**
- RDS Multi-AZ meets RTO and RPO requirements
- More cost-effective than cross-region solutions
- Provides automatic failover
- Suitable for most disaster recovery scenarios

**Question 2:**
A company needs to implement a multi-region disaster recovery solution. Requirements:
- RTO of 1 hour
- RPO of 15 minutes
- Automated failover
- Cost optimization

Which AWS service combination would best meet these requirements?

A) Amazon RDS with cross-region read replicas
B) Amazon Aurora with global database
C) Amazon DynamoDB with global tables
D) Amazon ElastiCache with replication

**Correct Answer: C**

**Explanation:**
- DynamoDB global tables provide automatic replication
- Can achieve RTO of 1 hour
- Supports RPO of 15 minutes
- Cost-effective for global distribution

## Domain 3: Design High-Performing Architectures

### Question Bank 1: Performance Optimization

**Question 1:**
A company needs to optimize their application's performance. Requirements:
- Global user base
- Dynamic content
- Low latency
- Cost-effective solution

Which AWS service combination would best meet these requirements?

A) Amazon CloudFront with Lambda@Edge
B) Amazon Route 53 with Application Load Balancer
C) AWS Global Accelerator with Network Load Balancer
D) Amazon CloudFront with Application Load Balancer

**Correct Answer: A**

**Explanation:**
- CloudFront provides global content delivery
- Lambda@Edge enables dynamic content processing
- This combination provides lowest latency
- Cost-effective for both static and dynamic content

**Question 2:**
A company needs to implement a high-performance database solution. Requirements:
- Sub-millisecond latency
- High throughput
- Automatic scaling
- Global distribution

Which AWS service would best meet these requirements?

A) Amazon DynamoDB
B) Amazon Aurora
C) Amazon RDS
D) Amazon ElastiCache

**Correct Answer: A**

**Explanation:**
- DynamoDB provides single-digit millisecond latency
- Offers automatic scaling
- Supports high throughput
- Global tables enable global distribution

### Question Bank 2: Caching and Content Delivery

**Question 1:**
A company needs to implement caching for their web application. Requirements:
- Edge caching
- Dynamic content caching
- Cost optimization
- Global distribution

Which AWS service combination would best meet these requirements?

A) Amazon CloudFront with Lambda@Edge
B) Amazon ElastiCache with Application Load Balancer
C) Amazon CloudFront with Application Load Balancer
D) Amazon ElastiCache with CloudFront

**Correct Answer: A**

**Explanation:**
- CloudFront provides edge caching
- Lambda@Edge enables dynamic content processing
- This combination is cost-effective
- Provides global distribution

**Question 2:**
A company needs to implement caching for their database. Requirements:
- Sub-millisecond latency
- High throughput
- Automatic scaling
- Cost optimization

Which AWS service would best meet these requirements?

A) Amazon ElastiCache
B) Amazon DynamoDB Accelerator (DAX)
C) Amazon RDS Proxy
D) Amazon MemoryDB

**Correct Answer: B**

**Explanation:**
- DAX provides sub-millisecond latency
- Automatically scales with DynamoDB
- Cost-effective for high throughput
- Seamless integration with DynamoDB

## Domain 4: Design Cost-Optimized Architectures

### Question Bank 1: Compute Optimization

**Question 1:**
A company needs to optimize their compute costs. They have:
- Batch processing jobs
- Variable workloads
- Some predictable traffic
- Need for cost-effectiveness

Which AWS service combination would best meet these requirements?

A) Amazon EC2 Spot Instances with Auto Scaling
B) AWS Lambda with Amazon EventBridge
C) Amazon EC2 Reserved Instances with Auto Scaling
D) AWS Fargate with Amazon EventBridge

**Correct Answer: A**

**Explanation:**
- Spot Instances provide lowest cost for batch processing
- Auto Scaling manages variable workloads
- More cost-effective than Lambda for long-running jobs
- Better suited for batch processing than Fargate

**Question 2:**
A company needs to optimize their serverless costs. They have:
- Variable workloads
- Need for cost predictability
- Some consistent usage
- Need for automatic scaling

Which AWS service combination would best meet these requirements?

A) AWS Lambda with Provisioned Concurrency
B) AWS Lambda with Auto Scaling
C) AWS Fargate with Auto Scaling
D) AWS App Runner with Auto Scaling

**Correct Answer: A**

**Explanation:**
- Provisioned Concurrency provides cost predictability
- Lambda automatically scales
- More cost-effective for consistent usage
- Better suited than Fargate for serverless workloads

### Question Bank 2: Storage Optimization

**Question 1:**
A company needs to optimize their storage costs. They have:
- Large amounts of data
- Varying access patterns
- Need for cost optimization
- Some data archival requirements

Which AWS service combination would best meet these requirements?

A) Amazon S3 with S3 Intelligent-Tiering
B) Amazon EFS with lifecycle policies
C) Amazon S3 with S3 Lifecycle policies
D) Amazon EBS with snapshot lifecycle

**Correct Answer: A**

**Explanation:**
- S3 Intelligent-Tiering automatically optimizes costs
- No retrieval fees for changing access patterns
- More cost-effective than EFS for object storage
- Better suited for varying access patterns

**Question 2:**
A company needs to optimize their database storage costs. They have:
- Large amounts of historical data
- Infrequent access to old data
- Need for cost optimization
- Some data archival requirements

Which AWS service combination would best meet these requirements?

A) Amazon RDS with storage autoscaling
B) Amazon Aurora with storage autoscaling
C) Amazon DynamoDB with TTL
D) Amazon RDS with read replicas

**Correct Answer: A**

**Explanation:**
- RDS storage autoscaling optimizes costs
- Can archive old data
- More cost-effective than read replicas
- Better suited for historical data

## Answer Key

### Domain 1: Design Secure Architectures
1. C
2. A
3. D
4. A

### Domain 2: Design Resilient Architectures
1. B
2. A
3. B
4. C

### Domain 3: Design High-Performing Architectures
1. A
2. A
3. A
4. B

### Domain 4: Design Cost-Optimized Architectures
1. A
2. A
3. A
4. A

Remember to:
1. Review explanations carefully
2. Understand why other options are incorrect
3. Practice with real AWS services
4. Take notes on important concepts
5. Review AWS documentation for details 