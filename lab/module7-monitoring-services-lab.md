# Module 7: Monitoring and Management Services - Hands-on Lab Guide

This guide provides detailed instructions for completing the hands-on exercises in Module 7.

## Exercise 1: Set Up Amazon CloudWatch Dashboards and Alarms

### Objective
Create CloudWatch dashboards to visualize key metrics and set up alarms to notify you of potential issues.

### Steps

1. **Sign in to the AWS Management Console**
   - Go to https://console.aws.amazon.com/
   - Sign in with your AWS account credentials

2. **Navigate to CloudWatch Service**
   - In the search bar at the top, type "CloudWatch"
   - Select "CloudWatch" from the dropdown menu

3. **Create a CloudWatch Dashboard**
   - In the left navigation pane, click "Dashboards"
   - Click "Create dashboard"
   - Enter a name for your dashboard (e.g., "EC2-Monitoring")
   - Click "Create dashboard"

4. **Add Widgets to Your Dashboard**
   - You'll be prompted to add widgets
   - Click "Line" to add a line graph widget
   - In the "Metrics" tab, expand "EC2" and then "Per-Instance Metrics"
   - Find your EC2 instance (or any active instance) and select the "CPUUtilization" metric
   - Click "Create widget"
   
   - Click "Add widget" again
   - Click "Number" to add a single number widget
   - In the "Metrics" tab, expand "EC2" and then "Per-Instance Metrics"
   - Find your EC2 instance and select the "NetworkIn" metric
   - Click "Create widget"
   
   - Click "Add widget" again
   - Click "Stacked area" to add a stacked area widget
   - In the "Metrics" tab, expand "EBS" and then "Per-Volume Metrics"
   - Find your EBS volumes and select "VolumeReadBytes" and "VolumeWriteBytes" metrics
   - Click "Create widget"
   
   - Click "Add widget" again
   - Click "Text" to add a text widget
   - Enter a title and some descriptive text for your dashboard
   - Click "Create widget"

5. **Customize Your Dashboard Layout**
   - Drag and resize the widgets to arrange them as you like
   - Click "Save dashboard" when you're satisfied with the layout

6. **Create a CloudWatch Alarm for High CPU Utilization**
   - In the left navigation pane, click "Alarms"
   - Click "Create alarm"
   - Click "Select metric"
   - Navigate through "EC2" > "Per-Instance Metrics"
   - Find your EC2 instance and select "CPUUtilization"
   - Click "Select metric"
   
   - Configure the alarm conditions:
     - For "Statistic", select "Average"
     - For "Period", select "1 minute"
     - For "Threshold type", select "Static"
     - For "Whenever CPUUtilization is", select "Greater than"
     - Enter a threshold value (e.g., "70")
     - Click "Next"
   
   - Configure the alarm actions:
     - Under "Notification", select "Create new topic"
     - Enter a topic name (e.g., "high-cpu-alert")
     - Enter your email address
     - Click "Create topic"
     - Click "Next"
   
   - Add a name and description:
     - Name: "High-CPU-Alarm"
     - Description: "Alarm when CPU exceeds 70% for 5 minutes"
     - Click "Next"
   
   - Review and click "Create alarm"
   - Check your email and confirm the SNS subscription

7. **Test the CPU Alarm (Optional)**
   - Connect to your EC2 instance
   - Install the stress tool:
     ```
     sudo amazon-linux-extras install epel -y
     sudo yum install stress -y
     ```
   - Run a stress test to increase CPU:
     ```
     stress --cpu 1 --timeout 300
     ```
   - Monitor the CloudWatch console to see the alarm state change
   - Check your email for the alarm notification

8. **Create a Composite Alarm (Optional)**
   - Create a second alarm for low disk space:
     - Follow the same steps as above but select a disk space metric
     - Configure appropriate thresholds
   
   - Create a composite alarm:
     - In the Alarms page, click "Create composite alarm"
     - Enter a name (e.g., "Critical-System-Alarm")
     - In the rule section, create a rule like:
       ```
       ALARM("High-CPU-Alarm") AND ALARM("Low-Disk-Space-Alarm")
       ```
     - Configure notification actions
     - Click "Create composite alarm"

