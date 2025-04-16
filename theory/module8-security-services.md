# Module 8: Security Services

## AWS Shield

DDoS protection service safeguarding applications running on AWS:

### Shield Standard

1. **Features**
   - Automatic protection for all AWS customers at no additional charge
   - Protects against common, frequently occurring network and transport layer DDoS attacks
   - Integrated with services like CloudFront and Route 53

2. **Protection Scope**
   - Layer 3 (Network layer) attacks: UDP floods, reflection attacks
   - Layer 4 (Transport layer) attacks: SYN floods, TCP floods

3. **Benefits**
   - Always-on detection and automatic inline mitigations
   - No need to engage AWS Support to benefit from protection
   - Seamless integration with AWS services

### Shield Advanced

1. **Features**
   - Enhanced protection for higher levels of defense
   - Additional protection for EC2, ELB, CloudFront, Global Accelerator, Route 53
   - Access to 24x7 DDoS Response Team (DRT)
   - Real-time attack visibility
   - Cost protection for scaling during DDoS attacks

2. **Protection Scope**
   - All protections in Shield Standard
   - Layer 7 (Application layer) attacks: HTTP floods, cache-busting attacks
   - Protection against larger and more sophisticated attacks

3. **Benefits**
   - Proactive engagement and custom mitigations from DRT
   - Integration with AWS WAF at no additional cost
   - Financial protection from scaling costs during attacks
   - Enhanced monitoring and reporting

## AWS WAF (Web Application Firewall)

Protects web applications from common web exploits:

### Key Concepts

1. **Web ACLs (Access Control Lists)**
   - Main resource for defining protection rules
   - Applied to CloudFront, API Gateway, ALB, AppSync
   - Can contain multiple rules and rule groups

2. **Rules**
   - Inspects web requests
   - Basic rule statements: IP sets, regex pattern sets, size constraints
   - Advanced rule statements: SQL injection, cross-site scripting (XSS)
   - Rate-based rules: Limit request volume from IP addresses

3. **Rule Actions**
   - Allow: Let the request through
   - Block: Deny the request
   - Count: Allow but count the request for metrics
   - CAPTCHA: Present CAPTCHA challenge
   - Challenge: Present silent browser challenge

4. **Rule Groups**
   - Reusable collection of rules
   - AWS Managed Rules: Pre-configured protection for common threats
   - AWS Marketplace Rules: Third-party security vendor rules
   - Custom Rule Groups: Your own rule sets

### WAF Features

1. **Traffic Visibility**
   - Full logging of all inspected web requests
   - Logs to CloudWatch, S3, or Firehose
   - Visibility into source IPs, request parameters, matched rules

2. **Bot Control**
   - Block, throttle, or verify common and pervasive bots
   - Machine learning to detect bot signatures
   - Verified bots allowlist (e.g., Google Bot, Bing Bot)

3. **Fraud Control**
   - Account takeover prevention (ATP)
   - Credential stuffing protection
   - Detect and verify abnormal login attempts

4. **Application Integration**
   - CloudFront
   - Application Load Balancer
   - API Gateway
   - AWS AppSync
   - Cognito User Pools

### Common Use Cases

1. Block common attack patterns (OWASP Top 10)
2. Filter specific traffic patterns
3. Rate limiting to prevent brute force attacks
4. Bot mitigation
5. Prevent account takeover attacks

## AWS Secrets Manager

Service for securely storing and managing secrets:

### Key Features

1. **Secret Storage**
   - Centralized storage for database credentials, API keys, OAuth tokens
   - Encryption using AWS KMS
   - Automatic generation of random secrets

2. **Automatic Rotation**
   - Schedule rotation for supported secrets
   - Built-in rotation for Amazon RDS, DocumentDB, Redshift
   - Custom rotation with Lambda functions
   - Ensures applications always use current credentials

3. **Access Control**
   - Fine-grained IAM permissions
   - Resource-based policies
   - VPC endpoints for private access

4. **Monitoring and Logging**
   - CloudTrail integration for audit logs
   - CloudWatch integration for monitoring
   - EventBridge integration for secret rotation events

### Common Use Cases

1. Database credential management
2. API key management
3. OAuth token management
4. Service-to-service authentication
5. Compliance with regulations requiring periodic credential rotation

## AWS Key Management Service (KMS)

Create and control encryption keys for AWS services and applications:

### Key Concepts

1. **Customer Master Keys (CMKs)**
   - Primary resource in KMS
   - Types:
     - Customer managed: Created, managed and used by you
     - AWS managed: Created, managed by AWS for use in services
     - AWS owned: AWS-owned keys used across multiple accounts

2. **Key Policies**
   - Resource-based policy document
   - Controls access to CMK
   - Required for all CMKs

