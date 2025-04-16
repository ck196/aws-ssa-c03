# Module 3: Storage Services - Hands-on Lab Guide

This guide provides detailed instructions for completing the hands-on exercises in Module 3.

## Exercise 1: Create and Configure an Amazon S3 Bucket with Versioning

### Objective
Create an S3 bucket, enable versioning, and upload multiple versions of a file.

### Steps

1. **Sign in to the AWS Management Console**
   - Go to https://console.aws.amazon.com/
   - Sign in with your AWS account credentials

2. **Navigate to Amazon S3**
   - In the search bar at the top, type "S3"
   - Select "S3" from the dropdown menu

3. **Create a Bucket**
   - Click "Create bucket"
   - Enter a globally unique bucket name (e.g., "my-unique-bucket-name-12345")
   - Select the AWS Region closest to you
   - Leave all other default settings
   - Scroll down and click "Create bucket"

4. **Enable Versioning**
   - Click on your newly created bucket
   - Go to the "Properties" tab
   - Scroll down to "Bucket Versioning"
   - Click "Edit"
   - Select "Enable"
   - Click "Save changes"

5. **Upload a File - Version 1**
   - Go to the "Objects" tab of your bucket
   - Click "Upload"
   - Click "Add files" and select a text file from your computer
     - If you don't have one, create a simple text file with content "This is version 1"
   - Leave all default settings
   - Click "Upload"

6. **Modify and Upload the Same File - Version 2**
   - On your local computer, open the text file you just uploaded
   - Modify its content (e.g., change it to "This is version 2")
   - Save the file
   - In the S3 console, click "Upload" again
   - Click "Add files" and select the same text file
   - Click "Upload"

7. **Modify and Upload the Same File - Version 3**
   - Repeat the process to create and upload a third version
   - Change the content to "This is version 3"

8. **View the Version History**
   - In the S3 console, stay in the "Objects" tab of your bucket
   - Click on the file name
   - Click the "Versions" tab
   - You should see all three versions of your file listed

9. **Download Different Versions**
   - In the versions list, for each version:
     - Select the version
     - Click "Download"
   - Open each downloaded file to confirm they contain the correct content

10. **Restore a Previous Version**
    - In the versions list, select version 1
    - Click "Download"
    - On your local computer, save this as a new file
    - Upload this file to the S3 bucket (creating version 4)
    - Alternatively, you can make a copy of the older version by selecting it and using "Copy"

11. **Delete a Version**
    - In the versions list, select one of the versions
    - Click "Delete"
    - Confirm the deletion by typing "permanently delete" in the confirmation field
    - Click "Delete"
    - Notice that other versions remain intact

12. **Enable MFA Delete (Optional, requires MFA device)**
    - This step requires AWS CLI and an MFA device
    - Configure AWS CLI with your credentials
    - Run:
      ```
      aws s3api put-bucket-versioning --bucket my-unique-bucket-name-12345 --versioning-configuration Status=Enabled,MFADelete=Enabled --mfa "arn-of-mfa-device mfa-code"
      ```

13. **Clean Up (Optional)**
    - If you want to delete the bucket:
    - Go to the bucket's "Objects" tab
    - Click "Empty"
    - Type "permanently delete" to confirm
    - Click "Empty"
    - After the bucket is empty, go back to the buckets list
    - Select your bucket
    - Click "Delete"
    - Type the bucket name to confirm
    - Click "Delete bucket"

## Exercise 2: Implement S3 Lifecycle Rules and Storage Classes

### Objective
Configure S3 lifecycle rules to automatically transition objects between storage classes and eventually expire them.

### Steps

1. **Navigate to Your S3 Bucket**
   - In the AWS Management Console, go to S3
   - Click on the bucket you created in Exercise 1 (or create a new one)

2. **Upload Test Files**
   - Click "Upload"
   - Click "Add files" and select several test files (or create them)
   - Ensure at least one file is larger than 128KB (for S3 Intelligent-Tiering)
   - Click "Upload"

3. **Create a Lifecycle Rule**
   - In your bucket, go to the "Management" tab
   - Scroll down to "Lifecycle rules"
   - Click "Create lifecycle rule"