9. **Create a CloudWatch Dashboard with Cross-Region Metrics (Optional)**
   - Click "Dashboards" and create a new dashboard
   - Add a widget
   - In the Region selector (top-right), select a different region
   - Select metrics from that region
   - Add the widget to your dashboard
   - Add another widget for your current region
   - Save the dashboard
   - You now have a multi-region dashboard

10. **Clean Up (Optional)**
    - Delete the alarms you created
    - Delete the SNS topic
    - Delete the CloudWatch dashboard

## Exercise 2: Configure AWS CloudWatch Logs

### Objective
Set up CloudWatch Logs to collect, monitor, and analyze log data from EC2 instances and other AWS services.

### Prerequisites
- An EC2 instance running Amazon Linux 2

### Steps

1. **Navigate to CloudWatch Service**
   - In the AWS Management Console, go to the CloudWatch service

2. **Create an IAM Role for CloudWatch Logs**
   - Go to the IAM service
   - Click "Roles" in the left navigation pane
   - Click "Create role"
   - Select "AWS service" as the trusted entity, and "EC2" as the service
   - Click "Next: Permissions"
   - Search for and select the "CloudWatchAgentServerPolicy"
   - Click "Next: Tags"
   - (Optional) Add tags
   - Click "Next: Review"
   - Name the role "CloudWatchLogsRole"
   - Click "Create role"

3. **Attach the IAM Role to Your EC2 Instance**
   - Go to the EC2 service
   - Select your instance
   - Click "Actions" → "Security" → "Modify IAM role"
   - Select the "CloudWatchLogsRole" you created
   - Click "Update IAM role"

4. **Connect to Your EC2 Instance**
   - Connect to your EC2 instance via SSH or Session Manager
   - Install the CloudWatch Logs agent:
     ```
     sudo yum update -y
     sudo yum install -y awslogs
     ```

5. **Configure the CloudWatch Logs Agent**
   - Edit the agent configuration file:
     ```
     sudo nano /etc/awslogs/awslogs.conf
     ```
   - Add configurations for the logs you want to monitor:
     ```
     [/var/log/messages]
     datetime_format = %b %d %H:%M:%S
     file = /var/log/messages
     buffer_duration = 5000
     log_stream_name = {instance_id}/var/log/messages
     initial_position = start_of_file
     log_group_name = /ec2/var/log/messages
     
     [/var/log/secure]
     datetime_format = %b %d %H:%M:%S
     file = /var/log/secure
     buffer_duration = 5000
     log_stream_name = {instance_id}/var/log/secure
     initial_position = start_of_file
     log_group_name = /ec2/var/log/secure
     ```
   - Save and exit (Ctrl+X, then Y, then Enter)

6. **Configure the Region**
   - Edit the AWS configuration file:
     ```
     sudo nano /etc/awslogs/awscli.conf
     ```
   - Update the region parameter to match your region:
     ```
     [plugins]
     cwlogs = cwlogs
     [default]
     region = us-east-1
     ```
   - Save and exit

7. **Start the CloudWatch Logs Agent**
   - Start the agent:
     ```
     sudo systemctl start awslogsd
     ```
   - Enable it to start on boot:
     ```
     sudo systemctl enable awslogsd
     ```
   - Check the status:
     ```
     sudo systemctl status awslogsd
     ```

8. **Generate Some Log Activity**
   - Run some commands that will generate log entries, such as:
     ```
     sudo yum update -y
     sudo useradd testuser
     sudo passwd testuser
     ```

9. **Verify Logs in CloudWatch Console**
   - Return to the CloudWatch console
   - Click "Log groups" in the left navigation pane
   - Look for the log groups you configured (/ec2/var/log/messages and /ec2/var/log/secure)
   - Click on a log group and then a log stream to view the logs

