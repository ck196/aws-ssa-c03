# Module 6: Security Services - Hands-on Lab Guide

This guide provides detailed instructions for completing the hands-on exercises in Module 6.

## Exercise 1: Configure IAM Roles, Groups, and Policies

### Objective
Create and configure IAM roles, groups, and policies to implement the principle of least privilege.

### Steps

1. **Sign in to the AWS Management Console**
   - Go to https://console.aws.amazon.com/
   - Sign in with your AWS account credentials

2. **Navigate to IAM Service**
   - In the search bar at the top, type "IAM"
   - Select "IAM" from the dropdown menu

3. **Create IAM Policies**
   - In the left navigation pane, click "Policies"
   - Click "Create policy"
   
   **EC2 Read-Only Policy:**
   - Click the "JSON" tab
   - Paste the following policy:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "ec2:Describe*",
             "ec2:Get*",
             "ec2:List*"
           ],
           "Resource": "*"
         }
       ]
     }
     ```
   - Click "Next: Tags"
   - (Optional) Add a tag: Key = "Purpose", Value = "EC2ReadOnly"
   - Click "Next: Review"
   - Name the policy "EC2ReadOnlyAccess-Custom"
   - Add a description: "Allows read-only access to EC2 resources"
   - Click "Create policy"
   
   **S3 Bucket-Specific Policy:**
   - Click "Create policy" again
   - Click the "JSON" tab
   - Paste the following policy (replace "your-bucket-name" with an actual bucket name):
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "s3:GetObject",
             "s3:PutObject",
             "s3:ListBucket"
           ],
           "Resource": [
             "arn:aws:s3:::your-bucket-name",
             "arn:aws:s3:::your-bucket-name/*"
           ]
         }
       ]
     }
     ```
   - Click "Next: Tags"
   - (Optional) Add a tag: Key = "Purpose", Value = "S3BucketAccess"
   - Click "Next: Review"
   - Name the policy "S3BucketAccess-Custom"
   - Add a description: "Allows specific access to a designated S3 bucket"
   - Click "Create policy"

4. **Create IAM Groups**
   - In the left navigation pane, click "User groups"
   - Click "Create group"
   
   **EC2 Operators Group:**
   - Enter "EC2Operators" as the group name
   - In the "Attach permissions policies" section, search for and select:
     - Your custom "EC2ReadOnlyAccess-Custom" policy
     - The AWS managed "AmazonEC2ReadOnlyAccess" policy (for comparison)
   - Click "Create group"
   
   **S3 Developers Group:**
   - Click "Create group" again
   - Enter "S3Developers" as the group name
   - In the "Attach permissions policies" section, search for and select:
     - Your custom "S3BucketAccess-Custom" policy
   - Click "Create group"

5. **Create IAM Users**
   - In the left navigation pane, click "Users"
   - Click "Add users"
   
   **First User:**
   - Enter "ec2-operator" as the user name
   - Select "Password - AWS Management Console access"
   - Select "Custom password" and enter a secure password
   - Uncheck "User must create a new password at next sign-in"
   - Click "Next: Permissions"
   - Select "Add user to group"
   - Select the "EC2Operators" group
   - Click "Next: Tags"
   - (Optional) Add a tag: Key = "Department", Value = "Operations"
   - Click "Next: Review"
   - Click "Create user"
   
   **Second User:**
   - Click "Add users" again
   - Enter "s3-developer" as the user name
   - Select "Password - AWS Management Console access"
   - Select "Custom password" and enter a secure password
   - Uncheck "User must create a new password at next sign-in"
   - Click "Next: Permissions"
   - Select "Add user to group"
   - Select the "S3Developers" group
   - Click "Next: Tags"
   - (Optional) Add a tag: Key = "Department", Value = "Development"
   - Click "Next: Review"
   - Click "Create user"

6. **Create an IAM Role for EC2**
   - In the left navigation pane, click "Roles"
   - Click "Create role"
   - Select "AWS service" as the trusted entity type
   - Select "EC2" as the service
   - Click "Next"
   - Search for and select the "AmazonS3ReadOnlyAccess" policy
   - Click "Next"
   - Enter "EC2-S3ReadOnly" as the role name
   - Add a description: "Allows EC2 instances to read from S3 buckets"
   - Click "Create role"

