# Module 5: Networking

## Amazon VPC (Virtual Private Cloud)

Amazon VPC lets you provision a logically isolated section of the AWS cloud:

### VPC Fundamentals

1. **VPC Components**
   - Subnets: Range of IP addresses in your VPC
   - Route Tables: Rules determining where network traffic is directed
   - Internet Gateway: Allows communication between VPC and internet
   - NAT Gateway/Instance: Enables private subnet resources to access internet
   - Security Groups: Virtual firewall at the instance level
   - Network ACLs: Stateless firewall at the subnet level
   - VPC Endpoints: Private connection to supported AWS services

2. **IP Addressing**
   - IPv4 CIDR blocks (primary)
   - IPv6 CIDR blocks (optional)
   - Public IP addresses for internet accessibility
   - Private IP addresses for internal communication

3. **Subnet Types**
   - Public: Has route to the internet via Internet Gateway
   - Private: No direct route to the internet
   - Subnet spans single Availability Zone

### Connectivity Options

1. **VPC Peering**
   - Direct network connection between two VPCs
   - Traffic stays on AWS backbone
   - No transitive peering (must establish direct connections)

2. **AWS Transit Gateway**
   - Hub-and-spoke connection model for multiple VPCs and on-premises networks
   - Simplifies network architecture
   - Regional resource, can work cross-region

3. **VPN Connections**
   - Site-to-Site VPN: IPsec connection to on-premises networks
   - Client VPN: Secure connections for remote users

4. **AWS Direct Connect**
   - Dedicated private connection from on-premises to AWS
   - Reduces network costs, increases bandwidth
   - Consistent network performance

### VPC Security

1. **Security Groups**
   - Stateful firewall (return traffic automatically allowed)
   - Allow rules only (no explicit deny)
   - Applied at the instance level
   - Can reference other security groups

2. **Network ACLs**
   - Stateless firewall (return traffic must be explicitly allowed)
   - Allow and deny rules
   - Applied at the subnet level
   - Processed in order based on rule number

3. **Flow Logs**
   - Capture network traffic information
   - Can be created for VPCs, subnets, or network interfaces
   - Published to CloudWatch Logs or S3

4. **Traffic Mirroring**
   - Copy network traffic for inspection
   - Send to security appliances for content inspection and threat monitoring

### Advanced VPC Features

1. **VPC Endpoints**
   - Interface Endpoints: ENI with private IP, powered by AWS PrivateLink
   - Gateway Endpoints: Target for specific route in route table (S3, DynamoDB)
   - Eliminates need for internet access to reach AWS services

2. **Egress-Only Internet Gateway**
   - Allows outbound IPv6 traffic, prevents inbound connections
   - Similar to NAT Gateway but for IPv6

3. **VPC Flow Logs**
   - Capture IP traffic information
   - Troubleshoot connectivity issues
   - Security monitoring and analysis

## Amazon Route 53

Highly available and scalable Domain Name System (DNS) web service:

### DNS Concepts

1. **Domain Registration**
   - Purchase and manage domain names
   - Automatic renewal options
   - Integration with Route 53 hosting

2. **DNS Records**
   - A/AAAA: Maps domain to IPv4/IPv6 address
   - CNAME: Maps domain to another domain
   - MX: Specifies mail servers
   - TXT: Text records for verification
   - NS: Specifies authoritative name servers
   - SOA: Contains administrative information

3. **Hosted Zones**
   - Public: Resolves queries from public clients
   - Private: Resolves queries from within specified VPCs

### Routing Policies

1. **Simple Routing**
   - Basic routing to a single resource
   - No health checks
   - If multiple values, returns all randomly

2. **Weighted Routing**
   - Route traffic based on assigned weights
   - Useful for A/B testing or gradual migration

3. **Latency-based Routing**
   - Routes traffic to region with lowest latency
   - Based on actual traffic patterns

4. **Failover Routing**
   - Active-passive failover configuration
   - Uses health checks to determine resource availability

5. **Geolocation Routing**
   - Routes based on geographic location of users
   - Useful for regional content delivery

6. **Geoproximity Routing**
   - Routes based on geographic location of resources
   - Adjustable bias to expand/shrink geographic regions

7. **Multi-value Answer Routing**
   - Returns multiple healthy records
   - Client chooses which to use

### Health Checking

1. **Endpoint Health Checks**
   - Monitor endpoints via HTTP, HTTPS, or TCP
   - Customize intervals, thresholds, and paths

2. **Calculated Health Checks**
   - Combine results of multiple health checks
   - Specify how many must pass for overall success

3. **CloudWatch Alarms**
   - Trigger health check failure based on CloudWatch metrics

## Amazon CloudFront

Global content delivery network (CDN) service:

### CloudFront Components

1. **Distributions**
   - Configuration for content delivery
   - Web distribution: HTTP/HTTPS content
   - RTMP distribution: Media streaming (legacy)

