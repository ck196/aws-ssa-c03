# Module 6: Monitoring and Management

## Amazon CloudWatch

Monitoring and observability service for AWS resources and applications:

### CloudWatch Components

1. **Metrics**
   - Data points about resource performance
   - Standard metrics provided by AWS services
   - Custom metrics published by applications
   - Basic monitoring (5-minute intervals)
   - Detailed monitoring (1-minute intervals)

2. **Logs**
   - Centralized log collection and analysis
   - Log groups organize log streams
   - Log retention configurable (1 day to never expire)
   - Log Insights for querying logs
   - Subscriptions to process logs in real-time

3. **Alarms**
   - Watch metrics and trigger actions
   - Based on static thresholds or anomaly detection
   - Actions: SNS notifications, Auto Scaling, EC2 actions
   - States: OK, ALARM, INSUFFICIENT_DATA

4. **Events (EventBridge)**
   - Near real-time stream of system events
   - Rule-based event routing to targets
   - Schedule-based events (cron expressions)
   - Event buses for custom applications

5. **Dashboards**
   - Customizable home pages for metrics
   - Cross-Region, cross-account visibility
   - Shareable and automatically refreshing

### Key Features

1. **Anomaly Detection**
   - Machine learning to detect unusual behavior
   - Adaptive thresholds based on historical patterns
   - Reduces false alarms

2. **Composite Alarms**
   - Combine multiple alarms with AND/OR logic
   - Reduce alarm noise
   - Create hierarchical alarm structures

3. **Container Insights**
   - Collect, aggregate, and summarize metrics from containerized applications
   - ECS, EKS, Kubernetes on EC2, Fargate

4. **Contributor Analysis**
   - Identify top contributors to metric values
   - Useful for finding resource-specific issues

5. **Synthetic Monitoring (CloudWatch Synthetics)**
   - Create canaries (configurable scripts)
   - Test endpoints and APIs
   - Monitor website availability

## AWS CloudTrail

Service for governance, compliance, and operational auditing:

### CloudTrail Concepts

1. **Trails**
   - Configuration for event delivery
   - Apply to all regions or a single region
   - Multiple trails per account

2. **Event Types**
   - Management events: Operations on AWS resources
   - Data events: Resource operations (S3 object-level, Lambda invocations)
   - Insights events: Unusual API call patterns
   - Network activity events: Network traffic to/from VPC endpoints

3. **Event Delivery**
   - S3 bucket for storage
   - CloudWatch Logs for monitoring
   - EventBridge for triggering actions

### CloudTrail Features

1. **Log File Integrity Validation**
   - Digital signatures verify log file integrity
   - Detect modifications, deletions, or forgeries

2. **Organization Trails**
   - Single trail for all accounts in AWS Organizations
   - Centralized audit and compliance

3. **CloudTrail Lake**
   - Event data store for immutable event storage
   - SQL-based queries for event analysis
   - Up to 7 years retention

4. **CloudTrail Insights**
   - Detect unusual API activity
   - Analyze management API call volume
   - Identify security and operational issues

### Security Best Practices

1. Use server-side encryption for log files
2. Use IAM policies to restrict access to CloudTrail
3. Enable log file validation
4. Configure CloudWatch alarms for specific API activities
5. Regularly review CloudTrail logs

## AWS Config

Service for assessing, auditing, and evaluating resource configurations:

### Config Components

1. **Configuration Items**
   - Point-in-time view of resource attributes
   - Relationships with other resources
   - Configuration history

2. **Configuration Snapshots**
   - Collection of configuration items at a point in time
   - Comprehensive view of resources

3. **Configuration Streams**
   - Near real-time stream of configuration changes
   - Delivered to SNS

4. **Configuration History**
   - Timeline of configuration changes
   - Track compliance over time

5. **Rules**
   - Evaluates resource configurations for compliance
   - AWS managed rules
   - Custom rules using Lambda

