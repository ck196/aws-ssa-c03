# Module 4: Networking Services - Hands-on Lab Guide

This guide provides detailed instructions for completing the hands-on exercises in Module 4.

## Exercise 1: Create a Custom VPC with Public and Private Subnets

### Objective
Design and implement a VPC with both public and private subnets, including the necessary route tables and an Internet Gateway.

### Steps

1. **Sign in to the AWS Management Console**
   - Go to https://console.aws.amazon.com/
   - Sign in with your AWS account credentials

2. **Navigate to VPC Service**
   - In the search bar at the top, type "VPC"
   - Select "VPC" from the dropdown menu

3. **Create a VPC**
   - In the left navigation pane, click "Your VPCs"
   - Click "Create VPC"
   - Select "VPC only"
   - Enter a name (e.g., "MyCustomVPC")
   - For IPv4 CIDR block, enter "10.0.0.0/16"
   - Keep other settings as default
   - Click "Create VPC"

4. **Create Subnets**
   - In the left navigation pane, click "Subnets"
   - Click "Create subnet"
   - Select your newly created VPC
   - Create the following subnets:
   
     **Public Subnet:**
     - Name: "Public-Subnet-A"
     - Availability Zone: Choose the first AZ in your region
     - IPv4 CIDR block: "10.0.1.0/24"
     - Click "Add new subnet"
   
     **Private Subnet:**
     - Name: "Private-Subnet-A"
     - Availability Zone: Choose the same AZ as your public subnet
     - IPv4 CIDR block: "10.0.2.0/24"
   
   - Click "Create subnet"

5. **Create an Internet Gateway**
   - In the left navigation pane, click "Internet gateways"
   - Click "Create internet gateway"
   - Enter a name (e.g., "MyVPC-IGW")
   - Click "Create internet gateway"
   - After creation, click "Actions" → "Attach to VPC"
   - Select your custom VPC
   - Click "Attach internet gateway"

6. **Create and Configure Route Tables**
   - In the left navigation pane, click "Route tables"
   
   **Public Route Table:**
   - Click "Create route table"
   - Enter a name (e.g., "Public-RT")
   - Select your VPC
   - Click "Create route table"
   - Select the route table you just created
   - In the "Routes" tab, click "Edit routes"
   - Click "Add route"
   - For Destination, enter "0.0.0.0/0" (all traffic)
   - For Target, select "Internet Gateway" and choose your IGW
   - Click "Save changes"
   - In the "Subnet associations" tab, click "Edit subnet associations"
   - Select your public subnet
   - Click "Save associations"
   
   **Private Route Table:**
   - Click "Create route table" again
   - Enter a name (e.g., "Private-RT")
   - Select your VPC
   - Click "Create route table"
   - In the "Subnet associations" tab, click "Edit subnet associations"
   - Select your private subnet
   - Click "Save associations"

7. **Enable Auto-assign Public IP for Public Subnet**
   - Go to "Subnets" in the left navigation pane
   - Select your public subnet
   - Click "Actions" → "Edit subnet settings"
   - Check "Enable auto-assign public IPv4 address"
   - Click "Save"

8. **Test the VPC Setup (Optional)**
   - Launch an EC2 instance in the public subnet:
     - Go to the EC2 service
     - Click "Launch instance"
     - Select Amazon Linux 2 AMI
     - Choose t2.micro instance type
     - Select your VPC and public subnet
     - Enable auto-assign public IP
     - Create or select a security group that allows SSH (port 22)
     - Launch the instance
   - Connect to the instance via SSH to verify internet access
   - Try launching another instance in the private subnet and confirm it doesn't have internet access

9. **Clean Up (Optional)**
   - Terminate any EC2 instances you launched
   - Delete your custom VPC (this will also delete associated subnets, route tables, and other resources)

## Exercise 2: Create a NAT Gateway for Private Subnet Internet Access

### Objective
Implement a NAT Gateway to allow instances in a private subnet to access the internet while remaining private.

### Prerequisites
- The custom VPC with public and private subnets from Exercise 1

### Steps

1. **Navigate to VPC Service**
   - In the AWS Management Console, go to the VPC service

