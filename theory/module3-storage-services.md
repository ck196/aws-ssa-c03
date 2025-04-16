# Module 3: Storage Services

## Amazon S3 (Simple Storage Service)

Object storage service offering industry-leading scalability, availability, security, and performance:

### S3 Storage Classes

1. **S3 Standard**
   - High durability (99.999999999%, 11 9's) and availability (99.99%)
   - Designed for frequently accessed data
   - Use cases: Websites, content distribution, data analytics

2. **S3 Intelligent-Tiering**
   - Automatically moves objects between access tiers based on usage patterns
   - Same durability and performance as Standard
   - Use cases: Data with unknown or changing access patterns

3. **S3 Standard-IA (Infrequent Access)**
   - Lower cost than Standard, but charges for retrieval
   - Same durability as Standard, lower availability (99.9%)
   - Use cases: Backups, disaster recovery files

4. **S3 One Zone-IA**
   - Stores data in a single AZ (99.5% availability)
   - 20% lower cost than Standard-IA
   - Use cases: Secondary backups, easily recreatable data

5. **S3 Glacier**
   - Very low-cost storage for archiving
   - Retrieval times from minutes to hours
   - Use cases: Long-term archiving with infrequent access

6. **S3 Glacier Deep Archive**
   - Lowest cost storage class
   - Retrieval time of 12 hours
   - Use cases: Long-term data archiving accessed once or twice per year

### S3 Security Features

1. **Access Control Mechanisms**
   - IAM policies
   - Bucket policies
   - Access Control Lists (ACLs)
   - Bucket Access Points

2. **Encryption Options**
   - Server-Side Encryption:
     - SSE-S3 (AWS managed keys)
     - SSE-KMS (AWS KMS managed keys)
     - SSE-C (customer-provided keys)
   - Client-Side Encryption

3. **S3 Block Public Access**
   - Account-level and bucket-level settings to prevent public access

4. **S3 Object Lock**
   - Prevents objects from being deleted or overwritten
   - Supports WORM (Write Once Read Many) model

### S3 Data Management Features

1. **Lifecycle Configuration**
   - Automates transition between storage classes
   - Can expire objects after specified time

2. **Versioning**
   - Maintains multiple variants of objects
   - Protects against accidental deletions and overwrites

3. **Cross-Region Replication (CRR)**
   - Automatic, asynchronous copying of objects across buckets in different regions
   - Use cases: Compliance, lower latency access, data protection

4. **S3 Transfer Acceleration**
   - Fast, secure file transfers over long distances
   - Uses CloudFront edge locations

5. **S3 Select**
   - Retrieve subset of data from an object using SQL expressions
   - Reduces data transfer and improves performance

## Amazon EBS (Elastic Block Store)

Block storage service designed for use with Amazon EC2 instances:

### EBS Volume Types

1. **General Purpose SSD (gp2/gp3)**
   - Balanced price and performance
   - Up to 16,000 IOPS (gp2) or 16,000 IOPS and 1,000 MB/s throughput (gp3)
   - Use cases: Boot volumes, development environments, small to medium databases

2. **Provisioned IOPS SSD (io1/io2)**
   - High performance for mission-critical workloads
   - Up to 64,000 IOPS per volume, 1,000 MB/s throughput
   - Use cases: I/O-intensive applications, large databases

3. **Throughput Optimized HDD (st1)**
   - Low-cost HDD volume for frequently accessed, throughput-intensive workloads
   - Up to 500 MB/s throughput
   - Use cases: Big data, data warehouses, log processing

4. **Cold HDD (sc1)**
   - Lowest cost HDD volume for less frequently accessed workloads
   - Up to 250 MB/s throughput
   - Use cases: Colder data requiring fewer scans per day

### EBS Features

1. **Snapshots**
   - Point-in-time copies of volumes
   - Incremental backups
   - Can be used to create new volumes or copy across regions

2. **Encryption**
   - Transparent encryption of data, snapshots, and volumes
   - Minimal impact on performance
   - Uses AWS KMS keys

3. **Elastic Volumes**
   - Dynamically increase capacity
   - Adjust performance (IOPS and throughput)
   - Change volume type

4. **Multi-Attach**
   - Attach a single EBS volume to multiple EC2 instances
   - Only available for io1/io2 volumes

## Amazon EFS (Elastic File System)

Scalable, elastic, cloud-native NFS file system:

### EFS Features

1. **Performance Modes**
   - General Purpose: Low latency, ideal for web servers, CMS
   - Max I/O: Higher latency but higher throughput for big data, media processing

2. **Throughput Modes**
   - Bursting: Throughput scales with file system size
   - Provisioned: Specify throughput independent of size

3. **Storage Classes**
   - Standard: For frequently accessed files
   - Infrequent Access (IA): Lower cost for files not accessed daily

4. **Lifecycle Management**
   - Automatically moves files between storage classes based on access patterns

### EFS Benefits

1. **Elastic capacity**
   - Automatically grows and shrinks as files are added and removed
   - Pay only for what you use

2. **Shared access**
   - Thousands of EC2 instances can access the file system concurrently
   - Ideal for shared workloads

3. **Regional service**
   - Available across multiple AZs in a region
   - Built-in high availability

4. **Security**
   - Control network access via VPC security groups
   - IAM authentication to control client access
   - Encrypt data at rest and in transit

## AWS Storage Gateway

Hybrid storage service connecting on-premises environments with cloud storage:

### Gateway Types

1. **File Gateway**
   - NFS and SMB interfaces to S3
   - Local caching for low-latency access
   - Use cases: File share backup, migrating file data to S3

2. **Volume Gateway**
   - iSCSI block storage volumes backed by S3
   - Two modes:
     - Cached volumes: Primary data in S3, frequently accessed data cached locally
     - Stored volumes: Primary data stored locally, asynchronously backed up to S3
   - Use cases: Backup and disaster recovery, cloud bursting

3. **Tape Gateway**
   - Virtual tape library interface
   - Stores virtual tapes in S3 and Glacier
   - Use cases: Replacing physical tape infrastructure with cloud storage

### Benefits

1. **Low-latency access to cloud data from on-premises applications**
2. **Cost-effective storage for on-premises data**
3. **Compatibility with existing backup applications**
4. **Secure data transfer and storage**

## AWS Snow Family

Physical devices for data migration and edge computing:

### Types of Snow Devices

1. **Snowcone**
   - Smallest device (8TB)
   - Ideal for space-constrained environments
   - Can run EC2 instances locally

2. **Snowball Edge**
   - Storage optimized (80TB) or compute optimized (42TB with more powerful compute)
   - Includes compute functionality with EC2 and Lambda
   - Ideal for data migration, data processing, and edge computing

3. **Snowmobile**
   - Exabyte-scale data migration in a shipping container
   - Up to 100PB per Snowmobile
   - Ideal for massive data center migrations

### Use Cases

1. **Data migration to AWS**
   - Overcome network limitations for large datasets
   - Secure, physical data transport

2. **Edge computing**
   - Collect, process, and analyze data in remote or disconnected locations
   - Run applications in environments with limited/no connectivity

3. **Content distribution**
   - Distribute large amounts of data to multiple locations
   - Process data locally while maintaining a consistent view

## Knowledge Check

1. Which S3 storage class would you recommend for data that is accessed approximately once per quarter and needs retrieval within a few hours?
2. How does S3 Cross-Region Replication differ from a backup strategy using S3 lifecycle policies?
3. What EBS volume type would be most appropriate for a high-performance SQL database requiring consistent IOPS?
4. What is the key difference between EBS and EFS storage?
5. Which Storage Gateway type would you recommend for replacing a company's legacy tape backup system?
6. What AWS service would you use to migrate 200TB of data from an on-premises data center with limited network bandwidth?
7. How can you automatically move S3 objects from Standard to Glacier storage after 90 days?

## Hands-on Exercise

1. Create an S3 bucket and configure versioning and lifecycle policies
2. Upload objects to different storage classes and test access patterns
3. Create an EBS volume, attach it to an EC2 instance, and create a snapshot
4. Set up an EFS file system and mount it on multiple EC2 instances
5. Configure a Storage Gateway in a test environment

## Next Steps

In the next module, we'll cover AWS Database Services, including RDS, DynamoDB, ElastiCache, and Redshift. 