2. **Origins**
   - S3 bucket: For static content
   - Custom Origin: Any HTTP server (EC2, ALB, on-premises)

3. **Edge Locations**
   - Points of presence for content caching
   - 200+ globally distributed
   - Content served from closest edge location to user

4. **Regional Edge Caches**
   - Larger caches between origin and edge locations
   - Improves performance for less popular content

### Key Features

1. **Cache Control**
   - TTL settings determine cache duration
   - Versioned URLs for content updates
   - Cache invalidation to remove objects

2. **Origin Shield**
   - Additional caching layer
   - Reduces load on origin
   - Improves cache hit ratio

3. **Security Features**
   - HTTPS with custom SSL certificates
   - Field-level encryption for sensitive data
   - Origin Access Identity (OAI) to restrict S3 access
   - AWS WAF integration for filtering malicious traffic
   - AWS Shield integration for DDoS protection

4. **Functions and Lambda@Edge**
   - Run code closer to users
   - Customize content delivery
   - Four possible trigger points:
     - Viewer request
     - Viewer response
     - Origin request
     - Origin response

5. **Real-time Logs**
   - Detailed information about requests
   - Configure sampling rate
   - Send to Kinesis Data Streams

### Use Cases

1. **Static content delivery**
2. **Dynamic content acceleration**
3. **Video streaming**
4. **Security and DDoS protection**
5. **API acceleration**
6. **Software distribution**

## AWS Direct Connect

Dedicated network connection from on-premises to AWS:

### Direct Connect Components

1. **Connections**
   - Dedicated Connection: 1Gbps, 10Gbps, 100Gbps
   - Hosted Connection: Various speeds through AWS Partners

2. **Virtual Interfaces (VIFs)**
   - Public VIF: Connect to public AWS services
   - Private VIF: Connect to resources in VPC
   - Transit VIF: Connect to resources using Transit Gateway

3. **Direct Connect Gateways**
   - Connect to multiple VPCs across regions
   - Simplifies network architecture

4. **Link Aggregation Groups (LAGs)**
   - Combine multiple connections as a single managed connection
   - Increased bandwidth and redundancy

### Benefits

1. **Reduced network costs**
   - Predictable and potential cost savings
   - Data transfer rates lower than internet

2. **Increased bandwidth**
   - Consistent throughput
   - Up to 100Gbps per connection

3. **Consistent network experience**
   - Dedicated, private connections
   - Reduced latency
   - Less network jitter

4. **Hybrid architecture support**
   - Extend data centers to the cloud
   - Seamless integration with on-premises resources

### Resilience Recommendations

1. **Single Region, Multiple AZs**
   - Connections in separate facilities
   - Multiple VIFs to separate AZs

2. **Multiple Regions**
   - Connections to separate regions
   - Global resilience

3. **Maximum Resilience**
   - Multiple connections in multiple regions
   - Combination with Site-to-Site VPN as backup

## API Gateway

Fully managed service for creating, publishing, and securing APIs:

### API Gateway Features

1. **Types of APIs**
   - REST APIs: Stateless API with HTTP endpoints
   - HTTP APIs: Lighter-weight, lower latency REST APIs
   - WebSocket APIs: Stateful API for two-way communication

2. **Integration Types**
   - Lambda functions
   - HTTP endpoints
   - AWS services
   - Mock integrations for testing

3. **API Management**
   - Versioning for lifecycle management
   - Stages for development, testing, production
   - Canary deployments for testing new versions

4. **Security Features**
   - Authentication with Lambda authorizers or Cognito
   - API keys for client identification
   - Resource policies for access control
   - AWS WAF integration for filtering

5. **Performance Features**
   - Caching responses to reduce backend calls
   - Throttling to prevent overload
   - Regional or edge-optimized endpoints

### Common Use Cases

1. **Serverless microservices architecture**
2. **Mobile application backends**
3. **B2B integrations**
4. **Proxy for existing APIs**

## Knowledge Check

1. What is the difference between security groups and network ACLs in a VPC?
2. Which Route 53 routing policy would you use to implement an active-passive disaster recovery strategy?
3. How does CloudFront Origin Shield help improve performance?
4. What are the different types of VPC endpoints and when would you use each?
5. How would you secure an API deployed with API Gateway?
6. What is the purpose of a NAT Gateway in a VPC architecture?
7. What connectivity option would you recommend for a hybrid architecture requiring consistent, high-bandwidth connection to AWS?

## Hands-on Exercise

1. Create a VPC with public and private subnets
2. Set up routing with Internet Gateway and NAT Gateway
3. Configure security groups and NACLs to secure the environment
4. Create a CloudFront distribution with an S3 bucket as origin
5. Set up a basic API using API Gateway and Lambda
6. Configure Route 53 for domain management with health checks

## Next Steps

In the next module, we'll cover AWS Monitoring and Management services, including CloudWatch, CloudTrail, AWS Config, and Systems Manager. 