2. **Create a NAT Gateway**
   - In the left navigation pane, click "NAT Gateways"
   - Click "Create NAT Gateway"
   - Select your public subnet (important: NAT Gateway must be in a public subnet)
   - Click "Allocate Elastic IP" to assign an Elastic IP address
   - Enter a name (e.g., "MyVPC-NAT")
   - Click "Create NAT Gateway"
   - Wait for the NAT Gateway to be created and its status to become "Available"

3. **Update the Private Route Table**
   - In the left navigation pane, click "Route tables"
   - Select the private route table you created in Exercise 1
   - In the "Routes" tab, click "Edit routes"
   - Click "Add route"
   - For Destination, enter "0.0.0.0/0" (all traffic)
   - For Target, select "NAT Gateway" and choose your NAT Gateway
   - Click "Save changes"

4. **Test the NAT Gateway**
   - Launch an EC2 instance in the private subnet:
     - Go to the EC2 service
     - Click "Launch instance"
     - Select Amazon Linux 2 AMI
     - Choose t2.micro instance type
     - Select your VPC and private subnet
     - Create or select a security group that allows SSH (port 22) from your public subnet
     - Launch the instance
   - Launch another EC2 instance in the public subnet (if you don't already have one)
   - Connect to the public instance via SSH
   - From the public instance, SSH to the private instance using its private IP
   - On the private instance, test internet connectivity:
     ```
     ping 8.8.8.8
     curl http://example.com
     ```
   - These commands should work, indicating that the private instance can access the internet through the NAT Gateway

5. **View CloudWatch Metrics (Optional)**
   - In the VPC console, select your NAT Gateway
   - Click the "Monitoring" tab to view metrics such as:
     - Bytes processed
     - Packets processed
     - Connection attempts
     - Connection timeouts

6. **Clean Up (Optional)**
   - Terminate any EC2 instances you launched
   - Delete the NAT Gateway:
     - In the VPC console, go to "NAT Gateways"
     - Select your NAT Gateway
     - Click "Actions" → "Delete NAT Gateway"
     - Confirm deletion
   - Release the Elastic IP:
     - In the VPC console, go to "Elastic IPs"
     - Select the IP address used by your NAT Gateway
     - Click "Actions" → "Release Elastic IP address"
     - Confirm release

## Exercise 3: Configure Security Groups and Network ACLs

### Objective
Configure Security Groups and Network ACLs to control traffic to and from your VPC resources.

### Prerequisites
- The custom VPC from Exercise 1

### Steps

1. **Create Security Groups**
   - Navigate to the VPC service
   - In the left navigation pane, click "Security Groups"
   - Click "Create security group"
   
   **Web Server Security Group:**
   - Name: "WebServer-SG"
   - Description: "Allow HTTP, HTTPS, and SSH traffic"
   - VPC: Select your custom VPC
   - Inbound rules:
     - Add rule: Type = HTTP, Source = Anywhere (0.0.0.0/0)
     - Add rule: Type = HTTPS, Source = Anywhere (0.0.0.0/0)
     - Add rule: Type = SSH, Source = Your IP or specific CIDR block for security
   - Outbound rules: Keep default (all traffic allowed)
   - Click "Create security group"
   
   **Database Security Group:**
   - Name: "Database-SG"
   - Description: "Allow MySQL traffic from WebServer-SG"
   - VPC: Select your custom VPC
   - Inbound rules:
     - Add rule: Type = MySQL/Aurora, Source = Select the WebServer-SG
   - Outbound rules: Keep default (all traffic allowed)
   - Click "Create security group"

2. **Create Network ACLs**
   - In the left navigation pane, click "Network ACLs"
   - Click "Create network ACL"
   
   **Public Subnet NACL:**
   - Name: "Public-NACL"
   - VPC: Select your custom VPC
   - Click "Create network ACL"
   - Select the NACL you just created
   - Configure inbound rules:
     - Click "Inbound rules" tab, then "Edit inbound rules"
     - Add rule #100: Type = HTTP, Source = 0.0.0.0/0, Allow
     - Add rule #200: Type = HTTPS, Source = 0.0.0.0/0, Allow
     - Add rule #300: Type = SSH, Source = Your IP or specific CIDR block, Allow
     - Add rule #400: Type = Custom TCP, Port = 1024-65535, Source = 0.0.0.0/0, Allow (for return traffic)
     - Click "Save changes"
   - Configure outbound rules:
     - Click "Outbound rules" tab, then "Edit outbound rules"
     - Add rule #100: Type = HTTP, Destination = 0.0.0.0/0, Allow
     - Add rule #200: Type = HTTPS, Destination = 0.0.0.0/0, Allow
     - Add rule #300: Type = Custom TCP, Port = 1024-65535, Destination = 0.0.0.0/0, Allow (for return traffic)
     - Click "Save changes"
   - Associate with public subnet:
     - Click "Subnet associations" tab, then "Edit subnet associations"
     - Select your public subnet
     - Click "Save changes"
   
   **Private Subnet NACL:**
   - Click "Create network ACL"
   - Name: "Private-NACL"
   - VPC: Select your custom VPC
   - Click "Create network ACL"
   - Select the NACL you just created
   - Configure inbound rules:
     - Click "Inbound rules" tab, then "Edit inbound rules"
     - Add rule #100: Type = MySQL/Aurora, Source = 10.0.1.0/24 (public subnet CIDR), Allow
     - Add rule #200: Type = SSH, Source = 10.0.1.0/24 (public subnet CIDR), Allow
     - Add rule #300: Type = Custom TCP, Port = 1024-65535, Source = 0.0.0.0/0, Allow (for return traffic)
     - Click "Save changes"
   - Configure outbound rules:
     - Click "Outbound rules" tab, then "Edit outbound rules"
     - Add rule #100: Type = HTTP, Destination = 0.0.0.0/0, Allow
     - Add rule #200: Type = HTTPS, Destination = 0.0.0.0/0, Allow
     - Add rule #300: Type = Custom TCP, Port = 1024-65535, Destination = 10.0.1.0/24 (public subnet CIDR), Allow
     - Click "Save changes"
   - Associate with private subnet:
     - Click "Subnet associations" tab, then "Edit subnet associations"
     - Select your private subnet
     - Click "Save changes"

3. **Test Security Group and NACL Rules**
   - Launch an EC2 instance in the public subnet:
     - Go to the EC2 service
     - Launch an instance in your public subnet
     - Associate the WebServer-SG
     - Connect via SSH and install a web server:
       ```
       sudo yum update -y
       sudo yum install httpd -y
       sudo systemctl start httpd
       sudo systemctl enable httpd
       echo "<html><body><h1>Public Instance</h1></body></html>" | sudo tee /var/www/html/index.html
       ```
   - Launch an EC2 instance in the private subnet:
     - Launch another instance in your private subnet
     - Associate the Database-SG
   - Test connectivity:
     - Access the web server from your browser using the public IP
     - From the public instance, try to SSH to the private instance
     - From the public instance, try to connect to the private instance on port 3306 (MySQL)
       ```
       telnet private-instance-ip 3306
       ```

4. **Clean Up (Optional)**
   - Terminate the EC2 instances
   - Delete the custom security groups
   - Reset the Network ACLs to default or delete them if they're not associated with any subnets

## Exercise 4: Set Up an Application Load Balancer

### Objective
Create an Application Load Balancer to distribute traffic across multiple EC2 instances.

### Prerequisites
- Custom VPC with public subnets in at least two Availability Zones

### Steps

1. **Prepare the VPC (if needed)**
   - If you haven't already, create a second public subnet in another Availability Zone:
     - In the VPC console, click "Subnets" → "Create subnet"
     - Name: "Public-Subnet-B"
     - VPC: Select your custom VPC
     - Availability Zone: Choose a different AZ from your first public subnet
     - IPv4 CIDR: "10.0.3.0/24"
     - Click "Create subnet"
   - Associate the subnet with the public route table:
     - Select the new subnet
     - Click "Actions" → "Edit route table association"
     - Select your public route table
     - Click "Save"
   - Enable auto-assign public IP:
     - Select the new subnet
     - Click "Actions" → "Edit subnet settings"
     - Check "Enable auto-assign public IPv4 address"
     - Click "Save"

2. **Create Target Instances**
   - Launch two EC2 instances, one in each public subnet:
     - Go to the EC2 service
     - Launch an instance in Public-Subnet-A
     - Use Amazon Linux 2 AMI and t2.micro instance
     - Use the WebServer-SG from Exercise 3
     - Add the following user data script:
       ```
       #!/bin/bash
       yum update -y
       yum install httpd -y
       systemctl start httpd
       systemctl enable httpd
       echo "<html><body><h1>Server 1 in AZ $(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone)</h1></body></html>" > /var/www/html/index.html
       ```
     - Launch the instance
   - Repeat the process for Public-Subnet-B, modifying the user data script to say "Server 2" instead of "Server 1"

3. **Create a Security Group for the Load Balancer**
   - In the VPC console, go to "Security Groups"
   - Click "Create security group"
   - Name: "ALB-SG"
   - Description: "Allow HTTP and HTTPS traffic"
   - VPC: Select your custom VPC
   - Inbound rules:
     - Add rule: Type = HTTP, Source = Anywhere (0.0.0.0/0)
     - Add rule: Type = HTTPS, Source = Anywhere (0.0.0.0/0)
   - Outbound rules: Keep default (all traffic allowed)
   - Click "Create security group"

4. **Create a Target Group**
   - Go to the EC2 service
   - In the left navigation pane, under "Load Balancing", click "Target Groups"
   - Click "Create target group"
   - Choose a target type: "Instances"
   - Name: "WebServers-TG"
   - Protocol: HTTP
   - Port: 80
   - VPC: Select your custom VPC
   - Health check settings:
     - Protocol: HTTP
     - Path: /
     - Advanced health check settings: Keep defaults
   - Click "Next"
   - Select your two web server instances
   - Click "Include as pending below"
   - Click "Create target group"

5. **Create the Application Load Balancer**
   - In the EC2 console, under "Load Balancing", click "Load Balancers"
   - Click "Create load balancer"
   - Select "Application Load Balancer"
   - Basic configuration:
     - Name: "Web-ALB"
     - Scheme: Internet-facing
     - IP address type: IPv4
   - Network mapping:
     - VPC: Select your custom VPC
     - Mappings: Select both of your public subnets
   - Security groups:
     - Select the ALB-SG you created
   - Listeners and routing:
     - Protocol: HTTP
     - Port: 80
     - Default action: Forward to your WebServers-TG
   - Click "Create load balancer"

6. **Test the Load Balancer**
   - Wait for the load balancer to be provisioned (this may take a few minutes)
   - Once its state is "Active", copy the DNS name from the "Description" tab
   - Open a web browser and paste the DNS name
   - Refresh the page several times
   - You should see the page alternating between "Server 1" and "Server 2", demonstrating that the load balancer is distributing traffic

7. **View Load Balancer Metrics**
   - In the EC2 console, select your load balancer
   - Click the "Monitoring" tab to view metrics such as:
     - Request count
     - HTTP 4XX and 5XX errors
     - Latency
     - Healthy host count

8. **Create a Custom Health Check Page (Optional)**
   - Connect to each of your EC2 instances via SSH
   - Create a health check page:
     ```
     echo "<html><body><h1>Health Check OK</h1></body></html>" | sudo tee /var/www/html/health.html
     ```
   - Update the target group to use this custom health check:
     - In the EC2 console, go to "Target Groups"
     - Select your target group
     - Click "Health checks" → "Edit"
     - Change the health check path to "/health.html"
     - Click "Save changes"

9. **Clean Up (Optional)**
   - Delete the load balancer:
     - In the EC2 console, select your load balancer
     - Click "Actions" → "Delete"
     - Confirm deletion
   - Delete the target group
   - Terminate the EC2 instances

## Exercise 5: Create a CloudFront Distribution for S3 Content

### Objective
Set up an Amazon CloudFront distribution to serve content from an S3 bucket with improved performance and global reach.

### Steps

1. **Create an S3 Bucket for Content**
   - Go to the S3 service
   - Click "Create bucket"
   - Enter a globally unique name (e.g., "my-cloudfront-content-12345")
   - Select a region close to you
   - Keep other settings as default
   - Click "Create bucket"

2. **Upload Test Content**
   - Click on your new bucket
   - Click "Upload"
   - Create a simple HTML file locally:
     ```html
     <!DOCTYPE html>
     <html>
     <head>
         <title>CloudFront Distribution Test</title>
         <style>
             body { font-family: Arial, sans-serif; margin: 40px; line-height: 1.6; }
             h1 { color: #0066cc; }
         </style>
     </head>
     <body>
         <h1>Hello from CloudFront!</h1>
         <p>This content is being served through a CloudFront distribution.</p>
         <p>Current time: <span id="datetime"></span></p>
         <script>
             document.getElementById("datetime").textContent = new Date().toString();
         </script>
     </body>
     </html>
     ```
   - Save this as "index.html" and upload it to your bucket
   - Upload an image file as well (any small image)
   - Click "Upload"

3. **Make Objects Public**
   - Select all uploaded objects
   - Click "Actions" → "Make public"
   - Confirm by clicking "Make public"

4. **Create a CloudFront Distribution**
   - Go to the CloudFront service
   - Click "Create distribution"
   - Origin settings:
     - Origin domain: Select your S3 bucket
     - Origin path: Leave empty
     - Name: Keep default
     - Enable Origin Shield: No
   - Default cache behavior settings:
     - Path pattern: Keep default (*)
     - Compress objects automatically: Yes
     - Viewer protocol policy: Redirect HTTP to HTTPS
     - Allowed HTTP methods: GET, HEAD
     - Cache key and origin requests: Use legacy cache settings
     - Cache policy: CachingOptimized
   - Distribution settings:
     - Price class: Choose "Use only North America and Europe" for cost savings
     - Alternate domain names (CNAMEs): Leave empty
     - SSL certificate: Keep default (CloudFront's default certificate)
     - Default root object: index.html
     - Standard logging: Off
   - Click "Create distribution"

5. **Wait for Distribution Deployment**
   - CloudFront distribution deployment typically takes 10-15 minutes
   - The Status will change from "In Progress" to "Deployed"

6. **Test the CloudFront Distribution**
   - Once deployed, copy the Distribution Domain Name (e.g., dxxxxxxxxxxxxx.cloudfront.net)
   - Open a web browser and navigate to this domain
   - You should see your "Hello from CloudFront!" page
   - Try accessing the image file directly through the CloudFront URL as well

7. **View Cache Statistics (Optional)**
   - After your distribution has served some content, view the statistics:
   - Select your distribution
   - Click the "Monitoring" tab
   - View metrics such as:
     - Total requests
     - Data transferred
     - HTTP status codes
     - Cache hit ratio

8. **Create an Invalidation (Optional)**
   - Update your index.html file locally:
     - Change the heading to "Updated CloudFront Content!"
     - Save the file and upload it to your S3 bucket, replacing the existing file
   - Create an invalidation to clear the CloudFront cache:
     - In the CloudFront console, select your distribution
     - Click the "Invalidations" tab
     - Click "Create invalidation"
     - Enter "/index.html" as the path
     - Click "Create invalidation"
   - Wait for the invalidation to complete (Status: Completed)
   - Refresh your CloudFront URL to see the updated content

9. **Clean Up (Optional)**
   - Delete the CloudFront distribution:
     - Select your distribution
     - Click "Disable"
     - Wait for the status to change to "Disabled"
     - Click "Delete"
     - Confirm deletion
   - Delete the S3 bucket and its contents

## Conclusion

In this hands-on lab, you've learned how to:
- Design and implement a VPC with public and private subnets
- Set up a NAT Gateway for internet access from private subnets
- Configure Security Groups and Network ACLs to control traffic
- Create an Application Load Balancer to distribute traffic across instances
- Deploy a CloudFront distribution to serve content globally with improved performance

These networking skills are essential for building secure, scalable, and highly available applications in AWS. 