# Module 1: AWS Fundamentals - Hands-on Lab Guide

This guide provides detailed instructions for completing the hands-on exercises in Module 1.

## Exercise 1: Create an IAM User with Programmatic Access

### Objective
Create an IAM user with programmatic access to AWS services and apply appropriate permissions.

### Steps

1. **Sign in to the AWS Management Console**
   - Go to https://console.aws.amazon.com/
   - Sign in with your AWS account credentials

2. **Navigate to IAM Service**
   - In the search bar at the top, type "IAM"
   - Select "IAM" from the dropdown menu

3. **Create a New IAM User**
   - In the left navigation pane, click on "Users"
   - Click the "Add users" button
   - Enter a username (e.g., "aws-admin")
   - Under "Select AWS access type", check "Access key - Programmatic access"
   - Click "Next: Permissions"

4. **Assign Permissions**
   - Choose "Attach existing policies directly"
   - Search for and select "ReadOnlyAccess" policy (for practice purposes)
   - Click "Next: Tags"

5. **Add Tags (Optional)**
   - Add a key-value pair tag (e.g., Key: "Purpose", Value: "Training")
   - Click "Next: Review"

6. **Review and Create**
   - Review the user details and permissions
   - Click "Create user"

7. **Save Credentials**
   - Download the CSV file containing the access key ID and secret access key
   - Alternatively, copy and securely store these credentials
   - Note: This is the only time AWS shows the secret access key
   - Click "Close"

## Exercise 2: Set up the AWS CLI on Your Local Machine

### Objective
Install and configure the AWS CLI to interact with AWS services from your command line.

### Prerequisites
- Access key and secret access key from Exercise 1
- Administrative access to your computer

### Steps

1. **Download the AWS CLI**
   - **For Windows**:
     - Go to https://aws.amazon.com/cli/
     - Download the Windows installer
     - Run the installer and follow the prompts

   - **For macOS**:
     - If you have Homebrew installed: `brew install awscli`
     - Alternatively, download from the AWS website and follow installation prompts

   - **For Linux**:
     - Ubuntu/Debian: `sudo apt-get update && sudo apt-get install awscli`
     - Amazon Linux/CentOS: `sudo yum install awscli`

2. **Verify Installation**
   - Open a command prompt or terminal
   - Run: `aws --version`
   - You should see the installed AWS CLI version

3. **Configure AWS CLI**
   - In the command prompt or terminal, run: `aws configure`
   - Enter your AWS Access Key ID when prompted
   - Enter your AWS Secret Access Key when prompted
   - Enter your default region (e.g., us-east-1, eu-west-1)
   - Enter your preferred output format (json, yaml, text, or table)

4. **Verify Configuration**
   - Run: `aws sts get-caller-identity`
   - This should display your AWS account ID, user ARN, and user ID
   - If successful, your AWS CLI is properly configured

## Exercise 3: Create an IAM Policy for S3 Read-Only Access

### Objective
Create a custom IAM policy that grants read-only access to S3 buckets.

### Steps

1. **Navigate to IAM Service**
   - In the AWS Management Console, go to the IAM service

2. **Create a Policy**
   - In the left navigation pane, click on "Policies"
   - Click "Create policy"
   - Select the JSON tab

3. **Define the Policy**
   - Replace the default policy with the following JSON:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": [
           "s3:Get*",
           "s3:List*",
           "s3-object-lambda:Get*",
           "s3-object-lambda:List*"
         ],
         "Resource": "*"
       }
     ]
   }
   ```
   - Click "Next: Tags"

4. **Add Tags (Optional)**
   - Add a key-value pair tag (e.g., Key: "Purpose", Value: "S3ReadOnly")
   - Click "Next: Review"

5. **Review and Create Policy**
   - Enter a name for the policy (e.g., "S3ReadOnlyAccess-Custom")
   - Enter a description (e.g., "Custom policy for S3 read-only access")
   - Review the policy to ensure it looks correct
   - Click "Create policy"

## Exercise 4: Attach the Policy to Your IAM User

### Objective
Attach the custom S3 read-only policy to your IAM user.

### Steps

1. **Navigate to IAM Users**
   - In the IAM console, click on "Users" in the left navigation pane
   - Click on the username you created in Exercise 1

2. **Add Permissions**
   - Click on the "Add permissions" button
   - Select "Attach existing policies directly"
   - In the search box, type the name of your custom policy ("S3ReadOnlyAccess-Custom")
   - Check the box next to the policy
   - Click "Next: Review"
   - Click "Add permissions"

3. **Verify Permissions**
   - On the user details page, click on the "Permissions" tab
   - Verify that both the original "ReadOnlyAccess" policy and your custom S3 policy are listed

## Exercise 5: Use the AWS CLI to List Available S3 Buckets

### Objective
Use the AWS CLI to list S3 buckets using your configured credentials.

### Steps

1. **Open Command Line Interface**
   - Open your command prompt or terminal

2. **List S3 Buckets**
   - Run the command: `aws s3 ls`
   - If you have any buckets in your account, they will be listed
   - If you don't have any buckets, the command will complete without output

3. **Create a Bucket (Optional)**
   - If you don't have any buckets or want to create a new one:
   - Run: `aws s3 mb s3://your-unique-bucket-name-123`
   - Replace "your-unique-bucket-name-123" with a globally unique bucket name
   - Note: If your user only has read-only access, this command will fail with an access denied error

4. **Test S3 Read Operations**
   - List the contents of a specific bucket:
   - Run: `aws s3 ls s3://bucket-name`
   - Replace "bucket-name" with an actual bucket name from your account

5. **Clean Up (Optional)**
   - If you created a bucket and want to delete it (requires write permissions):
   - First empty the bucket: `aws s3 rm s3://your-bucket-name --recursive`
   - Then delete the bucket: `aws s3 rb s3://your-bucket-name`

## Additional Challenges

1. **Explore IAM Groups**
   - Create an IAM group for developers
   - Attach appropriate policies to the group
   - Add your user to the group

2. **Set Up MFA for Your IAM User**
   - Enable Multi-Factor Authentication for your IAM user
   - Configure a virtual MFA device using an authenticator app

3. **Create a Custom IAM Role**
   - Create a role that could be assumed by EC2 instances
   - Attach the S3 read-only policy to this role

## Conclusion

In this hands-on lab, you've learned how to:
- Create IAM users with programmatic access
- Install and configure the AWS CLI
- Create custom IAM policies
- Attach policies to IAM users
- Use the AWS CLI to interact with S3

These foundational skills will be essential as you continue your AWS journey. 