10. **Create a Metric Filter and Alarm**
    - In the CloudWatch console, click "Log groups"
    - Select the "/ec2/var/log/secure" log group
    - Click "Actions" → "Create metric filter"
    - For filter pattern, enter:
      ```
      Failed password
      ```
    - Click "Next"
    - Name the filter "FailedPasswordAttempts"
    - Assign a metric namespace (e.g., "SecurityMetrics")
    - Assign a metric name (e.g., "FailedPasswordCount")
    - For metric value, enter "1"
    - Click "Next"
    - Review and click "Create metric filter"
    
    - Create an alarm for this metric:
      - Click "Create alarm"
      - Configure the alarm to trigger when there are multiple failed password attempts
      - Set up a notification action
      - Name the alarm "FailedPasswordAlarm"
      - Create the alarm

11. **Test the Failed Password Alarm (Optional)**
    - Connect to your EC2 instance
    - Attempt to log in with an incorrect password several times:
      ```
      ssh nonexistentuser@your-instance-ip
      ```
    - Monitor the CloudWatch console to see if the alarm triggers

12. **Create a Dashboard for Logs Insights**
    - In the CloudWatch console, click "Logs Insights"
    - In the query editor, enter a query like:
      ```
      fields @timestamp, @message
      | filter @message like /Failed password/
      | sort @timestamp desc
      | limit 20
      ```
    - Select your log groups and run the query
    - Click "Save"
    - Add this query to your dashboard
    - Name the widget and click "Add to dashboard"
    - Select your existing dashboard or create a new one

13. **Clean Up (Optional)**
    - Stop the CloudWatch Logs agent on your EC2 instance:
      ```
      sudo systemctl stop awslogsd
      ```
    - Delete the metric filter and alarm
    - Delete the log groups in the CloudWatch console
    - Detach the IAM role from your EC2 instance
    - Delete the IAM role

## Exercise 3: Use AWS X-Ray for Application Tracing

### Objective
Implement AWS X-Ray to trace and analyze requests through your application.

### Prerequisites
- Node.js installed
- An EC2 instance (optional for deploying the application)

### Steps

1. **Navigate to AWS X-Ray Service**
   - In the AWS Management Console, type "X-Ray" in the search bar
   - Select "X-Ray" from the dropdown menu

2. **Create an IAM Role for X-Ray**
   - Go to the IAM service
   - Click "Roles" in the left navigation pane
   - Click "Create role"
   - Select "AWS service" as the trusted entity, and "EC2" as the service
   - Click "Next: Permissions"
   - Search for and select the "AWSXRayDaemonWriteAccess" policy
   - Click "Next: Tags"
   - (Optional) Add tags
   - Click "Next: Review"
   - Name the role "XRayDaemonRole"
   - Click "Create role"

3. **Create a Simple Node.js Application with X-Ray SDK**
   - Open a terminal on your local machine
   - Create a new directory for the project:
     ```
     mkdir xray-demo
     cd xray-demo
     ```
   - Initialize a Node.js project:
     ```
     npm init -y
     ```
   - Install dependencies:
     ```
     npm install express aws-xray-sdk
     ```
   - Create an app.js file:
     ```
     nano app.js
     ```
   - Add the following code:
     ```javascript
     const express = require('express');
     const AWSXRay = require('aws-xray-sdk');
     
     // Configure X-Ray
     AWSXRay.config([AWSXRay.plugins.EC2Plugin]);
     const app = express();
     
     // Add X-Ray middleware
     app.use(AWSXRay.express.openSegment('XRayDemoApp'));
     
     // Define routes
     app.get('/', (req, res) => {
       res.send('Hello, X-Ray!');
     });
     
     app.get('/slow', (req, res) => {
       // Simulate a slow response
       setTimeout(() => {
         res.send('This was a slow response');
       }, 2000);
     });
     
     app.get('/error', (req, res) => {
       try {
         // Deliberately cause an error
         throw new Error('This is a test error');
       } catch (error) {
         // Capture the error with X-Ray
         req.segment.addError(error);
         res.status(500).send('An error occurred');
       }
     });
     
     // Close the segment
     app.use(AWSXRay.express.closeSegment());
     
     // Start the server
     const port = 3000;
     app.listen(port, () => {
       console.log(`Server running on port ${port}`);
     });
     ```
   - Save and exit