4. **Configure Basic Settings**
   - Enter a name for the rule (e.g., "TestLifecycleRule")
   - Under "Rule scope", choose whether to apply this rule to all objects or specific objects
   - If you want to apply to specific objects, you can use a prefix (e.g., "test/") or specific tags
   - Click "Next"

5. **Configure Lifecycle Rule Actions**
   - Select "Transition current versions of objects between storage classes"
   - Configure the following transitions:
     - Standard to Standard-IA: 30 days after object creation
     - Standard-IA to Glacier: 60 days after object creation
     - Glacier to Glacier Deep Archive: 180 days after object creation
   - You can also select "Expire current versions of objects" and set it to 365 days
   - Click "Next"

6. **Review and Create**
   - Review your lifecycle rule settings
   - Click "Create rule"

7. **Test the Lifecycle Rule (Simulated)**
   - Since the actual transitions would take days or months, we'll use the S3 console to manually change the storage class of an object
   - In your bucket's "Objects" tab, select an object
   - Click "Actions" → "Edit storage class"
   - Select different storage classes and note the differences in retrieval times and costs
   - Click "Save changes"

8. **View Lifecycle Rule Status**
   - Go back to the "Management" tab
   - Scroll down to "Lifecycle rules"
   - Your rule should be listed with its current status

9. **Clean Up (Optional)**
   - To delete the lifecycle rule:
   - In the "Management" tab, under "Lifecycle rules"
   - Select your rule
   - Click "Delete"
   - Confirm deletion

## Exercise 3: Set Up an S3 Static Website with Custom Error Pages

### Objective
Configure an S3 bucket for static website hosting with custom error pages.

### Steps

1. **Create a New S3 Bucket**
   - In the S3 console, click "Create bucket"
   - Enter a globally unique name (e.g., "my-static-website-12345")
   - Select a region
   - Under "Block Public Access settings for this bucket", uncheck "Block all public access"
   - Acknowledge the warning by checking the box
   - Leave other settings as default
   - Click "Create bucket"

