# Module 7: Application Integration

## Amazon SQS (Simple Queue Service)

Fully managed message queuing service for decoupling and scaling microservices:

### Key Concepts

1. **Queue Types**
   - Standard Queues:
     - Nearly unlimited throughput
     - At-least-once delivery
     - Best-effort ordering
   - FIFO Queues:
     - Exactly-once processing
     - First-in-first-out delivery
     - Limited throughput (3,000 messages/second with batching)

2. **Message Lifecycle**
   - Send message to queue (producer)
   - Message becomes available for processing
   - Consumer retrieves message and processes it
   - Message remains in queue but not available to other consumers (visibility timeout)
   - Consumer deletes message after processing
   - If not deleted before visibility timeout expires, message becomes available again

3. **Visibility Timeout**
   - Default: 30 seconds
   - Maximum: 12 hours
   - Can be extended on a per-message basis

4. **Message Retention**
   - Default: 4 days
   - Configurable: 1 minute to 14 days

### SQS Features

1. **Dead-Letter Queues (DLQ)**
   - Target for messages that can't be processed
   - Helps isolate and analyze problematic messages
   - Configure maximum receives before moving to DLQ

2. **Delay Queues**
   - Postpone delivery for a specific period
   - Default: 0 seconds
   - Maximum: 15 minutes

3. **Long Polling**
   - Reduces empty responses and latency
   - Waits for messages to arrive
   - Maximum: 20 seconds

4. **Message Attributes**
   - Metadata for messages
   - Up to 10 attributes per message
   - Used for message filtering

5. **Message Batching**
   - Send, receive, or delete multiple messages in a single API call
   - Reduces costs and increases throughput

### Common Use Cases

1. Workload decoupling
2. Spike handling and load leveling
3. Buffering and batch processing
4. Request offloading
5. Reliable messaging

## Amazon SNS (Simple Notification Service)

Fully managed pub/sub messaging service for microservices and serverless applications:

### Key Concepts

1. **Topics**
   - Communication channels for message publishing
   - Logical access point for publishers

2. **Publishers**
   - Entities that send messages to topics
   - AWS services, applications, or systems

3. **Subscribers**
   - Endpoints that receive messages from topics
   - Types: Lambda, SQS, HTTP/S, email, SMS, mobile push

4. **Subscriptions**
   - Link between topic and subscriber
   - Filtering options to control which messages are delivered

5. **Message Filtering**
   - JSON policy to selectively receive messages
   - Based on message attributes

### SNS Features

1. **Delivery Protocols**
   - HTTP/HTTPS for webhooks
   - Email/Email-JSON
   - SQS for queuing
   - Lambda for serverless processing
   - SMS for text messages
   - Mobile push for app notifications

2. **Message Attributes**
   - Metadata for messages
   - Used for message filtering and routing

3. **Message Archiving and Analytics**
   - Deliver messages to Firehose for archiving
   - Process with Kinesis Data Analytics

4. **FIFO Topics**
   - Strict order guarantee
   - Exactly-once delivery
   - Must subscribe SQS FIFO queues

5. **Message Durability**
   - Stored across multiple AZs
   - Automatic retries with backoff

### Common Use Cases

1. Application and service alerts
2. Push email and text messaging
3. Mobile push notifications
4. Fan-out to multiple subscribers
5. Application-to-application messaging

## Amazon EventBridge (formerly CloudWatch Events)

Serverless event bus service for connecting applications with data from various sources:

### Key Concepts

1. **Event Buses**
   - Default event bus for AWS services
   - Custom event buses for your applications
   - Partner event buses for SaaS integrations

2. **Events**
   - JSON objects representing changes in environment
   - Contains source, time, resources, and detail

3. **Rules**
   - Match incoming events
   - Route to targets based on patterns or schedules

4. **Targets**
   - Destinations for matched events
   - Over 20 AWS services can be targets

### EventBridge Features

1. **Event Pattern Matching**
   - Match on event structure
   - Content-based filtering
   - Transforms for target customization

2. **Scheduled Events**
   - CRON or rate expressions
   - Trigger target at regular interval
   - Create recurring processes

3. **Schema Registry**
   - Discover, create, and manage schemas
   - Code bindings for application integration
   - Versioning for schema evolution

