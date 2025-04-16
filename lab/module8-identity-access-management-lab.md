# Module 8: Identity and Access Management - Hands-on Lab Guide

This guide provides detailed instructions for completing the hands-on exercises in Module 8.

## Exercise 1: Implementing IAM Best Practices

### Objective
Configure IAM according to AWS best practices to secure your AWS account properly.

### Steps

1. **Sign in to the AWS Management Console**
   - Go to https://console.aws.amazon.com/
   - Sign in with your AWS account credentials

2. **Enable MFA for the Root User**
   - Click on your account name in the top right corner
   - Select "Security credentials"
   - Under "Multi-factor authentication (MFA)", click "Assign MFA device"
   - Choose your preferred MFA device type:
     - Virtual MFA device (authenticator app)
     - Security key
     - Hardware MFA device
   - For a virtual MFA device:
     - Choose "Authenticator app"
     - Click "Continue"
     - Use an authenticator app (like Google Authenticator, Microsoft Authenticator, or Authy) to scan the QR code
     - Enter two consecutive MFA codes from your authenticator app
     - Click "Add MFA"

3. **Create an IAM Admin User**
   - Navigate to the IAM service
   - Click "Users" in the left navigation pane
   - Click "Create user"
   - Enter a user name (e.g., "administrator")
   - Check "Provide user access to the AWS Management Console"
   - Select "I want to create an IAM user"
   - Create a custom password and uncheck "User must create a new password at next sign-in" if needed
   - Click "Next"
   - Choose "Attach policies directly"
   - Search for and select "AdministratorAccess"
   - Click "Next"
   - Review the user details and click "Create user"
   - Sign out and sign back in using your new admin user

4. **Set Up an IAM Password Policy**
   - In the IAM console, click "Account settings" in the left navigation pane
   - Under "Password policy", click "Edit"
   - Set the following requirements:
     - Minimum password length: 12 characters
     - Require at least one uppercase letter
     - Require at least one lowercase letter
     - Require at least one number
     - Require at least one non-alphanumeric character
     - Enable password expiration after 90 days
     - Prevent password reuse (set to last 5 passwords)
   - Click "Save changes"

5. **Create IAM Groups for Different Job Functions**
   - Click "User groups" in the left navigation pane
   - Click "Create group"
   
   **Developers Group:**
   - Enter "Developers" as the group name
   - Search for and select the following policies:
     - "AmazonS3ReadOnlyAccess"
     - "AmazonDynamoDBReadOnlyAccess"
     - "AmazonEC2ReadOnlyAccess"
   - Click "Create group"
   
   **DBAdmins Group:**
   - Click "Create group" again
   - Enter "DBAdmins" as the group name
   - Search for and select the following policies:
     - "AmazonRDSFullAccess"
     - "AmazonDynamoDBFullAccess"
   - Click "Create group"

6. **Create IAM Roles for Services**
   - Click "Roles" in the left navigation pane
   - Click "Create role"
   - Select "AWS service" as the trusted entity
   - Select "EC2" as the service
   - Click "Next"
   - Search for and select "AmazonS3ReadOnlyAccess"
   - Click "Next"
   - Enter a role name (e.g., "EC2-S3ReadOnly")
   - Add a description
   - Click "Create role"

7. **Use IAM Access Analyzer**
   - Click "Access analyzer" in the left navigation pane
   - Click "Create analyzer"
   - Select "Account" as the analyzer type
   - Enter a name for the analyzer
   - Keep the default organization or account setting
   - Click "Create analyzer"
   - Review any findings that appear (may take some time)

8. **Generate and Review IAM Credential Report**
   - Click "Credential report" in the left navigation pane
   - Click "Download report" (or "Generate report" if not already generated)
   - Open the CSV file and review user credentials statuses
   - Check for:
     - Users with no MFA
     - Users with old passwords
     - Unused access keys

9. **Clean Up (Optional)**
   - Do not delete the admin user or disable root MFA
   - You can delete the test groups and roles if desired

## Exercise 2: Implementing Attribute-Based Access Control (ABAC)

### Objective
Set up ABAC using IAM tags to control access to AWS resources.

### Steps

