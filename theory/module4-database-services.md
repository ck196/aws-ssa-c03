# Module 4: Database Services

## Amazon RDS (Relational Database Service)

Managed relational database service supporting multiple database engines:

### Supported Database Engines

1. **MySQL**
2. **PostgreSQL**
3. **MariaDB**
4. **Oracle**
5. **Microsoft SQL Server**
6. **Amazon Aurora**

### Key Features

1. **Automated Backups**
   - Daily full backup during maintenance window
   - Transaction logs backed up every 5 minutes
   - Point-in-time recovery up to 35 days

2. **Manual Snapshots**
   - User-initiated backups
   - Retained until explicitly deleted
   - Can copy to different regions

3. **Multi-AZ Deployments**
   - Synchronous replication to standby in different AZ
   - Automatic failover without manual intervention
   - Enhanced durability and availability

4. **Read Replicas**
   - Asynchronous replication for read scaling
   - Up to 15 read replicas
   - Can be promoted to standalone instances
   - Can be in different regions

5. **Security Features**
   - Encryption at rest using KMS
   - Encryption in transit using SSL
   - Network isolation using VPC
   - IAM database authentication

6. **Monitoring and Maintenance**
   - CloudWatch metrics and alarms
   - Enhanced Monitoring for OS-level metrics
   - Performance Insights for database performance analysis
   - Automated patching during maintenance windows

### Common Use Cases

1. Web and mobile applications
2. Enterprise applications
3. E-commerce platforms
4. Content management systems

## Amazon Aurora

MySQL and PostgreSQL-compatible relational database built for the cloud:

### Key Features

1. **Performance**
   - 5x throughput of MySQL, 3x throughput of PostgreSQL
   - Distributed, fault-tolerant, self-healing storage system
   - 10GB to 128TB of storage in 10GB increments

2. **High Availability and Durability**
   - Data replicated across 6 storage nodes in 3 AZs
   - 99.99% availability
   - Self-healing storage with disk failures automatically repaired

3. **Aurora Replicas**
   - Up to 15 read replicas with minimal lag (typically < 10ms)
   - Automatic failover to replica in case of primary failure
   - Reader endpoint for load balancing connections across replicas

4. **Aurora Serverless**
   - On-demand, auto-scaling configuration
   - Automatically starts up, shuts down, and scales based on application needs
   - Pay only for resources consumed

5. **Aurora Global Database**
   - Spans multiple AWS regions
   - Fast replication with typical latency < 1 second
   - Support for disaster recovery and global reads

6. **Aurora Database Cloning**
   - Create new database from existing one quickly
   - Uses copy-on-write protocol for efficiency
   - Useful for testing and development

### Use Cases

1. SaaS applications
2. Gaming applications requiring high throughput
3. Applications requiring high availability
4. Applications with unpredictable workloads (Serverless)

## Amazon DynamoDB

Fully managed NoSQL database service for any scale:

### Key Concepts

1. **Tables, Items, and Attributes**
   - Tables: Collection of items (similar to tables in relational databases)
   - Items: Collection of attributes (similar to rows)
   - Attributes: Fundamental data elements (similar to columns)

2. **Primary Keys**
   - Simple Primary Key: Single partition key
   - Composite Primary Key: Partition key + sort key
   - Determines data distribution and access patterns

3. **Secondary Indexes**
   - Global Secondary Index (GSI): Different partition key and optional sort key
   - Local Secondary Index (LSI): Same partition key but different sort key
   - Enables efficient queries on non-key attributes

### DynamoDB Features

1. **Performance and Scaling**
   - Single-digit millisecond latency
   - Automatic scaling of throughput capacity
   - No practical limits on table size or throughput

2. **DynamoDB Accelerator (DAX)**
   - In-memory cache for DynamoDB
   - Microsecond latency for cached reads
   - No application changes required

3. **Global Tables**
   - Multi-region, multi-active replication
   - Automatic replication across selected regions
   - Enables globally distributed applications

4. **Time to Live (TTL)**
   - Automatically expire items at specified time
   - Reduces storage costs for temporary data

5. **Transactions**
   - All-or-nothing operations across multiple items/tables
   - ACID compliance for complex business logic