7. **Test IAM User Access**
   - Sign out of the AWS Management Console
   - Sign in as the "ec2-operator" user
   - Verify that you can view EC2 resources but cannot modify them
   - Try to access S3 and verify that access is denied
   - Sign out and sign in as the "s3-developer" user
   - Verify that you can access the specified S3 bucket but not other AWS services

8. **Test IAM Role with EC2**
   - Sign in with your admin account
   - Navigate to EC2 service
   - Launch an EC2 instance
   - In the "Configure Instance Details" section, under "IAM role", select the "EC2-S3ReadOnly" role
   - Complete the launch process
   - Connect to the instance
   - Install the AWS CLI if not already installed
   - Run the following command to verify S3 read access:
     ```
     aws s3 ls
     ```
   - Try to create an S3 bucket and verify it fails:
     ```
     aws s3 mb s3://test-bucket-name
     ```

9. **Clean Up (Optional)**
   - Terminate the EC2 instance
   - Delete the IAM users
   - Delete the IAM groups
   - Delete the custom IAM policies
   - Delete the IAM role

## Exercise 2: Enable and Configure AWS CloudTrail

### Objective
Set up AWS CloudTrail to monitor and log API activity across your AWS infrastructure.

### Steps

1. **Navigate to CloudTrail Service**
   - In the AWS Management Console, type "CloudTrail" in the search bar
   - Select "CloudTrail" from the dropdown menu

2. **Create a Trail**
   - Click "Trails" in the left navigation pane
   - Click "Create trail"
   - Enter a name for the trail (e.g., "management-events-trail")
   - For "Storage location", select "Create new S3 bucket"
   - Keep the default bucket name or customize it
   - Check "Enable log file validation" for added security
   - Under "Log file SSE-KMS encryption", select "Enabled" (optional)
   - If you selected to enable KMS encryption, either create a new KMS key or use an existing one
   - Under "Additional settings", keep the defaults
   - Click "Next"

3. **Configure Trail Events**
   - Under "Event type", select "Management events"
   - Under "Management events", select "Read" and "Write"
   - (Optional) Under "Exclude AWS KMS events", check both options to reduce volume
   - (Optional) Under "Exclude Amazon RDS Data API events", check the option
   - Click "Next"

4. **Review and Create**
   - Review your trail configuration
   - Click "Create trail"

5. **Create a Trail for Data Events (Optional)**
   - Click "Create trail" again
   - Enter a name (e.g., "s3-data-events-trail")
   - Configure storage location as before
   - Click "Next"
   - Under "Event type", select "Data events"
   - For S3, select "All current and future S3 buckets" or select specific buckets
   - Check both "Read" and "Write" events
   - (Optional) Configure Lambda data events if needed
   - Click "Next"
   - Review and click "Create trail"

6. **Generate Some Activity to Log**
   - Navigate to S3 service
   - Create a new bucket
   - Upload a file to the bucket
   - Download the file
   - Delete the file
   - Navigate to EC2 service
   - Launch and terminate an EC2 instance

7. **View CloudTrail Logs**
   - Return to the CloudTrail console
   - Click "Event history" in the left navigation pane
   - Browse the recorded events
   - Use the "Lookup attributes" filter to find specific events:
     - Select "Event name" and enter "CreateBucket"
     - Select "Resource name" and enter the name of your S3 bucket
   - Click on an event to view its details

8. **Set Up CloudTrail Insights (Optional)**
   - Go to the "Trails" page
   - Select your main trail
   - Click "Edit"
   - Scroll down to "Insights"
   - Select "Enable Insights events"
   - Choose "API call rate", "API error rate", or both
   - Click "Save changes"
   - Note: Insights may take up to 36 hours to appear after enabling

9. **Create a CloudWatch Log Group for CloudTrail**
   - Go to the "Trails" page
   - Select your main trail
   - Click "Edit"
   - Under "CloudWatch Logs", click "Enabled"
   - For "Log group name", keep the default or enter a custom name
   - For "Role name", create a new role or use an existing one
   - Click "Save changes"