1. **Create IAM Policy for ABAC**
   - Navigate to the IAM service
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
             "ec2:RunInstances",
             "ec2:StartInstances",
             "ec2:StopInstances",
             "ec2:DescribeInstances",
             "ec2:TerminateInstances"
           ],
           "Resource": "*",
           "Condition": {
             "StringEquals": {
               "aws:ResourceTag/Project": "${aws:PrincipalTag/Project}"
             }
           }
         },
         {
           "Effect": "Allow",
           "Action": "ec2:CreateTags",
           "Resource": "*",
           "Condition": {
             "StringEquals": {
               "aws:RequestTag/Project": "${aws:PrincipalTag/Project}"
             }
           }
         },
         {
           "Effect": "Allow",
           "Action": "ec2:DescribeInstances",
           "Resource": "*"
         }
       ]
     }
     ```
   - Click "Next: Tags"
   - Click "Next: Review"
   - Name the policy "EC2ProjectBasedAccess"
   - Add a description
   - Click "Create policy"

2. **Create IAM Users for Testing ABAC**
   - In the IAM console, click "Users" in the left navigation pane
   - Click "Create user"
   - Enter "project-a-user" as the user name
   - Enable AWS Management Console access
   - Set a custom password
   - Uncheck "User must create a new password at next sign-in"
   - Click "Next"
   - Click "Attach policies directly"
   - Search for and select your "EC2ProjectBasedAccess" policy
   - Click "Next"
   - Click "Create user"
   
   - Repeat the process to create a second user named "project-b-user"

3. **Add Tags to IAM Users**
   - In the users list, click on "project-a-user"
   - Go to the "Tags" tab
   - Click "Add new tag"
   - Enter "Project" as the Key and "ProjectA" as the Value
   - Click "Save changes"
   
   - Go back to the users list and click on "project-b-user"
   - Go to the "Tags" tab
   - Click "Add new tag"
   - Enter "Project" as the Key and "ProjectB" as the Value
   - Click "Save changes"

4. **Test ABAC with the Project A User**
   - Sign out of the AWS console
   - Sign in as "project-a-user"
   - Navigate to the EC2 service
   - Launch an EC2 instance:
     - Choose an Amazon Linux 2 AMI
     - Select a t2.micro instance type
     - Proceed to the "Add Tags" section
     - Add a tag with Key "Project" and Value "ProjectA"
     - Complete the launch wizard and launch the instance
   - Verify that the instance launches successfully
   
   - Try to launch another instance:
     - Follow the same steps, but this time add a tag with Key "Project" and Value "ProjectB"
     - Attempt to launch the instance
     - This should fail with an access denied error

5. **Test ABAC with the Project B User**
   - Sign out of the AWS console
   - Sign in as "project-b-user"
   - Navigate to the EC2 service
   - Attempt to stop or terminate the "ProjectA" instance
   - This should fail with an access denied error
   
   - Launch an EC2 instance:
     - Choose an Amazon Linux 2 AMI
     - Select a t2.micro instance type
     - Proceed to the "Add Tags" section
     - Add a tag with Key "Project" and Value "ProjectB"
     - Complete the launch wizard and launch the instance
   - Verify that the instance launches successfully

6. **Clean Up**
   - Sign in with your admin user
   - Terminate any running EC2 instances
   - Delete the test users (project-a-user and project-b-user)
   - Delete the EC2ProjectBasedAccess policy

## Exercise 3: Working with IAM Identity Center (SSO)

### Objective
Set up AWS IAM Identity Center (formerly AWS Single Sign-On) to manage access to multiple AWS accounts.

### Prerequisites
- Access to AWS Organizations or permission to create an organization
- Multiple AWS accounts (or ability to create them)

### Steps

1. **Enable AWS Organizations**
   - Sign in to the AWS Management Console
   - Navigate to the AWS Organizations service
   - Click "Create organization"
   - Select "Enable all features" and click "Create organization"
   - If you receive an email verification request, complete it

2. **Enable IAM Identity Center**
   - Navigate to the IAM Identity Center service
   - Click "Enable" on the IAM Identity Center page
   - Select "Use AWS Organizations" as the identity source
   - Click "Next"
   - Review the settings and click "Enable"

3. **Create User Groups**
   - In the IAM Identity Center console, select "Groups" from the left navigation pane
   - Click "Create group"
   - Enter "Administrators" as the group name
   - (Optional) Add a description
   - Click "Create"
   
   - Repeat to create another group named "Developers"

4. **Create Users**
   - In the IAM Identity Center console, select "Users" from the left navigation pane
   - Click "Add user"
   - Enter the user details:
     - Username: "admin1"
     - Email address: your email or a test email
     - First name and Last name
     - Display name will auto-populate
   - Keep "Send an email to the user with password setup instructions" checked
   - Click "Next"
   - Select the "Administrators" group
   - Click "Next"
   - Review the information and click "Add user"
   
   - Repeat to create another user named "dev1" and add them to the "Developers" group

5. **Create Permission Sets**
   - In the IAM Identity Center console, select "Permission sets" from the left navigation pane
   - Click "Create permission set"
   - Select "Predefined permission set"
   - Select "AdministratorAccess"
   - Click "Next"
   - Keep the default settings and click "Next"
   - Review the information and click "Create"
   
   - Repeat to create another permission set:
     - Select "Predefined permission set"
     - Select "ReadOnlyAccess"
     - Complete the creation process

6. **Assign Access to AWS Accounts**
   - In the IAM Identity Center console, select "AWS accounts" from the left navigation pane
   - Select the checkbox next to your organization or specific accounts
   - Click "Assign users or groups"
   - Select the "Groups" tab
   - Select the "Administrators" group
   - Click "Next"
   - Select the "AdministratorAccess" permission set
   - Click "Next"
   - Review the information and click "Submit"
   
   - Repeat the process to assign the "Developers" group to accounts with the "ReadOnlyAccess" permission set

7. **Access the AWS Access Portal**
   - In the IAM Identity Center console, click on "Dashboard"
   - Note the AWS access portal URL
   - Open a new browser window or incognito/private browsing session
   - Navigate to the AWS access portal URL
   - If you set up email verification, check your email for the password setup link
   - Set up a password and sign in
   - You should see the AWS accounts you have access to
   - Click on an account and role to access the AWS Management Console for that account

8. **Create Custom Permission Sets (Optional)**
   - In the IAM Identity Center console, select "Permission sets" from the left navigation pane
   - Click "Create permission set"
   - Select "Custom permission set"
   - Enter a name (e.g., "EC2DeveloperAccess")
   - (Optional) Add a description
   - Click "Next"
   - Click the "JSON" tab
   - Paste a custom policy such as:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "ec2:Describe*",
             "ec2:RunInstances",
             "ec2:StartInstances",
             "ec2:StopInstances"
           ],
           "Resource": "*"
         }
       ]
     }
     ```
   - Click "Next"
   - Review and click "Create"
   - Assign this permission set to the Developers group