4. **Run the X-Ray Daemon Locally (For Testing)**
   - Download and run the X-Ray daemon:
     
     **For macOS:**
     ```
     curl -o xray-daemon.zip https://s3.us-east-2.amazonaws.com/aws-xray-assets.us-east-2/xray-daemon/aws-xray-daemon-macos-3.x.zip
     unzip xray-daemon.zip
     sudo ./xray-daemon -o -n us-east-1
     ```
     
     **For Linux:**
     ```
     curl -o xray-daemon.zip https://s3.us-east-2.amazonaws.com/aws-xray-assets.us-east-2/xray-daemon/aws-xray-daemon-linux-3.x.zip
     unzip xray-daemon.zip
     sudo ./xray-daemon -o -n us-east-1
     ```
     
     **For Windows:**
     ```
     Download from https://s3.us-east-2.amazonaws.com/aws-xray-assets.us-east-2/xray-daemon/aws-xray-daemon-windows-process-3.x.zip
     Unzip and run in PowerShell:
     .\xray.exe -o -n us-east-1
     ```

5. **Run the Application Locally**
   - Start the application:
     ```
     node app.js
     ```
   - Open a web browser and navigate to:
     - http://localhost:3000/ (normal response)
     - http://localhost:3000/slow (delayed response)
     - http://localhost:3000/error (error response)

6. **Deploy to EC2 (Optional)**
   - Create an EC2 instance with the X-Ray role
   - Install Node.js on the instance:
     ```
     curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -
     sudo yum install -y nodejs
     ```
   - Transfer your application to the EC2 instance
   - Install dependencies:
     ```
     npm install
     ```
   - Install and run the X-Ray daemon:
     ```
     curl -o xray-daemon.zip https://s3.us-east-2.amazonaws.com/aws-xray-assets.us-east-2/xray-daemon/aws-xray-daemon-linux-3.x.zip
     unzip xray-daemon.zip
     sudo ./xray-daemon -o -n us-east-1 &
     ```
   - Run your application:
     ```
     node app.js &
     ```
   - Configure security group to allow inbound traffic on port 3000
   - Access your application using the EC2 public IP

7. **View Traces in the X-Ray Console**
   - Return to the AWS X-Ray console
   - Click "Traces" in the left navigation pane
   - View the service map showing your application
   - Click on individual traces to see details
   - Analyze latency, errors, and request flow

8. **Explore X-Ray Analytics**
   - In the X-Ray console, click "Analytics"
   - Explore response time distribution
   - View error rates and types
   - Use filters to narrow down specific traces

9. **Add Custom Annotations and Metadata**
   - Modify your app.js file to include custom annotations:
     ```javascript
     app.get('/slow', (req, res) => {
       // Add custom annotation
       AWSXRay.getSegment().addAnnotation('SlowEndpoint', 'true');
       
       // Add custom metadata
       AWSXRay.getSegment().addMetadata('RequestInfo', {
         requestTime: new Date().toISOString(),
         processingDelay: '2000ms'
       });
       
       // Simulate a slow response
       setTimeout(() => {
         res.send('This was a slow response');
       }, 2000);
     });
     ```
   - Restart your application
   - Generate some traffic to the /slow endpoint
   - View the custom annotations and metadata in the X-Ray console

10. **Create Groups and Filter Expressions**
    - In the X-Ray console, click "Groups" in the left navigation pane
    - Click "Create group"
    - Enter a name (e.g., "SlowRequests")
    - Enter a filter expression:
      ```
      annotation.SlowEndpoint = "true"
      ```
    - Click "Create group"
    - Browse traces specific to this group