10. **Set Up SNS Notifications for CloudTrail (Optional)**
    - Go to the SNS service
    - Click "Topics" in the left navigation pane
    - Click "Create topic"
    - Select "Standard" type
    - Enter a name (e.g., "cloudtrail-alerts")
    - Click "Create topic"
    - Click "Create subscription"
    - Select "Email" as the protocol
    - Enter your email address
    - Click "Create subscription"
    - Confirm the subscription from your email
    - Return to CloudTrail
    - Edit your trail
    - Under "SNS notification delivery", select "Enabled"
    - Select your SNS topic
    - Click "Save changes"

11. **Clean Up (Optional)**
    - Delete the CloudTrail trails
    - Delete the S3 buckets created for CloudTrail
    - Delete the CloudWatch Log Group
    - Delete the SNS topic and subscription

## Exercise 3: Implement AWS WAF with CloudFront

### Objective
Set up AWS WAF (Web Application Firewall) to protect a CloudFront distribution from common web exploits.

### Prerequisites
- An existing S3 bucket with static website content
- (Optional) A CloudFront distribution serving the S3 content

### Steps

1. **Navigate to AWS WAF & Shield Service**
   - In the AWS Management Console, type "WAF" in the search bar
   - Select "WAF & Shield" from the dropdown menu

2. **Create a Web ACL**
   - Click "Web ACLs" in the left navigation pane
   - Click "Create web ACL"
   - Enter a name (e.g., "MyWebApplicationFirewall")
   - (Optional) Add a description
   - For "Resource type", select "CloudFront distributions"
   - For "Region", it should automatically select "Global (CloudFront)"
   - Click "Next"

3. **Add Rules to the Web ACL**
   - In the "Add rules and rule groups" section, click "Add rules" and select "Add managed rule groups"
   - Under "AWS managed rule groups", select:
     - "Amazon IP reputation list" (to block requests from IPs identified as bots or threats)
     - "Core rule set" (to block common attacks like SQL injection and XSS)
   - Click "Add rules"
   - Click "Add rules" again and select "Add my own rules and rule groups"
   - Select "Rule Builder"
   - Name the rule "RateBasedRule"
   - For "Type", select "Rate-based rule"
   - Set "Rate limit" to 2000 (requests per 5 minutes)
   - Leave "Inspect" as "All requests"
   - Click "Add rule"
   - Click "Next"

4. **Set Rule Priority**
   - Drag and drop the rules to set their priority order (rules are evaluated from top to bottom)
   - Typically, rate-based rules should be at the bottom
   - Click "Next"

5. **Configure Action Settings**
   - In the "Default web ACL action for requests that don't match any rules", select "Allow"
   - Under "Configure metrics", keep the defaults
   - Click "Next"

6. **Review and Create**
   - Review your web ACL configuration
   - Click "Create web ACL"

7. **Associate the Web ACL with a CloudFront Distribution**
   - If you don't already have a CloudFront distribution:
     - Go to the CloudFront service
     - Click "Create Distribution"
     - For "Origin Domain Name", select your S3 bucket with website content
     - Configure other settings as needed
     - Click "Create Distribution"
   - Return to the WAF console
   - Select your web ACL
   - Click the "Associated AWS resources" tab
   - Click "Add AWS resources"
   - Select your CloudFront distribution
   - Click "Add"

8. **Test the Web ACL**
   - Wait for the CloudFront distribution to deploy
   - Open your CloudFront URL in a browser to verify normal access
   - Test security by attempting some basic attack patterns:
     - SQL Injection: Try adding `?id=1' OR '1'='1` to the URL
     - XSS: Try adding `?input=<script>alert('XSS')</script>` to the URL
   - These should be blocked with a 403 Forbidden error

9. **View WAF Metrics and Logs**
   - In the WAF console, select your web ACL
   - Click the "Monitoring" tab to view metrics
   - (Optional) Set up logging:
     - Click the "Logging and metrics" tab
     - Click "Enable logging"
     - Select an existing S3 bucket or create a new one
     - Configure path prefix and logging options
     - Click "Save"

10. **Create a Custom Rule (Optional)**
    - In the WAF console, select your web ACL
    - Click "Rules" tab
    - Click "Add rules" and select "Add my own rules and rule groups"
    - Select "Rule Builder"
    - Name the rule "BlockSpecificCountries"
    - For "Type", select "Regular rule"
    - For "If a request", select "Originates from a country in"
    - Select countries you want to block
    - For "Then", select "Block"
    - Click "Add rule"
    - Click "Save"