3. **Key Material Origin**
   - AWS KMS: KMS generates key material
   - External: Import your own key material
   - Custom Key Store: Key material generated and stored in CloudHSM cluster

4. **Symmetric vs. Asymmetric Keys**
   - Symmetric: Same key for encryption and decryption
   - Asymmetric: Public key for encryption, private key for decryption

### KMS Operations

1. **Cryptographic Operations**
   - Encrypt/Decrypt: For small data (up to 4KB)
   - GenerateDataKey: Create data keys for envelope encryption
   - Sign/Verify: Digital signatures (asymmetric keys)

2. **Key Management Operations**
   - Create, describe, list keys
   - Enable/disable keys
   - Schedule key deletion
   - Rotate key material

3. **Envelope Encryption**
   - KMS generates data key
   - Data key encrypts data
   - KMS encrypts data key with CMK
   - Store encrypted data key with encrypted data

### Integration with AWS Services

1. **Native Integration**
   - S3, EBS, RDS, DynamoDB, Lambda
   - Transparent server-side encryption
   - Managed key rotation

2. **AWS Encryption SDK**
   - Client-side encryption library
   - Implements envelope encryption
   - Multiple programming languages supported

### Common Use Cases

1. Encrypting data at rest in AWS services
2. Meeting compliance requirements
3. Secure management of application secrets
4. Cross-account encryption operations
5. Client-side data encryption

## Amazon Certificate Manager (ACM)

Provisioning, managing, and deploying SSL/TLS certificates:

### Key Features

1. **Certificate Provisioning**
   - Request public certificates from Amazon
   - Import third-party certificates
   - Support for wildcard domains

2. **Certificate Types**
   - Public certificates (from Amazon Trust Services)
   - Private certificates (from ACM Private CA)
   - Imported certificates

3. **Managed Renewal**
   - Automatic renewal for Amazon-issued certificates
   - Email validation or DNS validation
   - DNS validation enables fully automated renewal

4. **Integration with AWS Services**
   - CloudFront
   - Elastic Load Balancing
   - API Gateway
   - AWS Elastic Beanstalk

### Benefits

1. **Cost Savings**
   - Free public certificates
   - No charge for automatic renewals

2. **Time Savings**
   - Automated certificate renewal
   - No manual certificate management

3. **Security Improvements**
   - Certificates never leave AWS
   - Private key security

## Amazon Cognito

Add user sign-up, sign-in, and access control to applications:

### Cognito User Pools

1. **Features**
   - User directory with sign-up and sign-in
   - Social identity federation (Google, Facebook, Amazon, Apple)
   - Enterprise federation (SAML, OIDC)
   - Multi-factor authentication
   - Advanced security features (compromised credentials, adaptive authentication)

2. **Authentication Flow**
   - Username/password authentication
   - Secure Remote Password (SRP) protocol
   - JWT tokens (ID, access, refresh)

3. **User Management**
   - Self-service sign-up
   - Account recovery
   - Email and phone verification
   - Custom attributes

### Cognito Identity Pools

1. **Features**
   - Provide temporary AWS credentials for authenticated and guest users
   - Access AWS services directly from client applications
   - Map user identities to IAM roles

2. **Identity Sources**
   - Cognito User Pools
   - Social identity providers
   - SAML and OIDC providers
   - Developer authenticated identities

3. **Access Control**
   - Role-based access control
   - Role mapping rules
   - Fine-grained access with access control attributes

### Common Use Cases

1. Mobile application authentication
2. Web application authentication
3. Secure API access
4. Single sign-on for multiple applications
5. Temporary access to AWS resources

## Knowledge Check

1. What is the primary difference between AWS Shield Standard and AWS Shield Advanced?
2. How can AWS WAF protect your web applications from SQL injection attacks?
3. What is the benefit of using automatic secret rotation in AWS Secrets Manager?
4. Explain envelope encryption in AWS KMS and why it's beneficial.
5. What are the advantages of using DNS validation instead of email validation for ACM certificates?
6. How do Cognito User Pools and Identity Pools work together to provide a complete authentication solution?
7. Which AWS security service would you use to detect and mitigate a Layer 7 (application layer) DDoS attack?

## Hands-on Exercise

1. Create a WAF Web ACL with rules to block common exploits
2. Store a database password in Secrets Manager and configure automatic rotation
3. Create a KMS key and use it to encrypt an S3 bucket
4. Request a public certificate through ACM and use it with a CloudFront distribution
5. Set up a Cognito User Pool for a web application and test the authentication flow

## Next Steps

In the next module, we'll cover Cost Management in AWS, focusing on AWS Cost Explorer, AWS Budgets, Reserved Instances, and Savings Plans. 