4. **Event Archive and Replay**
   - Store events for future processing
   - Replay events for testing or recovery
   - Configurable retention period

5. **Dead-Letter Queues**
   - Capture failed event deliveries
   - Analyze delivery failures
   - Reprocess events

### Use Cases

1. Serverless application integration
2. Reacting to state changes
3. Scheduled tasks and automation
4. SaaS integration without webhooks
5. Cross-account event handling

## AWS Step Functions

Service to coordinate components of distributed applications and microservices using visual workflows:

### Key Concepts

1. **State Machines**
   - JSON definition of workflow (Amazon States Language)
   - Collection of states that can perform work
   - Maximum execution time: 1 year

2. **Workflow Types**
   - Standard Workflows:
     - Exactly-once execution
     - Up to 1 year duration
     - Higher-cost, longer-running
   - Express Workflows:
     - At-least-once execution
     - Up to 5 minutes duration
     - Lower-cost, high-volume

3. **States**
   - Task: Perform work (AWS service, HTTP endpoint)
   - Choice: Add branching logic
   - Parallel: Execute branches concurrently
   - Map: Iterate over an array or batch
   - Wait: Delay execution
   - Success/Fail: End execution
   - Pass: Pass data without work

4. **Execution**
   - Instance of a workflow
   - Input, output, and execution history
   - Can be started via API, EventBridge, or other AWS services

### Step Functions Features

1. **Error Handling**
   - Retry: Automatic retry with configurable backoff
   - Catch: Handle errors and define fallback states
   - States can timeout to prevent blocked executions

2. **Service Integrations**
   - Optimized integrations with AWS services
   - Direct integrations with minimal code
   - SDK integrations for more flexibility

3. **Execution History**
   - Complete audit trail of each step
   - Input and output for each state
   - Timeline of events

4. **Visual Monitoring**
   - Graphical console for workflow visualization
   - Real-time execution tracking
   - Drill down into state details

### Use Cases

1. Order processing workflows
2. Data processing pipelines
3. Web application backend orchestration
4. Media processing workflows
5. ML model training and inferencing

## Amazon AppFlow

Fully managed integration service for exchanging data between SaaS applications and AWS services:

### Key Features

1. **Connections**
   - Pre-built connectors for popular SaaS platforms
   - Custom connectors for specific applications
   - Authentication and credential management

2. **Flows**
   - Define source, destination, and transformation
   - Trigger on schedule, event, or on-demand
   - Filter and transform data during transfer

3. **Data Mapping and Transformation**
   - Field mapping between source and destination
   - Functions for data manipulation
   - Filtering to include/exclude records

4. **Security**
   - Private connectivity with PrivateLink
   - Data encryption in transit and at rest
   - Field-level encryption for sensitive data

### Supported Applications

1. **Source Applications**
   - Salesforce, Marketo, Zendesk, ServiceNow
   - Google Analytics, Slack
   - Amazon S3

2. **Destination Services**
   - Amazon S3
   - Amazon Redshift
   - Amazon EventBridge
   - Snowflake

### Use Cases

1. Data synchronization between applications
2. Business analytics and reporting
3. Data backup and archiving
4. Marketing and sales analytics
5. User data consolidation

## Knowledge Check

1. How do SQS Standard and FIFO queues differ in terms of message delivery guarantees?
2. What happens when a message in SQS isn't processed within the visibility timeout period?
3. In what scenarios would you choose SNS over SQS?
4. How can EventBridge help you build a loosely coupled architecture?
5. What Step Functions feature would you use to handle temporary failures in your workflow?
6. How would you implement a fan-out pattern using AWS messaging services?
7. What types of state machine executions does Step Functions support, and when would you use each?

## Hands-on Exercise

1. Create an SQS queue and test sending/receiving messages
2. Set up an SNS topic with email and SQS subscribers
3. Configure an EventBridge rule to capture EC2 state changes and send to SNS
4. Create a simple Step Functions workflow with Lambda integrations
5. Implement a fan-out pattern using SNS with multiple SQS queues

## Next Steps

In the next module, we'll cover AWS Security Services, including IAM, AWS Shield, WAF, Secrets Manager, and KMS. 