11. **Clean Up (Optional)**
    - Remove the association between the web ACL and CloudFront distribution
    - Delete the web ACL
    - Delete the CloudFront distribution
    - Delete the S3 bucket if no longer needed

## Exercise 4: Set Up AWS Secrets Manager

### Objective
Use AWS Secrets Manager to store, rotate, and securely access sensitive information.

### Steps

1. **Navigate to AWS Secrets Manager**
   - In the AWS Management Console, type "Secrets Manager" in the search bar
   - Select "Secrets Manager" from the dropdown menu

2. **Create a New Secret**
   - Click "Store a new secret"
   - Under "Secret type", select "Other type of secret"
   - Add key-value pairs for your secret:
     - Key: "username", Value: "admin"
     - Key: "password", Value: "YourStrongPassword123!"
   - (Optional) Add additional key-value pairs as needed
   - For encryption key, leave the default "aws/secretsmanager" or select a custom KMS key
   - Click "Next"

3. **Configure Secret Details**
   - Enter a name for your secret (e.g., "app/dev/credentials")
   - (Optional) Add a description
   - (Optional) Add tags: Key = "Environment", Value = "Development"
   - Click "Next"

4. **Configure Automatic Rotation (Optional)**
   - For this exercise, select "Disable automatic rotation"
   - Click "Next"
   - Review your settings
   - Click "Store"

5. **Create a Database Secret**
   - Click "Store a new secret" again
   - Under "Secret type", select "Credentials for Amazon RDS database"
   - Enter a username and password
   - Select a database (if you have one) or skip this step
   - Click "Next"
   - Name the secret (e.g., "db/dev/mysql-credentials")
   - Complete the creation process as before

6. **Retrieve a Secret**
   - In the Secrets Manager console, select your first secret
   - Click "Retrieve secret value"
   - View the decrypted secret values

7. **Set Up Access from AWS CLI**
   - Open a terminal or command prompt
   - Ensure AWS CLI is installed and configured
   - Run the following command:
     ```
     aws secretsmanager get-secret-value --secret-id app/dev/credentials
     ```
   - Note how the entire secret, including encrypted values, is returned

8. **Access Specific Secret Values**
   - You can parse the JSON response to get specific values:
     ```
     aws secretsmanager get-secret-value --secret-id app/dev/credentials --query SecretString --output text | jq -r '.password'
     ```
     (Requires jq to be installed)

9. **Set Up Access from an EC2 Instance**
   - Launch an EC2 instance with an IAM role that has permissions to access Secrets Manager
   - Connect to the instance
   - Install the AWS CLI if not already installed
   - Try accessing the secret using the AWS CLI as shown above

10. **Create an IAM Policy for Limited Secrets Access**
    - Go to the IAM service
    - Click "Policies" in the left navigation pane
    - Click "Create policy"
    - Click the "JSON" tab
    - Paste the following policy:
      ```json
      {
        "Version": "2012-10-17",
        "Statement": [
          {
            "Effect": "Allow",
            "Action": [
              "secretsmanager:GetSecretValue"
            ],
            "Resource": "arn:aws:secretsmanager:*:*:secret:app/dev/*"
          }
        ]
      }
      ```
    - Click "Next: Tags"
    - (Optional) Add tags
    - Click "Next: Review"
    - Name the policy "AppDevSecretsReadOnly"
    - Click "Create policy"
    - Attach this policy to users or roles that need access to these specific secrets

11. **Configure Secret Rotation (Optional)**
    - Select your database secret
    - Click "Edit rotation"
    - Select "Enable automatic rotation"
    - Set the rotation schedule (e.g., 30 days)
    - Select "Create a new Lambda function" to handle rotation
    - Enter a name for the Lambda function
    - Click "Save"

12. **Clean Up (Optional)**
    - Delete the secrets you created
    - Delete any IAM policies you created
    - Terminate the EC2 instance if launched for this exercise

## Exercise 5: Implement AWS Key Management Service (KMS)

### Objective
Create and manage encryption keys using AWS KMS and use them to encrypt data.

### Steps

1. **Navigate to KMS Service**
   - In the AWS Management Console, type "KMS" in the search bar
   - Select "Key Management Service" from the dropdown menu