6. **Conformance Packs**
   - Collection of Config rules and remediation actions
   - Pre-built packs for common frameworks

### Use Cases

1. **Security Analysis**
   - Identify non-compliant resources
   - Security posture assessment
   - Vulnerability tracking

2. **Audit and Compliance**
   - Configuration drift detection
   - Compliance with internal policies
   - Regulatory compliance

3. **Change Management**
   - Track resource modifications
   - Impact analysis
   - Troubleshooting

4. **Resource Discovery**
   - Inventory of AWS resources
   - Resource relationships mapping

## AWS Systems Manager

Management service to operate and manage AWS infrastructure:

### Systems Manager Components

1. **Fleet Manager**
   - Unified interface for managing servers
   - File system, log, users management
   - No SSH/RDP required

2. **Session Manager**
   - Browser-based shell or CLI
   - No open inbound ports required
   - Audit session history and commands

3. **Patch Manager**
   - Automate patching process
   - Define patch baselines
   - Schedule patching with maintenance windows

4. **State Manager**
   - Define and maintain consistent configuration
   - Apply configurations on schedule or events
   - Report compliance status

5. **Inventory**
   - Collect metadata from managed instances
   - Application inventory
   - Custom inventory data

6. **Automation**
   - Simplify common maintenance tasks
   - Pre-defined or custom runbooks
   - Cross-account and cross-region automation

7. **Parameter Store**
   - Secure, hierarchical storage for configuration data
   - Plaintext and encrypted values
   - Integration with other AWS services

8. **OpsCenter**
   - Central location to view and resolve OpsItems
   - Aggregate issues across services
   - Track investigations and resolutions

9. **Application Manager**
   - Group resources by application
   - Monitor applications
   - Remediate issues

10. **Compliance**
    - Scan managed instances
    - Compare against patch, association, and configuration
    - Track compliance history

### Benefits

1. **Improve visibility and control**
2. **Enhance security posture**
3. **Automate operational tasks**
4. **Reduce operational overhead**

## AWS Trusted Advisor

Online tool providing real-time guidance to help optimize AWS resources:

### Advisor Categories

1. **Cost Optimization**
   - Idle resources
   - Reserved instance opportunities
   - Underutilized resources

2. **Performance**
   - High utilization instances
   - CloudFront optimizations
   - Service limits

3. **Security**
   - Security group configurations
   - IAM use
   - MFA on root account
   - Public access to sensitive resources

4. **Fault Tolerance**
   - Auto Scaling usage
   - Redundancy across AZs
   - RDS backups

5. **Service Limits**
   - Resources approaching service limits
   - Service limit increase requests

### Feature Availability

1. **Basic Checks**
   - Available to all AWS customers
   - Limited number of checks

2. **Full Trusted Advisor**
   - Available with Business and Enterprise Support plans
   - All checks and recommendations
   - Weekly email notifications
   - Programmatic access via API

## Knowledge Check

1. How does CloudWatch Logs Insights help with troubleshooting?
2. What's the difference between CloudTrail management events and data events?
3. How can you use AWS Config to maintain security compliance in your AWS environment?
4. What Systems Manager feature would you use to securely connect to EC2 instances without opening inbound ports?
5. How does AWS Trusted Advisor help optimize costs in your AWS environment?
6. Which CloudWatch feature allows you to proactively test website endpoints and APIs?
7. How can you use Systems Manager Parameter Store for security best practices?

## Hands-on Exercise

1. Create a CloudWatch dashboard with key metrics for an EC2 instance
2. Set up a CloudWatch alarm to notify when CPU utilization exceeds 80%
3. Configure a CloudTrail trail to capture all management events in your account
4. Create a basic AWS Config rule to check if your S3 buckets are encrypted
5. Use Systems Manager to run a command on multiple EC2 instances
6. Review Trusted Advisor recommendations for your AWS account

## Next Steps

In the next module, we'll cover AWS Application Integration services, focusing on SQS, SNS, EventBridge, and Step Functions. 