2. **Create HTML Files**
   - On your local computer, create the following files:
   
   **index.html** (main page):
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My S3 Static Website</title>
       <style>
           body { font-family: Arial, sans-serif; margin: 40px; line-height: 1.6; }
           h1 { color: #0066cc; }
       </style>
   </head>
   <body>
       <h1>Welcome to My S3 Static Website</h1>
       <p>This website is hosted on Amazon S3.</p>
       <p><a href="nonexistent.html">Click here</a> to test the error page.</p>
   </body>
   </html>
   ```
   
   **error.html** (error page):
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Error - Page Not Found</title>
       <style>
           body { font-family: Arial, sans-serif; margin: 40px; line-height: 1.6; }
           h1 { color: #cc0000; }
       </style>
   </head>
   <body>
       <h1>404 - Page Not Found</h1>
       <p>Sorry, the page you're looking for doesn't exist.</p>
       <p><a href="index.html">Go back to the homepage</a></p>
   </body>
   </html>
   ```

3. **Upload the HTML Files**
   - In the S3 console, click on your static website bucket
   - Click "Upload"
   - Add both HTML files
   - Click "Upload"

4. **Configure Static Website Hosting**
   - In your bucket, go to the "Properties" tab
   - Scroll down to "Static website hosting"
   - Click "Edit"
   - Select "Enable"
   - For "Index document", enter "index.html"
   - For "Error document", enter "error.html"
   - Click "Save changes"
   - Note the "Bucket website endpoint" URL that appears

5. **Create a Bucket Policy for Public Access**
   - Go to the "Permissions" tab
   - Under "Bucket policy", click "Edit"
   - Paste the following policy (replace `my-static-website-12345` with your bucket name):
     ```json
     {
         "Version": "2012-10-17",
         "Statement": [
             {
                 "Sid": "PublicReadGetObject",
                 "Effect": "Allow",
                 "Principal": "*",
                 "Action": "s3:GetObject",
                 "Resource": "arn:aws:s3:::my-static-website-12345/*"
             }
         ]
     }
     ```
   - Click "Save changes"

6. **Test the Website**
   - Go back to the "Properties" tab
   - Scroll down to "Static website hosting"
   - Click on the "Bucket website endpoint" URL
   - Your website should load
   - Click the "Click here to test the error page" link
   - The custom error page should display

7. **Clean Up (Optional)**
   - To delete the static website bucket:
   - Go to the "Permissions" tab
   - Under "Bucket policy", click "Edit"
   - Delete the policy and click "Save changes"
   - Go to the bucket's "Objects" tab
   - Select all objects and click "Delete"
   - Type "permanently delete" to confirm
   - After the bucket is empty, go back to the buckets list
   - Select your bucket and click "Delete"
   - Type the bucket name to confirm
   - Click "Delete bucket"

## Exercise 4: Create and Use EBS Volumes with EC2 Instances

### Objective
Create an EBS volume, attach it to an EC2 instance, format and mount it, then create a snapshot.

### Prerequisites
- An active EC2 instance (use the steps from Module 2, Exercise 1 to create one if needed)

### Steps

1. **Create an EBS Volume**
   - In the AWS Management Console, navigate to EC2
   - In the left navigation pane, under "Elastic Block Store", click "Volumes"
   - Click "Create volume"
   - Configure the volume:
     - Volume Type: General Purpose SSD (gp2)
     - Size: 1 GiB
     - Availability Zone: Same as your EC2 instance
     - Keep other settings as default
   - Click "Create volume"

2. **Attach the Volume to Your EC2 Instance**
   - Select the newly created volume
   - Click "Actions" → "Attach volume"
   - Select your instance from the dropdown
   - Note the device name (e.g., /dev/sdf)
   - Click "Attach volume"

3. **Connect to Your EC2 Instance**
   - In the EC2 console, select your instance
   - Click "Connect"
   - Follow the instructions to connect via SSH
   
   For Linux instances, use:
   ```
   ssh -i /path/to/key.pem ec2-user@your-instance-public-ip
   ```

4. **Format the Volume**
   - Run the following command to list available disk devices:
     ```
     lsblk
     ```
   - Identify your new volume (it will have the device name from step 2, but might appear as nvme1n1 for Nitro-based instances or xvdf)
   - Format the volume with an ext4 filesystem:
     ```
     sudo mkfs -t ext4 /dev/xvdf
     ```
     (Replace xvdf with your device name)

5. **Mount the Volume**
   - Create a mount point:
     ```
     sudo mkdir /data
     ```
   - Mount the volume:
     ```
     sudo mount /dev/xvdf /data
     ```
     (Replace xvdf with your device name)
   - Set permissions:
     ```
     sudo chown ec2-user:ec2-user /data
     ```

6. **Verify the Volume is Mounted**
   - Run the following command:
     ```
     df -h
     ```
   - Look for your volume mounted at /data

7. **Create Some Test Files**
   - Create a test file on the new volume:
     ```
     echo "This is a test file on my EBS volume" > /data/testfile.txt
     ```
   - Create a few more files:
     ```
     for i in {1..5}; do echo "Test file $i" > /data/file$i.txt; done
     ```

8. **Create a Snapshot**
   - Return to the AWS Management Console
   - In the EC2 service, under "Elastic Block Store", click "Volumes"
   - Select your volume
   - Click "Actions" → "Create snapshot"
   - Add a description (e.g., "Snapshot of my data volume")
   - Click "Create snapshot"

9. **View the Snapshot**
   - In the left navigation pane, under "Elastic Block Store", click "Snapshots"
   - Find your snapshot in the list
   - Note that it's in "pending" status until completed

10. **Create a Volume from the Snapshot (Optional)**
    - Select your snapshot
    - Click "Actions" → "Create volume from snapshot"
    - Configure the new volume:
      - Volume Type: General Purpose SSD (gp2)
      - Size: 1 GiB (or larger)
      - Availability Zone: Same as your EC2 instance
    - Click "Create volume"

11. **Configure Automatic Volume Mounting on Reboot (Optional)**
    - Return to your SSH session
    - Get the UUID of your volume:
      ```
      sudo blkid
      ```
    - Note the UUID for your volume (/dev/xvdf or equivalent)
    - Edit the fstab file:
      ```
      sudo nano /etc/fstab
      ```
    - Add the following line (replace the UUID with yours):
      ```
      UUID=your-volume-uuid /data ext4 defaults,nofail 0 2
      ```
    - Save and exit (Ctrl+X, then Y, then Enter)

12. **Clean Up (Optional)**
    - Unmount the volume:
      ```
      sudo umount /data
      ```
    - In the EC2 console, select the volume
    - Click "Actions" → "Detach volume"
    - Confirm detachment
    - After detachment is complete, click "Actions" → "Delete volume"
    - Confirm deletion
    - To delete the snapshot, go to "Snapshots"
    - Select your snapshot
    - Click "Actions" → "Delete snapshot"
    - Confirm deletion

## Exercise 5: Work with EFS to Create a Shared File System

### Objective
Create an EFS file system and mount it on multiple EC2 instances to demonstrate shared storage.

### Prerequisites
- Two EC2 instances running in the same VPC (use Module 2, Exercise 1 to create them)
- Ensure they're in the same security group or that their security groups have the same rules

### Steps

1. **Create a Security Group for EFS**
   - In the AWS Management Console, navigate to EC2
   - In the left navigation pane, click "Security Groups"
   - Click "Create security group"
   - Enter a name (e.g., "EFS-SG")
   - Add a description
   - Select your VPC
   - Add an inbound rule:
     - Type: NFS
     - Source: Select the security group of your EC2 instances
   - Click "Create security group"

2. **Create an EFS File System**
   - In the AWS Management Console, navigate to EFS
   - Click "Create file system"
   - Step 1 (Configure file system access):
     - Select your VPC
     - For mount targets, select all Availability Zones where your EC2 instances are running
     - For each mount target, select the EFS security group you created
     - Click "Next"
   - Step 2 (Configure optional settings):
     - Enter a name for your file system
     - Keep default settings for performance mode and throughput mode
     - Click "Next"
   - Step 3 (Review and create):
     - Review your settings
     - Click "Create"

3. **Prepare Your EC2 Instances**
   - Connect to both EC2 instances using SSH
   - On each instance, install the NFS client:
     ```
     sudo yum install -y nfs-utils
     ```
   - Create a mount point on each instance:
     ```
     sudo mkdir /efs
     ```

4. **Mount the EFS File System on Both Instances**
   - In the EFS console, select your file system
   - Click "Attach"
   - Copy the mount command from the "Mount via DNS" section
   - On each EC2 instance, run the mount command:
     ```
     sudo mount -t nfs4 -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport fs-xxxxxxxx.efs.region.amazonaws.com:/ /efs
     ```
     (Replace the actual values with those from your copied command)
   - Check that the mount was successful:
     ```
     df -h
     ```

5. **Configure Automatic Mounting on Reboot**
   - On each instance, edit the fstab file:
     ```
     sudo nano /etc/fstab
     ```
   - Add the following line:
     ```
     fs-xxxxxxxx.efs.region.amazonaws.com:/ /efs nfs4 nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport 0 0
     ```
     (Replace with your actual file system details)
   - Save and exit (Ctrl+X, then Y, then Enter)

6. **Test File Sharing Between Instances**
   - On Instance 1, create a file in the EFS mount:
     ```
     echo "This file was created on Instance 1" | sudo tee /efs/test-file.txt
     sudo chmod 666 /efs/test-file.txt
     ```
   - On Instance 2, verify you can see and modify the file:
     ```
     cat /efs/test-file.txt
     echo "This line was added on Instance 2" >> /efs/test-file.txt
     ```
   - Return to Instance 1 and check the updated file:
     ```
     cat /efs/test-file.txt
     ```
   - You should see both lines, demonstrating that both instances are sharing the same file system

7. **Test Performance (Optional)**
   - Create a large file on the EFS mount to test performance:
     ```
     dd if=/dev/zero of=/efs/test-file bs=1M count=1024
     ```
   - Read the file to test read performance:
     ```
     dd if=/efs/test-file of=/dev/null bs=1M
     ```

8. **Clean Up (Optional)**
   - Unmount the EFS file system from both instances:
     ```
     sudo umount /efs
     ```
   - Remove the entry from /etc/fstab on both instances
   - In the EFS console, select your file system
   - Click "Delete"
   - Type the file system ID to confirm
   - Click "Confirm"
   - In the EC2 console, delete the EFS security group

## Conclusion

In this hands-on lab, you've learned how to:
- Create and configure S3 buckets with versioning
- Implement S3 lifecycle rules for efficient storage management
- Set up a static website using S3
- Create, attach, and manage EBS volumes
- Create and use EFS for shared file storage across multiple instances

These storage service skills are essential for designing cost-effective and scalable solutions in AWS. 