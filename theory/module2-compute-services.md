# Module 2: Compute Services

## Amazon EC2 (Elastic Compute Cloud)

Amazon EC2 provides resizable compute capacity in the cloud:

### EC2 Instance Types

1. **General Purpose (T3, M5, M6)**
   - Balanced compute, memory, and networking
   - Use cases: Web servers, development environments, small databases

2. **Compute Optimized (C5, C6)**
   - High performance processors
   - Use cases: Batch processing, scientific modeling, gaming servers

3. **Memory Optimized (R5, R6, X1)**
   - Fast performance for workloads processing large datasets in memory
   - Use cases: In-memory databases, real-time big data analytics

4. **Storage Optimized (I3, D2)**
   - High sequential read/write access to large datasets
   - Use cases: Data warehousing, log processing

5. **Accelerated Computing (P3, G4, F1)**
   - Hardware accelerators or co-processors
   - Use cases: Machine learning, graphics processing

### EC2 Purchasing Options

1. **On-Demand Instances**
   - Pay by the hour with no long-term commitments
   - Best for short-term, spiky, or unpredictable workloads

2. **Reserved Instances**
   - 1 or 3-year term commitment for significant discount (up to 72%)
   - Standard, Convertible, and Scheduled RI types
   - Best for steady-state workloads

3. **Spot Instances**
   - Request unused EC2 capacity at up to 90% discount
   - Can be terminated with 2 minutes notice
   - Best for fault-tolerant, flexible workloads

4. **Dedicated Hosts**
   - Physical server dedicated for your use
   - Helps address compliance requirements
   - Can use existing server-bound software licenses

5. **Savings Plans**
   - Commitment to a consistent amount of usage ($/hour) for 1 or 3 years
   - More flexible than RIs, applies to multiple instance types

### EC2 Storage Options

1. **Amazon EBS (Elastic Block Store)**
   - Persistent block storage volumes
   - Types: General Purpose (SSD), Provisioned IOPS (SSD), Throughput Optimized (HDD), Cold HDD
   - Features: Snapshots, encryption, elasticity

2. **Instance Store**
   - Temporary block-level storage
   - Physically attached to the host computer
   - Data lost when instance stops or terminates
   - High I/O performance

### EC2 Networking

1. **Security Groups**
   - Virtual firewall controlling inbound and outbound traffic
   - Stateful (return traffic automatically allowed)
   - Can reference other security groups

2. **Elastic IP**
   - Static IPv4 address designed for dynamic cloud computing
   - Associated with your AWS account
   - Can be remapped to different instances

3. **Elastic Network Interface (ENI)**
   - Logical networking component that represents a virtual network card
   - Attributes include: IP address, MAC address, security groups

## AWS Lambda

Serverless compute service that runs code in response to events:

### Key Concepts

1. **Functions**
   - Core Lambda resource
   - Contains code, configuration
   - Executed when invoked

2. **Triggers**
   - Services/resources that invoke Lambda
   - Examples: API Gateway, S3, DynamoDB, CloudWatch Events

3. **Event Source Mappings**
   - Resource that reads from stream/queue and invokes Lambda
   - Examples: Kinesis, SQS, DynamoDB Streams

### Lambda Features

1. **Runtime Support**
   - Node.js, Python, Java, Go, .NET, Ruby
   - Custom runtime with Lambda layers

2. **Execution Model**
   - Cold start vs warm execution
   - Concurrent executions
   - Memory allocation (128MB-10GB)
   - Timeout (up to 15 minutes)

3. **Deployment Options**
   - ZIP archive
   - Container images
   - Lambda layers

### Lambda Benefits

1. **No server management**
2. **Continuous scaling**
3. **Sub-second metering**
4. **Integrates with many AWS services**

## AWS Elastic Beanstalk

Platform as a Service (PaaS) for deploying and managing applications:

### Supported Platforms

1. **Languages and Frameworks**
   - Java, .NET, PHP, Node.js, Python, Ruby, Go
   - Docker containers

2. **Web Servers**
   - Apache, Nginx, IIS

### Key Components

1. **Application**
   - Collection of Elastic Beanstalk components
   - Versions, environments, configurations

2. **Application Version**
   - Specific, labeled application code deployment

3. **Environment**
   - Collection of AWS resources running an application version
   - Tiers: Web Server or Worker

4. **Environment Configuration**
   - Parameters and settings that define environment behavior

### Deployment Options

1. **All at once**
   - Deploys to all instances simultaneously
   - Fastest but causes downtime

2. **Rolling**
   - Deploys in batches
   - Reduced capacity during deployment

3. **Rolling with additional batch**
   - Launches new instances before taking any offline
   - Maintains full capacity

4. **Immutable**
   - Creates a new Auto Scaling group with new instances
   - Safest for production environments

## Auto Scaling

Automatically adjusts capacity to maintain steady, predictable performance:

### Auto Scaling Components

1. **Launch Template/Configuration**
   - Defines the EC2 instance configuration
   - AMI, instance type, security groups, etc.

2. **Auto Scaling Group**
   - Collection of EC2 instances
   - Defines min, max, and desired capacity

3. **Scaling Policies**
   - Rules that determine when to scale
   - Target tracking, step scaling, simple scaling

### Scaling Types

1. **Dynamic Scaling**
   - Responds to changing demand
   - Based on CloudWatch metrics

2. **Predictive Scaling**
   - Schedules scaling actions based on predicted demand
   - Uses machine learning to analyze historical trends

3. **Scheduled Scaling**
   - Set scaling actions at specific times
   - Useful for predictable load changes

### Auto Scaling Best Practices

1. Use target tracking scaling policies when possible
2. Use multiple scaling policies for different metrics
3. Scale based on custom application metrics when relevant
4. Test scaling policies before production implementation

## Amazon ECS (Elastic Container Service)

Fully managed container orchestration service:

### Key Concepts

1. **Cluster**
   - Logical grouping of tasks or services
   - Infrastructure on which containers run

2. **Task Definition**
   - Blueprint for your application
   - Specifies containers, resources, ports, volumes

3. **Task**
   - Instance of a task definition
   - Set of containers running with defined resources

4. **Service**
   - Maintains specified number of tasks
   - Integrates with load balancers

5. **Container Agent**
   - Runs on each container instance
   - Communicates with ECS service

### Launch Types

1. **EC2 Launch Type**
   - You manage EC2 instances
   - More control over infrastructure
   - Better for large workloads with steady state

2. **Fargate Launch Type**
   - Serverless compute for containers
   - No EC2 instances to manage
   - Pay-per-task

### ECS Use Cases

1. Microservices architecture
2. Batch processing
3. Machine learning
4. Web applications
5. Continuous integration/deployment

## Knowledge Check

1. What EC2 instance type would you recommend for a memory-intensive database application?
2. How do Spot Instances differ from On-Demand Instances?
3. What is the maximum execution timeout for a Lambda function?
4. Which Elastic Beanstalk deployment method ensures zero downtime and no capacity reduction?
5. In Amazon ECS, what is the difference between a task and a service?
6. Which Auto Scaling component defines the EC2 instance configuration?
7. What's the primary benefit of using the Fargate launch type with ECS?

## Hands-on Exercise

1. Launch an EC2 instance using the AWS Management Console
2. Create a simple Lambda function that returns a "Hello World" message
3. Configure Auto Scaling for your EC2 instance
4. Deploy a sample application using Elastic Beanstalk
5. Create an ECS cluster and run a test container

## Next Steps

In the next module, we'll cover AWS Storage Services, including S3, EBS, EFS, and Storage Gateway. 