2. **Create a Customer Managed Key (CMK)**
   - In the left navigation pane, click "Customer managed keys"
   - Click "Create key"
   - For "Key type", select "Symmetric"
   - For "Key usage", keep "Encrypt and decrypt"
   - Click "Next"
   - Enter an alias for your key (e.g., "app-data-key")
   - (Optional) Add a description and tags
   - Click "Next"
   - For key administrators, select your IAM user or a role
   - Click "Next"
   - For key users, select your IAM user or a role
   - Click "Next"
   - Review the key policy
   - Click "Finish"

3. **Create an Asymmetric Key (Optional)**
   - Click "Create key" again
   - For "Key type", select "Asymmetric"
   - For "Key usage", select "Sign and verify"
   - For "Key spec", select "RSA_2048"
   - Complete the creation process as before, naming it "signing-key"

4. **Encrypt Data Using KMS**
   - Open AWS CloudShell or a terminal with AWS CLI configured
   - Create a sample file:
     ```
     echo "This is sensitive data" > plaintext.txt
     ```
   - Encrypt the file using your KMS key:
     ```
     aws kms encrypt \
       --key-id alias/app-data-key \
       --plaintext fileb://plaintext.txt \
       --output text \
       --query CiphertextBlob | base64 --decode > encrypted.txt
     ```

5. **Decrypt Data Using KMS**
   - Decrypt the encrypted file:
     ```
     aws kms decrypt \
       --ciphertext-blob fileb://encrypted.txt \
       --output text \
       --query Plaintext | base64 --decode > decrypted.txt
     ```
   - Verify the decrypted content:
     ```
     cat decrypted.txt
     ```

6. **Use KMS with S3 Bucket Encryption**
   - Go to the S3 service
   - Create a new bucket
   - Under "Default encryption", select "AWS Key Management Service key (SSE-KMS)"
   - Choose "Choose from your AWS KMS keys"
   - Select your "app-data-key"
   - Complete the bucket creation
   - Upload a file to the bucket
   - The file will be automatically encrypted using your KMS key

7. **Set Up Automatic Key Rotation**
   - In the KMS console, select your symmetric key
   - Click the "Key rotation" tab
   - Select "Automatically rotate this KMS key every year"
   - Click "Save"

8. **Create a Key Policy with Limited Permissions**
   - Select your KMS key
   - Click the "Key policy" tab
   - Click "Edit"
   - Add a statement to limit key usage to specific services:
     ```json
     {
       "Sid": "AllowServiceAccess",
       "Effect": "Allow",
       "Principal": {
         "Service": "s3.amazonaws.com"
       },
       "Action": [
         "kms:GenerateDataKey*",
         "kms:Decrypt"
       ],
       "Resource": "*"
     }
     ```
   - Click "Save changes"

9. **Create a Custom Key Store (Advanced, Optional)**
   - This requires AWS CloudHSM to be set up
   - In the KMS console, click "Custom key stores" in the left navigation pane
   - Click "Create custom key store"
   - Follow the prompts to connect to a CloudHSM cluster
   - Once set up, you can create keys in this custom key store for additional security

10. **Monitor KMS Key Usage**
    - In the KMS console, select your key
    - Click the "Cryptographic configuration" tab to view key specifications
    - View monitoring information in the CloudWatch service:
      - Navigate to CloudWatch
      - Under "Metrics", find "KMS" metrics
      - Search for your key ID to view usage metrics

11. **Clean Up (Optional)**
    - Delete files created during the exercise
    - Schedule the KMS key for deletion:
      - Select the key
      - Click "Key actions" → "Schedule key deletion"
      - Select a waiting period (minimum 7 days)
      - Confirm the deletion
    - Delete the S3 bucket created for this exercise

## Conclusion

In this hands-on lab, you've learned how to:
- Configure IAM roles, groups, and policies to implement least privilege
- Set up AWS CloudTrail for comprehensive API activity monitoring
- Implement AWS WAF to protect web applications from common attacks
- Use AWS Secrets Manager to securely store and access sensitive information
- Create and manage encryption keys with AWS KMS

These security skills are essential for protecting your AWS resources and ensuring compliance with security best practices and regulations. Security is a shared responsibility between AWS and you as the customer, and these services help you fulfill your part of the responsibility model. 