9. **Clean Up (Optional)**
   - Delete permission sets
   - Delete users and groups
   - Disable IAM Identity Center

## Exercise 4: Implementing Resource-Based Policies

### Objective
Create and apply resource-based policies to control access to AWS resources.

### Steps

1. **Create an S3 Bucket with Resource-Based Policy**
   - Navigate to the S3 service
   - Click "Create bucket"
   - Enter a globally unique bucket name (e.g., "resource-policy-demo-[your-initials]-[random-number]")
   - Keep the default settings
   - Click "Create bucket"
   
   - Select your new bucket
   - Click the "Permissions" tab
   - Scroll down to "Bucket policy" and click "Edit"
   - Paste the following policy (replace "BUCKET-NAME" with your bucket name):
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": {
             "AWS": "arn:aws:iam::ACCOUNT-ID:user/USERNAME"
           },
           "Action": [
             "s3:GetObject",
             "s3:ListBucket"
           ],
           "Resource": [
             "arn:aws:s3:::BUCKET-NAME",
             "arn:aws:s3:::BUCKET-NAME/*"
           ]
         }
       ]
     }
     ```
   - Replace "ACCOUNT-ID" with your AWS account ID (found in the top right menu under your account name)
   - Replace "USERNAME" with a test user you created earlier or "project-a-user" if still available
   - Click "Save changes"

2. **Upload a Test File to the S3 Bucket**
   - Click the "Objects" tab
   - Click "Upload"
   - Click "Add files" and select a simple text file or image
   - Click "Upload"

3. **Test Access with Different Users**
   - Sign out and sign in as the user specified in the bucket policy
   - Navigate to the S3 service
   - Find and click on your bucket
   - Verify that you can list objects and download the test file
   
   - Sign out and sign in as a different user (or create a new one)
   - Try to access the same bucket
   - Verify that access is denied

4. **Create an SQS Queue with Resource-Based Policy**
   - Navigate to the SQS service
   - Click "Create queue"
   - Enter a queue name (e.g., "resource-policy-demo-queue")
   - Select "Standard" queue type
   - Scroll down to "Access policy"
   - Select "Advanced" and paste the following policy:
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": {
             "AWS": "arn:aws:iam::ACCOUNT-ID:user/USERNAME"
           },
           "Action": [
             "sqs:SendMessage",
             "sqs:ReceiveMessage"
           ],
           "Resource": "arn:aws:sqs:REGION:ACCOUNT-ID:resource-policy-demo-queue"
         }
       ]
     }
     ```
   - Replace "ACCOUNT-ID" with your AWS account ID
   - Replace "USERNAME" with a test user's name
   - Replace "REGION" with your current AWS region
   - Keep other settings as default
   - Click "Create queue"

5. **Test SQS Queue Access**
   - Sign out and sign in as the user specified in the queue policy
   - Navigate to the SQS service
   - Select your queue
   - Click "Send and receive messages"
   - Enter a message body and click "Send message"
   - Click "Poll for messages" to verify you can receive the message
   
   - Sign out and sign in as a different user
   - Try to send and receive messages
   - Verify that access is denied