11. **Clean Up (Optional)**
    - Stop your application
    - Stop the X-Ray daemon
    - Delete the IAM role
    - Terminate the EC2 instance if created

## Exercise 4: Implement AWS Config for Resource Compliance

### Objective
Set up AWS Config to monitor and assess resource compliance with predefined rules.

### Steps

1. **Navigate to AWS Config Service**
   - In the AWS Management Console, type "Config" in the search bar
   - Select "AWS Config" from the dropdown menu

2. **Set Up AWS Config**
   - You may see a "Get started" button if this is your first time using AWS Config
   - Click "Get started" or "Settings" in the left navigation pane
   - Configure the recording settings:
     - Under "Resource types to record", select "Record all resources supported in this region"
     - Under "AWS Config role", select "Use an existing AWS Config service-linked role" or create a new role
     - Under "Delivery method", select "Create a bucket" to store configuration history
     - (Optional) Configure Amazon SNS topic for configuration changes
   - Click "Next"

3. **Select Rules to Include**
   - AWS Config will display a list of managed rules you can enable
   - Select rules relevant to your environment, such as:
     - "ec2-instance-no-public-ip" - Checks if EC2 instances have public IPs
     - "encrypted-volumes" - Checks if EBS volumes are encrypted
     - "s3-bucket-public-read-prohibited" - Checks for S3 buckets with public read access
     - "iam-password-policy" - Checks if the account password policy meets specified requirements
   - Click "Next"

4. **Review and Confirm**
   - Review your configuration settings
   - Click "Confirm"
   - Wait for AWS Config to initialize (this may take a few minutes)

5. **Explore the Dashboard**
   - Once AWS Config is set up, explore the dashboard
   - View the compliance status of your resources
   - Check which resources are non-compliant and why

6. **Investigate Non-Compliant Resources**
   - Click on a non-compliant rule
   - View the list of resources that are non-compliant with this rule
   - Select a resource to see details about why it's non-compliant
   - Use the timeline to view configuration changes for the resource

7. **Create a Custom Rule (Optional)**
   - Click "Rules" in the left navigation pane
   - Click "Add rule" and then "Create custom rule"
   - Select "AWS Lambda"
   - Create a Lambda function for your custom rule or use an existing one
   - Configure the rule parameters
   - Set a scope for the resources the rule will evaluate
   - Click "Save"

8. **Set Up Conformance Packs**
   - Click "Conformance packs" in the left navigation pane
   - Click "Deploy conformance pack"
   - You can select from AWS sample conformance packs, such as:
     - "Operational Best Practices for PCI DSS"
     - "Operational Best Practices for CIS Level 1"
   - Click "Next"
   - Enter a name for the conformance pack
   - Click "Deploy conformance pack"
   - Monitor the compliance status in the conformance packs dashboard

9. **Set Up Remediation Actions (Optional)**
   - Click "Rules" in the left navigation pane
   - Select a non-compliant rule
   - Click "Actions" → "Manage remediation"
   - Select "Automatic remediation"
   - Choose a remediation action from the AWS Systems Manager documents
   - Configure the parameters for the remediation action
   - Click "Save"

10. **View the Resource Inventory**
    - Click "Resources" in the left navigation pane
    - View the list of all resources being tracked by AWS Config
    - Use filters to find specific resource types or compliance status
    - Click on a resource to view its configuration history

11. **View Configuration Timeline**
    - Select a resource from the resource inventory
    - Click on the "Resource Timeline" tab
    - View the configuration changes over time
    - Compare configurations between different time periods

12. **Clean Up (Optional)**
    - To stop AWS Config:
      - Go to "Settings" in the left navigation pane
      - Click "Turn off recording"
      - Confirm by clicking "Continue"
    - Delete conformance packs and custom rules if created
    - (Optional) Delete the S3 bucket storing configuration history

## Exercise 5: Set Up AWS Systems Manager for Instance Management

### Objective
Configure and use AWS Systems Manager to manage and maintain EC2 instances.

