# Module 1: AWS Fundamentals

## AWS Global Infrastructure

AWS operates a global infrastructure consisting of:

1. **Regions**: Geographic areas containing multiple Availability Zones
   - Each region is completely independent
   - Currently 27+ regions worldwide
   - Services may vary by region
   - When selecting a region, consider:
     - Data sovereignty/compliance requirements
     - Latency to end users
     - Service availability
     - Pricing (varies by region)

2. **Availability Zones (AZs)**: Physically separate data centers within a region
   - Connected by low-latency links
   - Isolated from failures in other AZs
   - Recommended to deploy across multiple AZs for high availability

3. **Edge Locations**: Content delivery network endpoints
   - Used by CloudFront and Route 53
   - More numerous than regions (200+ globally)
   - Reduces latency for end users

## AWS Shared Responsibility Model

AWS operates under a shared responsibility model which clearly defines security responsibilities:

### AWS Responsibilities ("Security OF the Cloud")
- Physical infrastructure security
- Network infrastructure
- Virtualization infrastructure
- Service software

### Customer Responsibilities ("Security IN the Cloud")
- Data encryption
- Identity and access management
- Operating system configuration
- Network traffic protection
- Customer data

Remember: AWS is responsible for protecting the infrastructure that runs all services, while you are responsible for securing your data and applications.

## Identity and Access Management (IAM)

IAM enables you to securely control access to AWS services and resources:

### Key IAM Concepts:
1. **Users**: Individuals requiring AWS access
   - Each user has unique credentials
   - Should follow principle of least privilege

2. **Groups**: Collections of users with shared permissions
   - Simplifies permission management
   - Users can belong to multiple groups

3. **Roles**: Sets of permissions that can be assumed
   - Used by services or external entities
   - Temporary credentials, no passwords
   - Common use cases: EC2 instance roles, cross-account access

4. **Policies**: JSON documents defining permissions
   - Can be attached to users, groups, or roles
   - Two types: AWS managed and customer managed
   - Example policy structure:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::example-bucket/*"
       }
     ]
   }
   ```

5. **Best Practices**:
   - Enable MFA for all users
   - Use IAM roles for applications on EC2
   - Regularly rotate credentials
   - Use the principle of least privilege
   - Use IAM Access Analyzer to identify resources shared with external entities

## AWS CLI and SDK Basics

### AWS Command Line Interface (CLI)
- Tool to manage AWS services from the command line
- Provides direct access to AWS service APIs
- Installation: `pip install awscli`
- Configuration: `aws configure` (requires access key, secret key)
- Example commands:
  ```bash
  # List S3 buckets
  aws s3 ls
  
  # Create an EC2 instance
  aws ec2 run-instances --image-id ami-12345678 --instance-type t2.micro
  
  # Describe running instances
  aws ec2 describe-instances
  ```

### AWS SDKs (Software Development Kits)
- Libraries for programmatic access to AWS
- Available for multiple languages:
  - Python (boto3)
  - JavaScript
  - Java
  - .NET
  - Ruby
  - Go
  - PHP
- Example using boto3 (Python):
  ```python
  import boto3
  
  # Create an S3 client
  s3 = boto3.client('s3')
  
  # List buckets
  response = s3.list_buckets()
  for bucket in response['Buckets']:
      print(bucket['Name'])
  ```

## Knowledge Check

1. What factors should you consider when choosing an AWS Region?
2. How many AZs should you use for a high-availability application?
3. In the Shared Responsibility Model, who is responsible for encrypting data?
4. What IAM entity would you use to grant permissions to an EC2 instance?
5. How would you list all S3 buckets using the AWS CLI?

## Hands-on Exercise

1. Create an IAM user with programmatic access
2. Set up the AWS CLI on your local machine
3. Create an IAM policy that grants read-only access to S3
4. Attach the policy to your IAM user
5. Use the AWS CLI to list available S3 buckets

## Next Steps

In the next module, we'll cover AWS Compute Services, focusing on EC2, Lambda, and Auto Scaling. 