6. **Create a KMS Key with Resource-Based Policy**
   - Navigate to the KMS service
   - Click "Customer managed keys" in the left navigation pane
   - Click "Create key"
   - Select "Symmetric" for key type
   - Click "Next"
   - Enter an alias (e.g., "resource-policy-demo-key")
   - (Optional) Add a description and tags
   - Click "Next"
   - For key administrators, select your admin user
   - Click "Next"
   - For key users, select the test user specified in previous policies
   - Click "Next"
   - Review the settings and click "Finish"

7. **Use the KMS Key to Encrypt S3 Objects**
   - Navigate back to the S3 service
   - Create a new bucket with default settings
   - In the "Default encryption" section, select "Override" and choose:
     - Encryption type: "Server-side encryption with AWS KMS keys (SSE-KMS)"
     - AWS KMS key: select your newly created key
   - Upload a file to this bucket
   - Sign in as the key user and verify you can download the file
   - Sign in as a different user and verify you cannot download the file

8. **Clean Up**
   - Delete the S3 buckets
   - Delete the SQS queue
   - Schedule deletion for the KMS key (minimum 7 days waiting period)

## Exercise 5: Implementing AWS STS and Cross-Account Access

### Objective
Use AWS Security Token Service (STS) to implement cross-account access.

### Prerequisites
- Two AWS accounts (primary and secondary)
- Administrative access to both accounts

### Steps

1. **Create a Role in the Secondary Account**
   - Sign in to the secondary AWS account
   - Navigate to the IAM service
   - Click "Roles" in the left navigation pane
   - Click "Create role"
   - Select "Another AWS account" as the trusted entity type
   - Enter the Account ID of your primary AWS account
   - (Optional) Select "Require external ID" and enter a secret value
   - Click "Next"
   - Attach the "ReadOnlyAccess" policy
   - Click "Next"
   - Enter a role name (e.g., "CrossAccountReadOnly")
   - (Optional) Add a description
   - Click "Create role"
   - Note the ARN of the newly created role

2. **Create a User Policy in the Primary Account**
   - Sign in to the primary AWS account
   - Navigate to the IAM service
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
           "Action": "sts:AssumeRole",
           "Resource": "arn:aws:iam::SECONDARY-ACCOUNT-ID:role/CrossAccountReadOnly"
         }
       ]
     }
     ```
   - Replace "SECONDARY-ACCOUNT-ID" with your secondary AWS account ID
   - Click "Next: Tags"
   - Click "Next: Review"
   - Name the policy "AssumeSecondaryAccountRole"
   - Click "Create policy"

3. **Create a Test User in the Primary Account**
   - In the IAM console of the primary account, click "Users" in the left navigation pane
   - Click "Create user"
   - Enter a username (e.g., "cross-account-user")
   - Enable AWS Management Console access
   - Set a custom password
   - Uncheck "User must create a new password at next sign-in"
   - Click "Next"
   - Attach the "AssumeSecondaryAccountRole" policy you just created
   - Click "Next"
   - Review and click "Create user"

4. **Test Cross-Account Access with Console Switching**
   - Sign out and sign in as the "cross-account-user"
   - Click your username in the top right corner
   - Select "Switch role"
   - Enter the following information:
     - Account: Your secondary account ID
     - Role: CrossAccountReadOnly
     - Display Name: Secondary Account
     - (Optional) Choose a color
   - Click "Switch Role"
   - Verify that you are now operating in the secondary account with read-only access
   - Try to create a resource and verify that you cannot

5. **Implement Cross-Account Access Programmatically (Optional)**
   - Install and configure the AWS CLI
   - Configure a named profile for your primary account:
     ```
     aws configure --profile primary
     ```
   - Use STS to assume the role in the secondary account:
     ```
     aws sts assume-role --role-arn "arn:aws:iam::SECONDARY-ACCOUNT-ID:role/CrossAccountReadOnly" --role-session-name "CrossAccountSession" --profile primary
     ```
   - The command returns temporary credentials
   - Configure a new profile with these temporary credentials:
     ```
     aws configure set aws_access_key_id VALUE --profile secondary
     aws configure set aws_secret_access_key VALUE --profile secondary
     aws configure set aws_session_token VALUE --profile secondary
     aws configure set region REGION --profile secondary
     ```
   - Use the secondary profile to access resources:
     ```
     aws s3 ls --profile secondary
     ```

6. **Clean Up**
   - Delete the test user in the primary account
   - Delete the AssumeSecondaryAccountRole policy
   - Delete the CrossAccountReadOnly role in the secondary account

## Conclusion

In this hands-on lab, you've learned how to:
- Implement IAM best practices including MFA, password policies, and least privilege
- Use Attribute-Based Access Control (ABAC) to manage permissions using tags
- Set up IAM Identity Center for centralized access management across multiple accounts
- Create and apply resource-based policies for S3, SQS, and KMS
- Implement cross-account access using AWS STS

These identity and access management skills are essential for securing your AWS environment and implementing proper governance controls in enterprise-scale deployments. 