### Prerequisites
- EC2 instances with the SSM Agent installed (Amazon Linux 2 and most recent AMIs have it pre-installed)

### Steps

1. **Navigate to Systems Manager Service**
   - In the AWS Management Console, type "Systems Manager" in the search bar
   - Select "AWS Systems Manager" from the dropdown menu

2. **Create an IAM Role for Systems Manager**
   - Go to the IAM service
   - Click "Roles" in the left navigation pane
   - Click "Create role"
   - Select "AWS service" as the trusted entity, and "EC2" as the service
   - Click "Next: Permissions"
   - Search for and select "AmazonSSMManagedInstanceCore"
   - (Optional) Add additional policies like "AmazonEC2ReadOnlyAccess" for more functionality
   - Click "Next: Tags"
   - (Optional) Add tags
   - Click "Next: Review"
   - Name the role "SSMInstanceRole"
   - Click "Create role"

3. **Attach the IAM Role to EC2 Instances**
   - Go to the EC2 service
   - Select your instance(s)
   - Click "Actions" → "Security" → "Modify IAM role"
   - Select the "SSMInstanceRole" you created
   - Click "Update IAM role"

4. **Verify Managed Instances**
   - Return to the Systems Manager console
   - Click "Fleet Manager" or "Managed Instances" in the left navigation pane
   - Your EC2 instances should appear in the list (this may take a few minutes)
   - If they don't appear, check that:
     - The SSM Agent is installed and running
     - The instance has the correct IAM role
     - The instance has outbound internet access

5. **Use Run Command to Execute a Command**
   - In the Systems Manager console, click "Run Command" in the left navigation pane
   - Click "Run command"
   - Under "Command document", search for and select "AWS-RunShellScript" (for Linux) or "AWS-RunPowerShellScript" (for Windows)
   - Under "Command parameters", enter a simple command:
     
     **For Linux:**
     ```
     echo "Hello from Systems Manager" > /tmp/ssm-test.txt
     cat /tmp/ssm-test.txt
     ```
     
     **For Windows:**
     ```
     Write-Host "Hello from Systems Manager"
     ```
   - Under "Targets", select your instances
   - (Optional) Configure output options to send command output to an S3 bucket or CloudWatch Logs
   - Click "Run"
   - Monitor the command status and results

6. **Create and Run an Automation Document**
   - Click "Documents" in the left navigation pane
   - Click "Create document"
   - Select "Automation" as the document type
   - Enter a name (e.g., "UpdateAndReboot")
   - In the "Document editor" tab, paste the following YAML:
     ```yaml
     ---
     schemaVersion: '0.3'
     description: Update EC2 instance and reboot if needed
     parameters:
       InstanceId:
         type: String
         description: The ID of the instance to update
     mainSteps:
     - name: UpdateInstance
       action: aws:runCommand
       inputs:
         DocumentName: AWS-RunShellScript
         InstanceIds:
         - "{{ InstanceId }}"
         Parameters:
           commands:
           - sudo yum update -y
     - name: CheckIfRebootNeeded
       action: aws:runCommand
       inputs:
         DocumentName: AWS-RunShellScript
         InstanceIds:
         - "{{ InstanceId }}"
         Parameters:
           commands:
           - if [ -f /var/run/reboot-required ]; then echo "reboot_required=true"; else echo "reboot_required=false"; fi
       outputs:
       - Name: RebootRequired
         Selector: $.Output
         Type: String
     - name: RebootIfNeeded
       action: aws:branch
       inputs:
         Choices:
         - NextStep: RebootInstance
           Variable: "{{ CheckIfRebootNeeded.RebootRequired }}"
           Contains: "reboot_required=true"
       isEnd: true
     - name: RebootInstance
       action: aws:executeAwsApi
       inputs:
         Service: ec2
         Api: RebootInstances
         InstanceIds:
         - "{{ InstanceId }}"
       isEnd: true
     ```
   - Click "Create document"
   
   - To run the automation:
     - Click "Automation" in the left navigation pane
     - Click "Execute automation"
     - Select your custom document
     - Enter the instance ID parameter
     - Click "Execute"
     - Monitor the automation execution

