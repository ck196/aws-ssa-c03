# Module 2: Compute Services - Hands-on Lab Guide

This guide provides detailed instructions for completing the hands-on exercises in Module 2.

## Exercise 1: Launch an EC2 Instance Using the AWS Management Console

### Objective
Launch an Amazon EC2 instance and connect to it using SSH.

### Prerequisites
- Active AWS account
- Key pair for SSH access (we'll create one during the exercise)
- Basic knowledge of Linux commands (for connecting to the instance)

### Steps

1. **Sign in to the AWS Management Console**
   - Go to https://console.aws.amazon.com/
   - Sign in with your AWS account credentials

2. **Navigate to EC2 Service**
   - In the search bar at the top, type "EC2"
   - Select "EC2" from the dropdown menu

3. **Launch an Instance**
   - Click the "Launch instance" button
   - Enter a name for your instance (e.g., "MyWebServer")

4. **Choose an Amazon Machine Image (AMI)**
   - Under "Application and OS Images", select "Amazon Linux 2 AMI" (free tier eligible)
   - Keep the default architecture (x86)

5. **Choose an Instance Type**
   - Select "t2.micro" (free tier eligible)
   - Click "Next"

6. **Configure Key Pair**
   - Under "Key pair (login)", click "Create new key pair"
   - Enter a name for your key pair (e.g., "my-ec2-keypair")
   - Select "RSA" and ".pem" for key pair type and format
   - Click "Create key pair"
   - The private key file will download automatically (.pem file)
   - Store this file in a secure location, as you'll need it to connect to your instance

7. **Configure Network Settings**
   - Keep the default VPC and subnet
   - Select "Create security group"
   - Make sure "Allow SSH traffic from" is selected and set to "Anywhere" (for practice purposes only)
   - Select "Allow HTTP traffic from the internet" to enable web traffic

8. **Configure Storage**
   - Keep the default storage configuration (8 GB gp2 EBS volume)

9. **Review and Launch**
   - Review your instance configuration
   - Click "Launch instance"

10. **Wait for Instance to Initialize**
    - Click on "View all instances" to go to the instances page
    - Wait until your instance shows "Running" status and passes status checks
    - This typically takes 1-2 minutes

11. **Gather Connection Information**
    - Select your instance
    - In the "Details" tab, find and note the "Public IPv4 address" or "Public IPv4 DNS"

12. **Connect to Your Instance**
    
    **For Windows Users**:
    - Use PuTTY or Windows Subsystem for Linux
    - If using PuTTY, convert the .pem file to .ppk format using PuTTYgen
    - Connect using the username "ec2-user" and your key file
    
    **For macOS/Linux Users**:
    - Open Terminal
    - Change permissions on your key file:
      ```
      chmod 400 /path/to/my-ec2-keypair.pem
      ```
    - Connect using SSH:
      ```
      ssh -i /path/to/my-ec2-keypair.pem ec2-user@your-instance-public-ip
      ```

13. **Test the Connection**
    - Once connected, run the following commands to verify the system:
      ```
      whoami
      df -h
      free -m
      ```

14. **Install a Simple Web Server (Optional)**
    - Update the package repositories:
      ```
      sudo yum update -y
      ```
    - Install the Apache web server:
      ```
      sudo yum install httpd -y
      ```
    - Start and enable the web server:
      ```
      sudo systemctl start httpd
      sudo systemctl enable httpd
      ```
    - Create a simple home page:
      ```
      echo "<html><body><h1>Hello from my EC2 instance!</h1></body></html>" | sudo tee /var/www/html/index.html
      ```
    - Test the web server by visiting your instance's public IP in a web browser

15. **Clean Up**
    - Return to the EC2 console
    - Select your instance
    - Click "Instance state" → "Terminate instance"
    - Confirm termination

## Exercise 2: Create a Simple Lambda Function

### Objective
Create and test a simple AWS Lambda function that returns a "Hello World" message.

### Steps

1. **Navigate to Lambda Service**
   - In the AWS Management Console, type "Lambda" in the search bar
   - Select "Lambda" from the dropdown menu

2. **Create a Lambda Function**
   - Click "Create function"
   - Select "Author from scratch"
   - Enter a function name (e.g., "HelloWorldFunction")
   - For Runtime, select "Node.js 16.x" (or the latest version available)
   - Under "Permissions", select "Create a new role with basic Lambda permissions"
   - Click "Create function"

3. **Write the Function Code**
   - In the "Code source" section, you'll see a code editor with a file named "index.js"
   - Replace the existing code with:
     ```javascript
     exports.handler = async (event) => {
         // Log the received event
         console.log('Received event:', JSON.stringify(event, null, 2));
         
         // Prepare response
         const response = {
             statusCode: 200,
             body: JSON.stringify({
                 message: 'Hello, World!',
                 input: event
             })
         };
         
         return response;
     };
     ```
   - Click "Deploy" to save your changes

4. **Test the Function**
   - Click the "Test" tab
   - If no test event exists, you'll be prompted to create one:
     - Enter an Event name (e.g., "TestEvent")
     - Keep the default JSON template or modify it
     - Click "Save"
   - Click "Test" to execute the function
   - In the "Execution results" tab, you should see your function's response, including the "Hello, World!" message
   - You'll also see logs from the function execution

5. **Configure an API Trigger (Optional)**
   - Click "Add trigger" from the Function overview
   - Select "API Gateway" as the trigger source
   - Choose "Create a new API"
   - Select "HTTP API" as the API type
   - Set "Security" to "Open" (for practice purposes only)
   - Click "Add"
   - Once the API is created, note the API endpoint URL

6. **Test the API Endpoint (Optional)**
   - Open a new browser tab
   - Navigate to the API endpoint URL you noted earlier
   - You should see the JSON response from your Lambda function

7. **View CloudWatch Logs**
   - Scroll to the "Monitor" tab
   - Click "View CloudWatch logs"
   - Click on the most recent log stream
   - You'll see logs from your function executions, including any console.log output

8. **Clean Up**
   - Return to the Lambda function page
   - Click "Actions" → "Delete function"
   - If you created an API Gateway, navigate to the API Gateway service and delete the API as well

## Exercise 3: Configure Auto Scaling for Your EC2 Instance

### Objective
Set up an Auto Scaling group with a launch template and scaling policies to automatically adjust capacity based on demand.

### Steps

1. **Create a Launch Template**
   - In the EC2 console, click on "Launch Templates" in the left navigation pane
   - Click "Create launch template"
   - Enter a name and description (e.g., "MyWebServerTemplate", "Template for web server instances")
   - For AMI, select "Amazon Linux 2 AMI"
   - For Instance type, select "t2.micro"
   - Select your key pair
   - Under Network settings, select "Create security group"
   - Allow SSH (port 22) and HTTP (port 80) traffic
   - Expand "Advanced details"
   - In the "User data" field, add the following script to install and configure a web server:
     ```bash
     #!/bin/bash
     yum update -y
     yum install httpd -y
     systemctl start httpd
     systemctl enable httpd
     echo "<html><body><h1>Hello from Auto Scaling Group!</h1></body></html>" > /var/www/html/index.html
     ```
   - Click "Create launch template"

2. **Create an Auto Scaling Group**
   - In the EC2 console, click on "Auto Scaling Groups" in the left navigation pane
   - Click "Create Auto Scaling group"
   - Enter a name for your Auto Scaling group (e.g., "MyWebServerASG")
   - Select the launch template you just created
   - Click "Next"

3. **Configure Network Settings**
   - Select your VPC
   - Select at least two Availability Zones and one subnet per AZ
   - Click "Next"

4. **Configure Advanced Options**
   - For Load balancing, select "No load balancer" (for simplicity)
   - For Health checks, keep the default EC2 health checks
   - For Health check grace period, keep the default 300 seconds
   - Click "Next"

5. **Configure Group Size and Scaling Policies**
   - Set Desired capacity to 1
   - Set Minimum capacity to 1
   - Set Maximum capacity to 3
   - For Scaling policies, select "Target tracking scaling policy"
   - For Metric type, select "Average CPU utilization"
   - Set Target value to 50 (this means the ASG will try to maintain CPU utilization at 50%)
   - Keep the default instance warmup time
   - Click "Next"

6. **Add Notifications (Optional)**
   - Optionally, set up an SNS notification for scaling events
   - Click "Next"

7. **Add Tags**
   - Add a Name tag (e.g., Key: "Name", Value: "ASG-WebServer")
   - Click "Next"

8. **Review and Create**
   - Review all the settings
   - Click "Create Auto Scaling group"

9. **Verify Your Auto Scaling Group**
   - Go to the Auto Scaling Groups page
   - Select your newly created group
   - In the "Instances" tab, you should see that an instance has been launched
   - Wait for the instance to be in the "InService" lifecycle state

10. **Test Your Auto Scaling Group**
    - Find the public IP of the instance created by your Auto Scaling group
    - In a web browser, navigate to this IP address
    - You should see the web page with "Hello from Auto Scaling Group!"

11. **Test Scaling (Optional Advanced Exercise)**
    - Connect to your instance via SSH
    - Install the "stress" utility to simulate high CPU load:
      ```
      sudo amazon-linux-extras install epel -y
      sudo yum install stress -y
      ```
    - Run stress to increase CPU utilization:
      ```
      stress --cpu 1 --timeout 300
      ```
    - In the AWS console, monitor the CloudWatch metrics for your Auto Scaling group
    - After a few minutes, your Auto Scaling group should add instances to handle the load
    - After you stop the stress test, and after a cool-down period, the group should scale back in

12. **Clean Up**
    - In the EC2 console, go to "Auto Scaling Groups"
    - Select your Auto Scaling group
    - Click "Delete"
    - Confirm deletion
    - Go to "Launch Templates"
    - Select your launch template
    - Click "Actions" → "Delete template"
    - Confirm deletion

## Exercise 4: Deploy a Sample Application Using Elastic Beanstalk

### Objective
Deploy a simple web application using AWS Elastic Beanstalk with minimal configuration.

### Prerequisites
- Sample application code (we'll use AWS's sample application)

### Steps

1. **Navigate to Elastic Beanstalk**
   - In the AWS Management Console, type "Elastic Beanstalk" in the search bar
   - Select "Elastic Beanstalk" from the dropdown menu

2. **Create a New Application**
   - Click "Create application"
   - Enter an application name (e.g., "MyBeanstalkApp")
   - Optionally, add application tags
   - Click "Create"

3. **Create an Environment**
   - You'll be prompted to create an environment
   - Select "Web server environment"
   - Enter an Environment name (e.g., "MyBeanstalkApp-env")
   - For Domain, keep the auto-generated name or customize it
   - For Platform, select "Node.js" (or another platform of your choice)
   - For Application code, select "Sample application" (for this exercise)
   - Click "Create environment"

4. **Wait for Environment Creation**
   - Elastic Beanstalk will now create your environment
   - This includes an EC2 instance, security group, and other resources
   - The process takes about 5-10 minutes
   - You can follow the progress on the environment dashboard

5. **Explore Your Deployed Application**
   - Once the environment is created, you'll see a green checkmark and "Health: Ok"
   - Click on the URL displayed near the top of the page
   - This will open your sample application in a new browser tab

6. **Explore Monitoring and Logs**
   - In the Elastic Beanstalk console, click on the "Monitoring" tab
   - Here you can see basic metrics about your environment
   - Click on the "Logs" tab
   - Click "Request Logs" to request and download logs from your environment

7. **Update the Application (Optional)**
   - Download AWS's Node.js sample application:
     ```
     wget https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/samples/nodejs.zip
     ```
   - Unzip the file:
     ```
     unzip nodejs.zip
     ```
   - Modify the application (e.g., edit `nodejs/index.html` to change the welcome message)
   - Zip the application again:
     ```
     cd nodejs
     zip -r ../nodejs-v2.zip *
     cd ..
     ```
   - In the Elastic Beanstalk console, go to your environment
   - Click "Upload and deploy"
   - Click "Choose file" and select your nodejs-v2.zip file
   - Enter a version label (e.g., "v2")
   - Click "Deploy"
   - Wait for the deployment to complete
   - Refresh your application URL to see the changes

8. **Clean Up**
   - In the Elastic Beanstalk console, go to your environment
   - Click "Actions" → "Terminate environment"
   - Type the environment name to confirm
   - Click "Terminate"
   - Wait for the environment to be terminated
   - Navigate to the application page
   - Click "Actions" → "Delete application"
   - Type the application name to confirm
   - Click "Delete"

## Exercise 5: Create an ECS Cluster and Run a Test Container

### Objective
Create an Amazon ECS cluster and deploy a simple container application.

### Steps

1. **Navigate to ECS Service**
   - In the AWS Management Console, type "ECS" in the search bar
   - Select "Elastic Container Service" from the dropdown menu

2. **Create a Cluster**
   - Click "Create cluster"
   - Select "Networking only" (AWS Fargate) for a serverless option
   - Click "Next step"
   - Enter a cluster name (e.g., "MyECSCluster")
   - Keep all other default settings
   - Click "Create"
   - Wait for the cluster to be created

3. **Register a Task Definition**
   - In the left navigation pane, click "Task Definitions"
   - Click "Create new Task Definition"
   - Select "FARGATE" as the launch type
   - Click "Next step"
   - Enter a Task Definition Name (e.g., "nginx-task")
   - For Task Role and Task execution role, keep the default selections
   - Set Task memory to 0.5GB
   - Set Task CPU to 0.25 vCPU
   - Click "Add container"
   - Enter a Container name (e.g., "nginx")
   - For Image, enter "nginx:latest" (this will use the official Nginx image from Docker Hub)
   - Set Port mappings to 80
   - Keep all other default settings
   - Click "Add"
   - Click "Create"

4. **Run a Task**
   - Go back to the Clusters page
   - Select your cluster
   - In the "Tasks" tab, click "Run new Task"
   - For Launch type, select "FARGATE"
   - For Task Definition, select the task definition you created
   - Set Number of tasks to 1
   - For Cluster VPC, select your default VPC
   - For Subnets, select at least one subnet
   - For Security groups, click "Edit" and ensure inbound port 80 is allowed
   - Leave all other settings at their defaults
   - Click "Run Task"

5. **Monitor the Task**
   - You'll be taken to the Tasks screen
   - Your task should be in the "PROVISIONING" state, then "PENDING", and eventually "RUNNING"
   - This process may take a minute or two

6. **Access Your Container**
   - Once the task is running, select it by clicking on its task ID
   - In the "Configuration" section, find the "Public IP"
   - Open a new browser tab and navigate to this IP address
   - You should see the default Nginx welcome page

7. **Clean Up**
   - Return to the task list
   - Select your running task
   - Click "Stop"
   - Confirm by clicking "Stop" again
   - Go to the Task Definitions page
   - Select your task definition
   - Click "Deregister"
   - Select the revision and click "Deregister"
   - Go to the Clusters page
   - Select your cluster
   - Click "Delete Cluster"
   - Confirm by clicking "Delete"

## Conclusion

In this hands-on lab, you've learned how to:
- Launch and connect to an EC2 instance
- Create and test a simple Lambda function
- Configure Auto Scaling for EC2 instances
- Deploy an application using Elastic Beanstalk
- Create an ECS cluster and run a container

These compute service skills form the foundation for building scalable and resilient applications in AWS. 