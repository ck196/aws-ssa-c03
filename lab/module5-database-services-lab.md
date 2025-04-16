# Module 5: Database Services - Hands-on Lab Guide

This guide provides detailed instructions for completing the hands-on exercises in Module 5.

## Exercise 1: Create and Configure an Amazon RDS MySQL Database

### Objective
Launch an Amazon RDS MySQL instance, connect to it, and perform basic database operations.

### Steps

1. **Sign in to the AWS Management Console**
   - Go to https://console.aws.amazon.com/
   - Sign in with your AWS account credentials

2. **Navigate to RDS Service**
   - In the search bar at the top, type "RDS"
   - Select "RDS" from the dropdown menu

3. **Create a DB Subnet Group**
   - In the left navigation pane, click "Subnet groups"
   - Click "Create DB Subnet Group"
   - Enter a name (e.g., "my-db-subnet-group")
   - Enter a description
   - Select your VPC (use the default VPC if you don't have a custom one)
   - Add subnets from at least two different Availability Zones
   - Click "Create"

4. **Create a Security Group for RDS**
   - Go to the EC2 service
   - In the left navigation pane, click "Security Groups"
   - Click "Create security group"
   - Enter a name (e.g., "rds-mysql-sg")
   - Add a description (e.g., "Security group for RDS MySQL")
   - Select your VPC
   - Add an inbound rule:
     - Type: MySQL/Aurora (port 3306)
     - Source: Your IP address (for security)
   - Click "Create security group"

5. **Create an RDS MySQL Instance**
   - Return to the RDS console
   - Click "Create database"
   - Choose "Standard create"
   - Select "MySQL" as the engine type
   - For Version, select the latest available version
   - Under Templates, select "Free tier" (for practice purposes)
   - Set up DB instance identifier (e.g., "my-mysql-db")
   - Configure credentials:
     - Master username: admin (or your preferred username)
     - Master password: Create a secure password and note it down
   - Under Instance configuration, keep the defaults (db.t2.micro or db.t3.micro)
   - Under Storage, keep the defaults
   - Under Connectivity:
     - Select your VPC
     - Select the DB subnet group you created
     - For Public access, select "No" (for security)
     - For VPC security group, select "Choose existing" and pick the security group you created
     - For Availability Zone, keep "No preference"
   - Under Additional configuration:
     - Enter an initial database name (e.g., "testdb")
     - Keep other defaults
   - Uncheck "Enable automated backups" to save time (for practice only)
   - Click "Create database"

6. **Create an EC2 Instance to Connect to the Database**
   - Go to the EC2 service
   - Click "Launch instance"
   - Name your instance (e.g., "DB-Client")
   - Select Amazon Linux 2 AMI
   - Choose t2.micro instance type
   - Create or select a key pair
   - Configure network settings:
     - Select the same VPC as your RDS instance
     - Enable auto-assign public IP
     - Create a security group that allows SSH (port 22) from your IP
   - Click "Launch instance"

7. **Modify the RDS Security Group**
   - Go back to the EC2 service, then to Security Groups
   - Select the RDS security group you created
   - Edit inbound rules
   - Add a rule:
     - Type: MySQL/Aurora (port 3306)
     - Source: Select the security group of your EC2 instance
   - Save rules

8. **Connect to the EC2 Instance**
   - Once the EC2 instance is running, connect to it via SSH:
     ```
     ssh -i /path/to/your-key.pem ec2-user@your-ec2-public-ip
     ```

9. **Install MySQL Client on the EC2 Instance**
   - Update the package manager:
     ```
     sudo yum update -y
     ```
   - Install the MySQL client:
     ```
     sudo yum install mysql -y
     ```

10. **Get RDS Endpoint Information**
    - Go back to the RDS console
    - Click "Databases"
    - Select your MySQL database
    - In the "Connectivity & security" tab, note the endpoint and port

11. **Connect to the RDS Database**
    - From your EC2 instance, connect to the MySQL database:
      ```
      mysql -h your-rds-endpoint -P 3306 -u admin -p
      ```
    - Enter the password when prompted
    - If successful, you'll see the MySQL prompt

12. **Perform Basic Database Operations**
    - At the MySQL prompt, run the following commands to explore and manipulate data:
      ```sql
      -- Show existing databases
      SHOW DATABASES;
      
      -- Use the database you created
      USE testdb;
      
      -- Create a new table
      CREATE TABLE employees (
          id INT AUTO_INCREMENT PRIMARY KEY,
          first_name VARCHAR(50),
          last_name VARCHAR(50),
          email VARCHAR(100),
          department VARCHAR(50),
          hire_date DATE
      );
      
      -- Insert sample data
      INSERT INTO employees (first_name, last_name, email, department, hire_date)
      VALUES 
          ('John', 'Doe', 'john.doe@example.com', 'Engineering', '2020-01-15'),
          ('Jane', 'Smith', 'jane.smith@example.com', 'Marketing', '2019-05-20'),
          ('Bob', 'Johnson', 'bob.johnson@example.com', 'Finance', '2021-03-10');
      
      -- Query the data
      SELECT * FROM employees;
      
      -- Filter data
      SELECT * FROM employees WHERE department = 'Engineering';
      
      -- Update data
      UPDATE employees SET department = 'Product' WHERE id = 1;
      SELECT * FROM employees;
      
      -- Delete data
      DELETE FROM employees WHERE id = 3;
      SELECT * FROM employees;
      ```
    - Exit the MySQL client when finished:
      ```
      EXIT;
      ```

13. **Create a DB Snapshot (Optional)**
    - In the RDS console, select your database
    - Click "Actions" → "Take snapshot"
    - Enter a snapshot name
    - Click "Take snapshot"

14. **Clean Up (Optional)**
    - In the RDS console, select your database
    - Click "Actions" → "Delete"
    - Uncheck "Create final snapshot" (for practice purposes only)
    - Type "delete me" in the confirmation field
    - Click "Delete"
    - Terminate your EC2 instance
    - Delete the security groups you created

## Exercise 2: Create and Query an Amazon DynamoDB Table

### Objective
Create a DynamoDB table, insert items, and perform various query operations.

### Steps

1. **Navigate to DynamoDB Service**
   - In the AWS Management Console, type "DynamoDB" in the search bar
   - Select "DynamoDB" from the dropdown menu

2. **Create a DynamoDB Table**
   - Click "Create table"
   - Enter a table name (e.g., "Users")
   - For Partition key, enter "UserID" with type "String"
   - For Sort key, enter "LastName" with type "String"
   - Leave default settings (on-demand capacity mode is simpler for testing)
   - Click "Create table"

3. **Add Items to the Table**
   - Once the table is created, select it
   - Click the "Actions" button, then "Create item"
   - Enter values for your first item:
     - UserID: "U001" (String)
     - LastName: "Smith" (String)
   - Click "Add new attribute" to add additional attributes:
     - FirstName: "John" (String)
     - Email: "john.smith@example.com" (String)
     - Age: 35 (Number)
     - Active: true (Boolean)
   - Click "Create item"
   
   - Create several more items with the following values:
   
     **Second User:**
     - UserID: "U002"
     - LastName: "Johnson"
     - FirstName: "Mary"
     - Email: "mary.johnson@example.com"
     - Age: 28
     - Active: true
     
     **Third User:**
     - UserID: "U001"
     - LastName: "Davis"
     - FirstName: "Robert"
     - Email: "robert.davis@example.com"
     - Age: 42
     - Active: false
     
     **Fourth User:**
     - UserID: "U003"
     - LastName: "Wilson"
     - FirstName: "Sarah"
     - Email: "sarah.wilson@example.com"
     - Age: 31
     - Active: true

4. **Query the Table**
   - Click the "Explore table items" button
   - In the "Operation type" dropdown, select "Query"
   - For Partition key value, enter "U001"
   - Click "Run"
   - You should see the two items with UserID "U001" (Smith and Davis)

5. **Filter Query Results**
   - With "Query" still selected, enter "U001" as the Partition key value
   - In the "Additional filters" section, click "Add filter"
   - Select "Active" as the attribute, "=" as the operator, and enter "true" (Boolean) as the value
   - Click "Run"
   - You should now only see the active user (Smith) with UserID "U001"

6. **Perform a Scan**
   - Change the "Operation type" to "Scan"
   - In the "Filter" section, click "Add filter"
   - Select "Age" as the attribute, ">" as the operator, and enter "30" as the value
   - Click "Run"
   - You should see users older than 30 (Smith, Davis, Wilson)

7. **Create a Global Secondary Index (GSI)**
   - Go back to the table details
   - Click on the "Indexes" tab
   - Click "Create index"
   - For Partition key, select "Active" with type "Boolean"
   - For Sort key, select "Age" with type "Number"
   - Enter an index name (e.g., "ActiveAgeIndex")
   - Keep other settings as default
   - Click "Create index"
   - Wait for the index to be created (this might take a few minutes)

8. **Query Using the GSI**
   - Once the index is created, go back to the "Explore table items" page
   - In the "Operation type" dropdown, select "Query"
   - In the "Index" dropdown, select your newly created GSI "ActiveAgeIndex"
   - For Partition key value, enter "true" (Boolean)
   - In the "Additional filters" section, add a filter for Age >= 30
   - Click "Run"
   - You should see active users who are 30 or older (Smith and Wilson)

9. **Update an Item**
   - Go back to the "Explore table items" page
   - Select "Scan" as the operation type
   - Find and select the item with UserID "U002" (Mary Johnson)
   - Click "Actions" → "Edit item"
   - Change the Age to 40
   - Click "Save changes"

10. **Delete an Item**
    - In the "Explore table items" page
    - Select "Scan" as the operation type
    - Find and select an item (e.g., the one with UserID "U003")
    - Click "Actions" → "Delete item"
    - Confirm deletion

11. **Work with DynamoDB Using AWS CLI (Optional)**
    - If you have AWS CLI configured, you can perform operations from the command line:
    
      **List tables:**
      ```
      aws dynamodb list-tables
      ```
      
      **Get item:**
      ```
      aws dynamodb get-item \
          --table-name Users \
          --key '{"UserID":{"S":"U001"},"LastName":{"S":"Smith"}}'
      ```
      
      **Put item:**
      ```
      aws dynamodb put-item \
          --table-name Users \
          --item '{
              "UserID": {"S": "U004"},
              "LastName": {"S": "Brown"},
              "FirstName": {"S": "Michael"},
              "Email": {"S": "michael.brown@example.com"},
              "Age": {"N": "45"},
              "Active": {"BOOL": true}
          }'
      ```

12. **Enable Point-in-Time Recovery (Optional)**
    - Go to your DynamoDB table
    - Click the "Backups" tab
    - Under "Point-in-time recovery", click "Edit"
    - Select "Enable"
    - Click "Save"

13. **Clean Up (Optional)**
    - To delete the table, go to the DynamoDB dashboard
    - Select your table
    - Click "Delete"
    - Type the table name to confirm
    - Click "Delete"

## Exercise 3: Set Up an Amazon ElastiCache Redis Cluster

### Objective
Create an ElastiCache Redis cluster, connect to it, and perform basic caching operations.

### Prerequisites
- VPC with at least two subnets in different Availability Zones
- EC2 instance for connecting to the Redis cluster

### Steps

1. **Navigate to ElastiCache Service**
   - In the AWS Management Console, type "ElastiCache" in the search bar
   - Select "ElastiCache" from the dropdown menu

2. **Create a Subnet Group**
   - In the left navigation pane, click "Subnet Groups"
   - Click "Create subnet group"
   - Enter a name (e.g., "redis-subnet-group")
   - Enter a description
   - Select your VPC
   - Select at least two subnets in different Availability Zones
   - Click "Create"

3. **Create a Security Group for Redis**
   - Go to the EC2 service
   - Click "Security Groups" in the left navigation pane
   - Click "Create security group"
   - Enter a name (e.g., "redis-sg")
   - Add a description
   - Select your VPC
   - Add an inbound rule:
     - Type: Custom TCP
     - Port: 6379 (Redis default port)
     - Source: The security group of your EC2 instance (or CIDR range)
   - Click "Create security group"

4. **Create an ElastiCache Redis Cluster**
   - Return to the ElastiCache console
   - Click "Redis" in the left navigation pane
   - Click "Create"
   - For Cluster mode, select "Disabled" (simpler for testing)
   - Enter a name (e.g., "my-redis-cluster")
   - For Description, enter a brief description
   - For Engine version compatibility, keep the default
   - For Port, keep the default (6379)
   - For Parameter group, keep the default
   - For Node type, select "cache.t2.micro" (Free tier eligible)
   - For Number of replicas, select 0 for simplicity
   - Under Advanced Redis settings:
     - For Subnet group, select the subnet group you created
     - For Security, select the security group you created
   - Keep other settings as default
   - Click "Create"

5. **Launch an EC2 Instance to Connect to Redis (if you don't have one)**
   - Go to the EC2 service
   - Click "Launch instance"
   - Select Amazon Linux 2 AMI
   - Choose t2.micro instance type
   - Select a key pair
   - Configure networking:
     - Select the same VPC as your Redis cluster
     - Enable auto-assign public IP
   - Configure security group to allow SSH (port 22) from your IP
   - Click "Launch instance"

6. **Get ElastiCache Endpoint Information**
   - Once your Redis cluster is available, click on its name
   - Note the "Primary Endpoint" (it should look like: my-redis-cluster.xxxxx.0001.use1.cache.amazonaws.com)

7. **Connect to Your EC2 Instance**
   - Use SSH to connect to your instance:
     ```
     ssh -i /path/to/your-key.pem ec2-user@your-ec2-public-ip
     ```

8. **Install Redis CLI on the EC2 Instance**
   - Update packages:
     ```
     sudo yum update -y
     ```
   - Install Redis CLI:
     ```
     sudo amazon-linux-extras install redis6
     ```

9. **Connect to Your Redis Cluster**
   - Use the Redis CLI to connect to your cluster:
     ```
     redis-cli -h your-redis-endpoint -p 6379
     ```
   - If successful, you'll see the Redis prompt: `your-redis-endpoint:6379>`

10. **Test Basic Redis Operations**
    - At the Redis prompt, try the following commands:
    
      **Set a key-value pair:**
      ```
      SET user:1 "John Smith"
      ```
      
      **Get a value:**
      ```
      GET user:1
      ```
      
      **Set a hash:**
      ```
      HMSET user:2 name "Mary Johnson" email "mary@example.com" age 28
      ```
      
      **Get hash fields:**
      ```
      HGETALL user:2
      ```
      
      **Set an expiration on a key:**
      ```
      SET session:123 "user_data" EX 60
      ```
      (This sets the key to expire in 60 seconds)
      
      **Check time to live for a key:**
      ```
      TTL session:123
      ```
      
      **Check if a key exists:**
      ```
      EXISTS user:1
      ```
      
      **Delete a key:**
      ```
      DEL user:1
      ```
      
      **Try to get the deleted key:**
      ```
      GET user:1
      ```
    
    - Exit the Redis CLI when finished:
      ```
      exit
      ```

11. **Create a Python Script to Interact with Redis (Optional)**
    - Create a file on your EC2 instance:
      ```
      nano redis_test.py
      ```
    - Add the following content:
      ```python
      import redis
      import time
      
      # Install Redis Python client if not already installed
      # pip install redis
      
      # Connect to your Redis cluster
      r = redis.Redis(
          host='your-redis-endpoint', 
          port=6379
      )
      
      # Set a string value
      r.set('test_key', 'Hello from Python!')
      
      # Get the value
      value = r.get('test_key')
      print(f"Retrieved value: {value.decode('utf-8')}")
      
      # Set a key with expiration
      r.setex('expiring_key', 10, 'This will expire in 10 seconds')
      
      # Check if key exists
      print(f"Key exists: {r.exists('expiring_key')}")
      
      # Wait for expiration
      print("Waiting for key to expire...")
      time.sleep(11)
      
      # Check if key still exists
      print(f"Key exists after expiration: {r.exists('expiring_key')}")
      
      # Working with hashes
      r.hset('user:profile', mapping={
          'name': 'Alice',
          'email': 'alice@example.com',
          'age': '30'
      })
      
      # Get all fields from hash
      user = r.hgetall('user:profile')
      print("User profile:")
      for key, value in user.items():
          print(f"  {key.decode('utf-8')}: {value.decode('utf-8')}")
      
      # Clean up
      r.delete('test_key', 'user:profile')
      print("Cleaned up keys")
      ```
    - Save and exit (Ctrl+X, then Y, then Enter)
    - Install Python Redis client:
      ```
      sudo pip3 install redis
      ```
    - Replace 'your-redis-endpoint' with your actual Redis endpoint in the script
    - Run the script:
      ```
      python3 redis_test.py
      ```

12. **Clean Up (Optional)**
    - In the ElastiCache console, select your Redis cluster
    - Click "Delete"
    - Confirm deletion
    - Delete the subnet group you created
    - Delete the security group
    - Terminate your EC2 instance if no longer needed

## Exercise 4: Create an Amazon Aurora Database Cluster

### Objective
Create an Amazon Aurora MySQL-compatible cluster with a primary instance and a read replica, and test failover.

### Steps

1. **Navigate to RDS Service**
   - In the AWS Management Console, type "RDS" in the search bar
   - Select "RDS" from the dropdown menu

2. **Create a DB Subnet Group (if you don't already have one)**
   - Follow the steps from Exercise 1 to create a DB subnet group

3. **Create a Security Group for Aurora**
   - Follow the steps from Exercise 1 to create a security group for Aurora (MySQL port 3306)

4. **Create an Aurora Cluster**
   - In the RDS console, click "Create database"
   - Select "Standard create"
   - For Engine type, select "Amazon Aurora"
   - For Edition, select "Amazon Aurora MySQL-Compatible Edition"
   - For Version, select the latest version
   - Under Templates, select "Dev/Test" (to save costs)
   - Under Settings:
     - Enter a DB cluster identifier (e.g., "aurora-cluster-demo")
     - Set Master username and password
   - For DB instance class, select "Burstable classes" and then "db.t3.small"
   - For Availability & durability, select "Create an Aurora Replica/Reader node in a different AZ"
   - Under Connectivity:
     - Select your VPC
     - Select the DB subnet group you created
     - For Public access, select "No"
     - For VPC security group, select "Choose existing" and pick the security group you created
   - Under Additional configuration:
     - Enter an initial database name (e.g., "auroradb")
     - Uncheck "Enable automated backups" for this exercise
     - Keep other settings as default
   - Click "Create database"

5. **Create an EC2 Instance to Connect to Aurora**
   - Follow the steps from Exercise 1 to create an EC2 instance in the same VPC

6. **Connect to Your EC2 Instance**
   - Use SSH to connect to your instance:
     ```
     ssh -i /path/to/your-key.pem ec2-user@your-ec2-public-ip
     ```

7. **Install MySQL Client on the EC2 Instance**
   - Follow the steps from Exercise 1 to install the MySQL client

8. **Get Aurora Cluster Endpoint Information**
   - In the RDS console, click "Databases"
   - Click on your Aurora cluster name
   - Note both endpoints:
     - Writer endpoint (cluster endpoint) - for write operations
     - Reader endpoint - for read operations

9. **Connect to the Writer Endpoint**
   - From your EC2 instance, connect to the Aurora writer endpoint:
     ```
     mysql -h your-writer-endpoint -P 3306 -u admin -p
     ```
   - Enter your password when prompted
   - If successful, you'll see the MySQL prompt

10. **Create a Test Database and Table**
    - At the MySQL prompt, run:
      ```sql
      USE auroradb;
      
      CREATE TABLE products (
          id INT AUTO_INCREMENT PRIMARY KEY,
          name VARCHAR(100),
          price DECIMAL(10, 2),
          description TEXT,
          created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
      );
      
      INSERT INTO products (name, price, description)
      VALUES 
          ('Laptop', 999.99, 'High-performance laptop with 16GB RAM'),
          ('Smartphone', 699.99, 'Latest model with improved camera'),
          ('Tablet', 349.99, 'Lightweight tablet with long battery life'),
          ('Headphones', 159.99, 'Noise-cancelling wireless headphones'),
          ('Monitor', 249.99, '27-inch 4K monitor with HDR support');
      
      SELECT * FROM products;
      ```
    - Exit the MySQL client:
      ```
      EXIT;
      ```

11. **Connect to the Reader Endpoint**
    - Connect to the Aurora reader endpoint:
      ```
      mysql -h your-reader-endpoint -P 3306 -u admin -p
      ```
    - Run a query to verify you can read data:
      ```sql
      USE auroradb;
      SELECT * FROM products;
      ```
    - Try to insert data (this should fail as the reader endpoint is read-only):
      ```sql
      INSERT INTO products (name, price, description)
      VALUES ('Keyboard', 79.99, 'Mechanical gaming keyboard');
      ```
    - Exit the MySQL client:
      ```
      EXIT;
      ```

12. **Test Aurora Failover (Optional)**
    - In the RDS console, select your Aurora cluster
    - Click on the writer instance (it will have the "Writer" role)
    - Click "Actions" → "Failover"
    - Click "Failover" in the confirmation dialog
    - Wait for the failover to complete (this may take a few minutes)
    - Refresh the page and observe the instance roles have changed

13. **Connect to Aurora After Failover**
    - Connect to the writer endpoint again (note that the endpoint remains the same even after failover):
      ```
      mysql -h your-writer-endpoint -P 3306 -u admin -p
      ```
    - Verify your data is still available:
      ```sql
      USE auroradb;
      SELECT * FROM products;
      ```
    - Add a new record to confirm write functionality:
      ```sql
      INSERT INTO products (name, price, description)
      VALUES ('Speaker', 129.99, 'Bluetooth speaker with 20-hour battery life');
      
      SELECT * FROM products;
      ```
    - Exit the MySQL client:
      ```
      EXIT;
      ```

14. **Clean Up (Optional)**
    - In the RDS console, select your Aurora cluster
    - Click "Actions" → "Delete"
    - Uncheck "Create final snapshot"
    - Type "delete me" in the confirmation field
    - Click "Delete"
    - Delete the subnet group and security group if no longer needed
    - Terminate your EC2 instance if no longer needed

## Exercise 5: Work with Amazon DocumentDB

### Objective
Create an Amazon DocumentDB cluster, connect to it, and perform basic document database operations.

### Prerequisites
- VPC with subnets in at least two Availability Zones
- Knowledge of MongoDB commands (DocumentDB is MongoDB-compatible)

### Steps

1. **Navigate to DocumentDB Service**
   - In the AWS Management Console, type "DocumentDB" in the search bar
   - Select "DocumentDB" from the dropdown menu

2. **Create a DB Subnet Group (if you don't already have one)**
   - In the left navigation pane, click "Subnet groups"
   - Click "Create"
   - Enter a name (e.g., "docdb-subnet-group")
   - Enter a description
   - Select your VPC
   - Select at least two subnets in different Availability Zones
   - Click "Create"

3. **Create a Security Group for DocumentDB**
   - Go to the EC2 service
   - Click "Security Groups" in the left navigation pane
   - Click "Create security group"
   - Enter a name (e.g., "docdb-sg")
   - Add a description
   - Select your VPC
   - Add an inbound rule:
     - Type: Custom TCP
     - Port: 27017 (DocumentDB/MongoDB default port)
     - Source: The security group of your EC2 instance (or CIDR range)
   - Click "Create security group"

4. **Create a DocumentDB Cluster**
   - Return to the DocumentDB console
   - Click "Clusters" in the left navigation pane
   - Click "Create"
   - Enter a cluster identifier (e.g., "docdb-demo")
   - Set master username and password
   - For Instance class, select "db.t3.medium" (smallest available class)
   - For Number of instances, select 1 for testing (typically you would use at least 2 for production)
   - Under Advanced settings:
     - For VPC, select your VPC
     - For Subnet group, select the subnet group you created
     - For VPC security group, select the security group you created
     - Keep other settings as default
   - Click "Create cluster"

5. **Launch an EC2 Instance to Connect to DocumentDB**
   - Go to the EC2 service
   - Click "Launch instance"
   - Select Amazon Linux 2 AMI
   - Choose t2.micro instance type
   - Select a key pair
   - Configure networking:
     - Select the same VPC as your DocumentDB cluster
     - Enable auto-assign public IP
   - Configure security group to allow SSH (port 22) from your IP
   - Click "Launch instance"

6. **Get DocumentDB Endpoint Information**
   - Once your DocumentDB cluster is available, click on its name
   - Note the "Endpoint" on the Connectivity & security tab

7. **Connect to Your EC2 Instance**
   - Use SSH to connect to your instance:
     ```
     ssh -i /path/to/your-key.pem ec2-user@your-ec2-public-ip
     ```

8. **Download the Amazon DocumentDB Certificate**
   - The certificate is required to connect to your DocumentDB cluster:
     ```
     mkdir -p ~/.ssh
     wget https://s3.amazonaws.com/rds-downloads/rds-combined-ca-bundle.pem -O ~/.ssh/rds-combined-ca-bundle.pem
     ```

9. **Install MongoDB Shell**
   - Create a repository file for MongoDB:
     ```
     echo -e "[mongodb-org-4.0] \nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/amazon/2/mongodb-org/4.0/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc" | sudo tee /etc/yum.repos.d/mongodb-org-4.0.repo
     ```
   - Install MongoDB shell:
     ```
     sudo yum install -y mongodb-org-shell
     ```

10. **Connect to Your DocumentDB Cluster**
    - Connect using the MongoDB shell:
      ```
      mongo --ssl --host your-docdb-endpoint:27017 --sslCAFile ~/.ssh/rds-combined-ca-bundle.pem --username admin --password
      ```
    - Enter your password when prompted
    - If successful, you'll see the MongoDB shell prompt

11. **Perform Basic DocumentDB Operations**
    - At the MongoDB shell prompt, try the following commands:
    
      **Show existing databases:**
      ```
      show dbs
      ```
      
      **Create and use a new database:**
      ```
      use inventory
      ```
      
      **Insert documents into a collection:**
      ```
      db.products.insertMany([
        {
          name: "Notebook Computer",
          brand: "XYZ",
          price: 1299.99,
          specs: {
            cpu: "Intel Core i7",
            memory: "16GB",
            storage: "512GB SSD"
          },
          inStock: true,
          categories: ["electronics", "computers"]
        },
        {
          name: "Wireless Earbuds",
          brand: "AudioTech",
          price: 89.99,
          specs: {
            batteryLife: "8 hours",
            chargingCase: "24 hours",
            waterproof: true
          },
          inStock: true,
          categories: ["electronics", "audio"]
        },
        {
          name: "Smart Watch",
          brand: "FitLife",
          price: 199.99,
          specs: {
            display: "AMOLED",
            sensors: ["heart rate", "GPS", "accelerometer"],
            batteryLife: "5 days"
          },
          inStock: false,
          categories: ["electronics", "wearables"]
        }
      ])
      ```
      
      **Find all documents in a collection:**
      ```
      db.products.find().pretty()
      ```
      
      **Find documents with a specific field value:**
      ```
      db.products.find({ brand: "AudioTech" }).pretty()
      ```
      
      **Find documents with price greater than a value:**
      ```
      db.products.find({ price: { $gt: 100 } }).pretty()
      ```
      
      **Find documents with a nested field value:**
      ```
      db.products.find({ "specs.waterproof": true }).pretty()
      ```
      
      **Find in-stock products:**
      ```
      db.products.find({ inStock: true }).pretty()
      ```
      
      **Find products in a specific category:**
      ```
      db.products.find({ categories: "audio" }).pretty()
      ```
      
      **Update a document:**
      ```
      db.products.updateOne(
        { name: "Smart Watch" },
        { $set: { inStock: true, price: 179.99 } }
      )
      ```
      
      **Find updated document:**
      ```
      db.products.find({ name: "Smart Watch" }).pretty()
      ```
      
      **Delete a document:**
      ```
      db.products.deleteOne({ name: "Wireless Earbuds" })
      ```
      
      **Verify deletion:**
      ```
      db.products.find().pretty()
      ```
      
      **Create an index:**
      ```
      db.products.createIndex({ price: 1 })
      ```
      
      **Show all indexes in a collection:**
      ```
      db.products.getIndexes()
      ```
    
    - Exit the MongoDB shell when finished:
      ```
      exit
      ```

12. **Clean Up (Optional)**
    - In the DocumentDB console, select your cluster
    - Click "Actions" → "Delete"
    - Type "delete me" in the confirmation field
    - Click "Delete"
    - Delete the subnet group and security group if no longer needed
    - Terminate your EC2 instance if no longer needed

## Conclusion

In this hands-on lab, you've learned how to:
- Create and configure an Amazon RDS MySQL database
- Create and query an Amazon DynamoDB table
- Set up an Amazon ElastiCache Redis cluster
- Create an Amazon Aurora database cluster with read replicas
- Work with Amazon DocumentDB for MongoDB-compatible document database operations

These database service skills are essential for designing scalable, high-performance data storage solutions in AWS. Each database service has unique characteristics that make it suitable for different types of applications and workloads. 