7. **Create a Maintenance Window**
   - Click "Maintenance Windows" in the left navigation pane
   - Click "Create maintenance window"
   - Enter a name (e.g., "WeeklyPatching")
   - Set the schedule:
     - Select "CRON/Rate expression"
     - Enter a schedule expression (e.g., "cron(0 2 ? * SUN *)" for 2:00 AM every Sunday)
   - Set the duration (e.g., 2 hours)
   - Set the stop initiating cutoff (e.g., 1 hour)
   - (Optional) Enable "Allow unregistered targets"
   - Click "Create maintenance window"
   
   - Register targets:
     - Select your new maintenance window
     - Click the "Targets" tab
     - Click "Register targets"
     - Select "Specify instance tags" and enter tags that match your instances
     - Click "Register target"
   
   - Register tasks:
     - Click the "Tasks" tab
     - Click "Register tasks"
     - Select "Run Command" as the task type
     - Select "AWS-RunPatchBaseline" as the document
     - Configure the parameters:
       - Operation: Install
       - Reboot option: RebootIfNeeded
     - Click "Register task"

8. **Set Up Patch Manager**
   - Click "Patch Manager" in the left navigation pane
   - Click "Configure patching"
   - Select instances by tags or manually select them
   - Select the patching schedule (use your maintenance window)
   - Configure patch baselines or use the default
   - Click "Configure patching"

9. **Use Session Manager to Connect to Instances**
   - Click "Session Manager" in the left navigation pane
   - Click "Start session"
   - Select an instance
   - Click "Start session"
   - A terminal window will open in your browser
   - Execute some commands:
     ```
     whoami
     pwd
     ls -la
     ```
   - Type "exit" to end the session

10. **Create a Parameter Store Parameter**
    - Click "Parameter Store" in the left navigation pane
    - Click "Create parameter"
    - Enter a name (e.g., "/myapp/database/endpoint")
    - Select "String" as the type
    - Enter a value (e.g., "mydb.example.com")
    - (Optional) Add a description
    - Click "Create parameter"
    
    - Create a secure string parameter:
      - Click "Create parameter" again
      - Enter a name (e.g., "/myapp/database/password")
      - Select "SecureString" as the type
      - Keep the default KMS key or select a custom one
      - Enter a value (e.g., "MySecurePassword123!")
      - Click "Create parameter"
    
    - Access the parameter from an EC2 instance via Session Manager:
      ```
      aws ssm get-parameter --name "/myapp/database/endpoint" --region us-east-1
      aws ssm get-parameter --name "/myapp/database/password" --with-decryption --region us-east-1
      ```

11. **Set Up State Manager for Configuration Management**
    - Click "State Manager" in the left navigation pane
    - Click "Create association"
    - Select "AWS-RunShellScript" as the document
    - Enter commands to enforce a specific state, such as:
      ```
      #!/bin/bash
      # Ensure the Apache web server is installed and running
      if ! rpm -q httpd > /dev/null; then
        yum install -y httpd
      fi
      systemctl enable httpd
      systemctl start httpd
      ```
    - Select your target instances
    - Set a schedule (e.g., run every day)
    - Click "Create association"

12. **Clean Up (Optional)**
    - Delete maintenance windows and associated tasks
    - Delete State Manager associations
    - Delete Parameter Store parameters
    - Remove Systems Manager IAM role from instances

## Conclusion

In this hands-on lab, you've learned how to:
- Create CloudWatch dashboards and alarms to monitor AWS resources
- Configure CloudWatch Logs to collect and analyze log data
- Implement AWS X-Ray for distributed application tracing
- Use AWS Config to assess resource compliance
- Manage EC2 instances with AWS Systems Manager

These monitoring and management skills are essential for operating and maintaining reliable and secure AWS environments. By properly monitoring your resources, you can identify issues before they impact users, maintain compliance with organizational policies, and automate routine maintenance tasks. 