6. **On-Demand Capacity Mode**
   - Pay-per-request pricing
   - No capacity planning required
   - Auto-scales instantly

7. **Backup and Restore**
   - On-demand backups
   - Point-in-time recovery (up to 35 days)

### Use Cases

1. Mobile and web applications
2. Gaming applications
3. IoT data storage
4. Ad tech and real-time bidding systems
5. Session storage and metadata caching

## Amazon ElastiCache

In-memory caching service supporting Redis and Memcached:

### ElastiCache for Redis

1. **Key Features**
   - Rich data structures (strings, lists, sets, sorted sets, hashes)
   - Persistence for durability
   - Multi-AZ with automatic failover
   - Replication for read scaling
   - Cluster mode for partitioning data across nodes

2. **Advanced Redis Features**
   - Redis AUTH for authentication
   - Encryption in transit and at rest
   - Redis Streams for messaging
   - Backup and restore functionality

3. **Use Cases**
   - Caching and session stores
   - Gaming leaderboards and real-time analytics
   - Geospatial applications
   - Chat/messaging applications
   - Media streaming

### ElastiCache for Memcached

1. **Key Features**
   - Simple key-value store
   - Multi-threaded architecture
   - Automatic node discovery
   - No replication or persistence
   - Auto Discovery for adding and removing nodes

2. **Use Cases**
   - Object caching to reduce database load
   - Session caching for web applications
   - Content caching for faster page loads

### Caching Strategies

1. **Lazy Loading (Cache-Aside)**
   - Load data into cache only when necessary
   - Advantages: Only requested data is cached
   - Disadvantages: Cache miss penalty, potential stale data

2. **Write-Through**
   - Update cache when database is updated
   - Advantages: Data always fresh
   - Disadvantages: Write penalty, unused data cached

3. **Time-to-Live (TTL)**
   - Set expiration time for cached items
   - Balances data freshness with performance

## Amazon Redshift

Fully managed, petabyte-scale data warehouse service:

### Architecture

1. **Cluster Components**
   - Leader node: Query planning, result aggregation
   - Compute nodes: Query execution, data storage
   - Slices: Units of parallel processing

2. **Columnar Storage**
   - Column-based data storage
   - Better compression and query performance
   - Optimized for analytics workloads

3. **Massively Parallel Processing (MPP)**
   - Distributes and executes queries across all compute resources
   - Scales linearly with additional nodes

### Redshift Features

1. **Performance Optimization**
   - Zone maps to skip unnecessary blocks
   - Result caching for repetitive queries
   - Automatic workload management (WLM)
   - Concurrency scaling for consistent performance

2. **Redshift Spectrum**
   - Query data directly from S3 without loading
   - Scales to thousands of instances
   - Separates storage from compute

3. **Data Sharing**
   - Share data across Redshift clusters without copying
   - Producers share data, consumers access it

4. **Security**
   - VPC networking
   - Encryption at rest and in transit
   - Fine-grained access controls
   - Integration with AWS IAM

### Use Cases

1. Business intelligence
2. Data warehousing and big data analytics
3. Log analysis
4. Real-time analytics for large datasets

## Knowledge Check

1. What RDS feature provides high availability by maintaining a synchronized standby database in a different Availability Zone?
2. How does Aurora differ from standard RDS in terms of storage architecture?
3. When would you choose DynamoDB over a relational database like RDS?
4. What is the main difference between ElastiCache for Redis and ElastiCache for Memcached?
5. What makes Redshift optimized for analytical queries compared to transactional databases?
6. How does DynamoDB Global Tables help in building globally distributed applications?
7. In what scenario would you use Aurora Serverless over standard Aurora?

## Hands-on Exercise

1. Create an RDS MySQL instance and configure Multi-AZ deployment
2. Set up a DynamoDB table with a composite primary key and a global secondary index
3. Create an ElastiCache Redis cluster and test basic caching operations
4. Launch a small Redshift cluster and load sample data for analysis
5. Implement a backup and restore strategy for an RDS database

## Next Steps

In the next module, we'll cover AWS Networking services, focusing on VPC, Route 53, CloudFront, and Direct Connect. 