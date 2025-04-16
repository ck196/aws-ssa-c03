# Module 9: Application Integration Services - Hands-on Lab Guide

This guide provides detailed instructions for completing the hands-on exercises in Module 9.

## Exercise 1: Setting Up Amazon SQS for Message Queuing

### Objective
Create and configure Amazon SQS queues to decouple application components.

### Steps

1. **Sign in to the AWS Management Console**
   - Go to https://console.aws.amazon.com/
   - Sign in with your AWS account credentials

2. **Navigate to Amazon SQS Service**
   - In the search bar at the top, type "SQS"
   - Select "Simple Queue Service" from the dropdown menu

3. **Create a Standard SQS Queue**
   - Click "Create queue"
   - For "Type", select "Standard"
   - Enter a name for your queue (e.g., "MyStandardQueue")
   - Under "Configuration", keep the default settings:
     - Visibility timeout: 30 seconds
     - Message retention period: 4 days
     - Maximum message size: 256 KB
     - Delivery delay: 0 seconds
   - Under "Access policy", keep "Basic" selected
   - Leave all other settings at their defaults
   - Click "Create queue"

4. **Create a FIFO SQS Queue**
   - Click "Create queue"
   - For "Type", select "FIFO"
   - Enter a name for your queue, ensuring it ends with ".fifo" (e.g., "MyFifoQueue.fifo")
   - Under "Configuration":
     - Check "Content-based deduplication" to enable automatic deduplication based on message content
   - Leave all other settings at their defaults
   - Click "Create queue"

5. **Send a Message to the Standard Queue**
   - Select your standard queue from the list
   - Click "Send and receive messages"
   - In the "Message body" field, enter a test message: "This is a test message for the standard queue"
   - Leave "Message attributes" empty for now
   - Click "Send message"
   - You should see a success message

6. **Send a Message to the FIFO Queue**
   - Select your FIFO queue from the list
   - Click "Send and receive messages"
   - In the "Message body" field, enter a test message: "This is a test message for the FIFO queue"
   - For "Message group ID", enter "group1"
   - If content-based deduplication is not enabled, provide a "Message deduplication ID"
   - Click "Send message"
   - You should see a success message

7. **Receive Messages from the Standard Queue**
   - With your standard queue selected, go to the "Receive messages" section
   - Click "Poll for messages"
   - You should see your message appear in the list
   - Select the message to view its content
   - Click "Delete" to remove the message from the queue after processing

8. **Receive Messages from the FIFO Queue**
   - Select your FIFO queue
   - Go to the "Receive messages" section
   - Click "Poll for messages"
   - You should see your message appear in the list
   - Verify that messages are received in the order they were sent (if you sent multiple)
   - Delete the message after viewing

9. **Configure Dead-Letter Queue (DLQ)**
   - Click "Create queue" to create a new standard queue to use as a DLQ
   - Name it "MyDeadLetterQueue"
   - Create the queue with default settings
   
   - Select your original standard queue
   - Click "Edit"
   - Scroll down to "Dead-letter queue" and click "Enabled"
   - From the dropdown, select the "MyDeadLetterQueue" you just created
   - Set "Maximum receives" to 3 (message will be moved to DLQ after 3 failed processing attempts)
   - Click "Save"

10. **Test the Dead-Letter Queue**
    - Send a message to your standard queue
    - Receive the message but do not delete it
    - Wait for the visibility timeout to expire (30 seconds by default)
    - Repeat receiving and not deleting 2 more times
    - After the third attempt, check your dead-letter queue
    - Poll for messages in the DLQ and verify your message has been moved there

11. **Clean Up**
    - Select each queue you created
    - Click "Delete"
    - Confirm the deletion by typing the queue name

## Exercise 2: Creating a Pub/Sub System with Amazon SNS

### Objective
Set up an Amazon SNS topic to broadcast messages to multiple subscribers.

### Steps

1. **Navigate to the Amazon SNS Service**
   - In the AWS Management Console, search for "SNS"
   - Select "Simple Notification Service" from the dropdown

2. **Create an SNS Topic**
   - Click "Topics" in the left navigation pane
   - Click "Create topic"
   - For "Type", select "Standard"
   - Enter a name for your topic (e.g., "MyNotificationTopic")
   - (Optional) Enter a display name (e.g., "Notifications")
   - Leave other settings at their defaults
   - Click "Create topic"

3. **Create Email Subscriptions**
   - With your newly created topic selected, click "Create subscription"
   - For "Protocol", select "Email"
   - For "Endpoint", enter your email address
   - Click "Create subscription"
   - Check your email inbox and confirm the subscription by clicking the link in the confirmation email
   
   - (Optional) Create a second subscription with a different email address

4. **Create an SQS Subscription**
   - First, ensure you have an SQS queue available (you can use the one from Exercise 1 or create a new one)
   - Navigate back to your SNS topic
   - Click "Create subscription"
   - For "Protocol", select "Amazon SQS"
   - For "Endpoint", select the ARN of your SQS queue from the dropdown
   - Leave other settings at their defaults
   - Click "Create subscription"

5. **Modify the SQS Queue Access Policy**
   - Navigate to the SQS service
   - Select your queue
   - Click "Edit"
   - Scroll down to "Access policy"
   - Click "Advanced"
   - Ensure the policy includes permissions for SNS to send messages to this queue
   - If not, add a policy like this (replace the ARNs with your actual ARNs):
     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Effect": "Allow",
           "Principal": {
             "Service": "sns.amazonaws.com"
           },
           "Action": "sqs:SendMessage",
           "Resource": "YOUR-SQS-QUEUE-ARN",
           "Condition": {
             "ArnEquals": {
               "aws:SourceArn": "YOUR-SNS-TOPIC-ARN"
             }
           }
         }
       ]
     }
     ```
   - Click "Save"

6. **Publish a Message to the SNS Topic**
   - Return to the SNS service
   - Select your topic
   - Click "Publish message"
   - Enter a subject (e.g., "Test Notification")
   - Enter a message body (e.g., "This is a test notification from Amazon SNS")
   - Scroll down and click "Publish message"

7. **Verify Message Delivery**
   - Check your email inbox for the notification message
   - Navigate to the SQS service
   - Select your queue that's subscribed to the SNS topic
   - Click "Send and receive messages"
   - Click "Poll for messages"
   - Verify that the message from SNS appears in the queue

8. **Create an SNS Filter Policy**
   - Select your SQS subscription in the SNS topic
   - Click "Edit"
   - Scroll down to "Filter policy - optional"
   - Click "Create filter policy"
   - Add a filter policy like this:
     ```json
     {
       "priority": ["high", "critical"]
     }
     ```
   - Click "Save changes"

9. **Test the Filter Policy**
   - Click "Publish message" on your SNS topic
   - Enter a subject and message body
   - Expand "Message attributes"
   - Add a message attribute:
     - Name: "priority"
     - Type: "String"
     - Value: "high"
   - Click "Publish message"
   - This message should be delivered to the SQS queue
   
   - Publish another message with a "priority" attribute of "low"
   - This message should not be delivered to the SQS queue due to the filter policy

10. **Clean Up**
    - Delete any subscriptions by selecting them and clicking "Delete"
    - Delete the SNS topic
    - Delete any SQS queues created for this exercise

## Exercise 3: Building Event-Driven Architecture with Amazon EventBridge

### Objective
Create an EventBridge rule to route events to targets based on event patterns.

### Prerequisites
- An S3 bucket (can be created during the exercise)
- An SQS queue (can be from previous exercises or newly created)

### Steps

1. **Navigate to the Amazon EventBridge Service**
   - In the AWS Management Console, search for "EventBridge"
   - Select "Amazon EventBridge" from the dropdown

2. **Create an Event Bus**
   - Click "Event buses" in the left navigation pane
   - Click "Create event bus"
   - Enter a name for your event bus (e.g., "MyCustomEventBus")
   - Leave other settings at their defaults
   - Click "Create"

3. **Create an S3 Bucket for Event Notification**
   - Navigate to the S3 service
   - Click "Create bucket"
   - Enter a globally unique bucket name (e.g., "eventbridge-demo-[your-initials]-[random-number]")
   - Leave other settings at their defaults
   - Click "Create bucket"

4. **Create an SQS Queue as a Target**
   - If you don't already have an SQS queue from previous exercises:
   - Navigate to the SQS service
   - Click "Create queue"
   - Enter a name (e.g., "EventBridgeTargetQueue")
   - Use default settings
   - Click "Create queue"

5. **Create an EventBridge Rule for S3 Events**
   - Return to EventBridge
   - Click "Rules" in the left navigation pane
   - Select the default event bus
   - Click "Create rule"
   - Enter a name (e.g., "S3ObjectCreatedRule")
   - (Optional) Add a description
   - For "Rule type", select "Rule with an event pattern"
   - Click "Next"
   
   - For "Event source", select "AWS events or EventBridge partner events"
   - Under "Event pattern":
     - Select "AWS services" from the dropdown
     - Select "Simple Storage Service (S3)" as the service
     - Select "All Events" or specifically "Object Created" event type
     - Under "Specific bucket(s) by name", enter the name of your bucket
   - Click "Next"
   
   - For "Target types", select "AWS service"
   - For "Select a target", choose "SQS queue"
   - For "Queue", select your SQS queue
   - Click "Next"
   
   - Configure any tags if desired
   - Click "Next"
   
   - Review the rule configuration
   - Click "Create rule"

6. **Test the EventBridge Rule**
   - Navigate to your S3 bucket
   - Click "Upload"
   - Select a small test file from your computer
   - Click "Upload"
   - Wait a few seconds for the event to be processed
   
   - Navigate to the SQS service
   - Select your target queue
   - Click "Send and receive messages"
   - Click "Poll for messages"
   - You should see a message containing the S3 event details

7. **Create a Custom Event Pattern Rule**
   - Return to EventBridge
   - Click "Rules"
   - Select the default event bus
   - Click "Create rule"
   - Enter a name (e.g., "CustomEventRule")
   - For "Rule type", select "Rule with an event pattern"
   - Click "Next"
   
   - For "Event source", select "AWS events or EventBridge partner events"
   - Under "Event pattern", select "Custom pattern (JSON editor)" and enter:
     ```json
     {
       "source": ["custom.myapp"],
       "detail-type": ["order.created"],
       "detail": {
         "amount": [{"numeric": [">", 100]}]
       }
     }
     ```
   - Click "Next"
   
   - For "Target types", select "AWS service"
   - For "Select a target", choose "SQS queue"
   - For "Queue", select your SQS queue
   - Click "Next"
   
   - Review and create the rule

8. **Test the Custom Event Pattern Rule with AWS CLI**
   - Open AWS CloudShell or your local terminal with AWS CLI configured
   - Run the following command (replace the event bus name if you're using a custom bus):
     ```
     aws events put-events --entries '[
       {
         "Source": "custom.myapp",
         "DetailType": "order.created",
         "Detail": "{\"orderId\": \"12345\", \"customer\": \"JohnDoe\", \"amount\": 150}",
         "EventBusName": "default"
       }
     ]'
     ```
   - Check your SQS queue for the message
   
   - Send another event with a smaller amount:
     ```
     aws events put-events --entries '[
       {
         "Source": "custom.myapp",
         "DetailType": "order.created",
         "Detail": "{\"orderId\": \"12346\", \"customer\": \"JaneDoe\", \"amount\": 75}",
         "EventBusName": "default"
       }
     ]'
     ```
   - This event should not appear in your queue due to the filter condition

9. **Create a Scheduled Rule**
   - Return to EventBridge
   - Click "Rules"
   - Select the default event bus
   - Click "Create rule"
   - Enter a name (e.g., "ScheduledRule")
   - For "Rule type", select "Schedule"
   - Click "Next"
   
   - For "Schedule pattern", select "Fixed rate"
   - Set the rate to 5 minutes
   - Click "Next"
   
   - For "Target types", select "AWS service"
   - For "Select a target", choose "SQS queue"
   - For "Queue", select your SQS queue
   - Click "Next"
   
   - Review and create the rule
   
   - Wait for 5 minutes and check your queue for the scheduled message

10. **Clean Up**
    - Delete all EventBridge rules created
    - Delete the custom event bus if created
    - Delete the S3 bucket
    - Delete the SQS queue if no longer needed

## Exercise 4: Orchestrating Workflows with AWS Step Functions

### Objective
Create a Step Functions state machine to coordinate a multi-step workflow.

### Steps

1. **Navigate to the AWS Step Functions Service**
   - In the AWS Management Console, search for "Step Functions"
   - Select "Step Functions" from the dropdown

2. **Create an IAM Role for Step Functions**
   - Navigate to the IAM service
   - Click "Roles" in the left navigation pane
   - Click "Create role"
   - For "Trusted entity type", select "AWS service"
   - For "Use case", select "Step Functions"
   - Click "Next"
   - Search for and select "AmazonSQSFullAccess"
   - Click "Next"
   - Enter a role name (e.g., "StepFunctionsExecutionRole")
   - Click "Create role"

3. **Create a Simple State Machine**
   - Return to Step Functions
   - Click "State machines" in the left navigation pane
   - Click "Create state machine"
   - For "Choose authoring method", select "Design your workflow visually"
   - For "Type", select "Standard"
   - Click "Next"
   
   - You're now in the Workflow Studio
   - From the "Flow" tab, drag a "Pass" state onto the canvas
   - Name it "HelloWorld"
   - In the "Output" section, click "Add payload"
   - Enter the following JSON:
     ```json
     {
       "message": "Hello, Step Functions!"
     }
     ```
   - Click "Next" to go to the "Configure state machine settings" page
   - Enter a name for your state machine (e.g., "HelloWorldStateMachine")
   - For "Permissions", select "Choose an existing role"
   - Select the IAM role you created earlier
   - Click "Create"

4. **Execute the Simple State Machine**
   - On your state machine's page, click "Start execution"
   - Leave the input JSON as the default `{}`
   - Click "Start execution"
   - Watch the execution progress and succeed
   - Examine the output in the "Execution output" tab

5. **Create a More Complex State Machine with Choices**
   - Click "State machines" in the left navigation
   - Click "Create state machine"
   - Select "Design your workflow visually" and "Standard" type
   - Click "Next"
   
   - From the "Flow" tab, drag a "Choice" state onto the canvas
   - Name it "CheckOrderAmount"
   
   - Create a "Rule #1":
     - For "Variable", enter "$.amount"
     - For "Operator", select "is greater than"
     - For "Value", enter "100"
   
   - From the "Flow" tab, drag a "Pass" state for the true condition
   - Connect it to the "Rule #1" branch
   - Name it "ProcessLargeOrder"
   - Add an output payload:
     ```json
     {
       "result": "Order is eligible for premium processing",
       "originalAmount": $.amount
     }
     ```
   
   - From the "Flow" tab, drag another "Pass" state for the default condition
   - Connect it to the "Default" branch
   - Name it "ProcessStandardOrder"
   - Add an output payload:
     ```json
     {
       "result": "Order will be processed via standard delivery",
       "originalAmount": $.amount
     }
     ```
   
   - Click "Next"
   - Enter a name (e.g., "OrderProcessingStateMachine")
   - Select your existing IAM role
   - Click "Create"

6. **Test the Complex State Machine**
   - Click "Start execution"
   - Enter the following input:
     ```json
     {
       "amount": 150,
       "orderId": "12345"
     }
     ```
   - Click "Start execution"
   - Observe which branch the execution takes
   
   - Start another execution with a smaller amount:
     ```json
     {
       "amount": 50,
       "orderId": "12346"
     }
     ```
   - Verify that it takes the other branch

7. **Create a State Machine with SQS Integration**
   - Make sure you have an SQS queue from the previous exercises
   - Create a new state machine
   - From the "Actions" tab, drag "Send message to SQS" onto the canvas
   - Configure it:
     - Queue URL: Enter your SQS queue URL
     - Message body: Use `$.message` or a fixed string like "Message from Step Functions"
   
   - Add other states as desired (like a Wait state before sending)
   - Complete the state machine configuration and create it
   
   - Execute the state machine with appropriate input
   - Check your SQS queue to confirm message delivery

8. **Create a Wait State and Parallel Execution**
   - Create a new state machine
   - Add a "Wait" state that pauses for 30 seconds
   
   - Add a "Parallel" state after the Wait
   - In the first branch, add a Pass state with some output
   - In the second branch, add another Pass state with different output
   
   - Follow the state with a final Pass state that combines outputs
   - Complete the configuration and create the state machine
   
   - Execute it and observe how parallel branches execute simultaneously

9. **View CloudWatch Logs and Metrics**
   - In the Step Functions console, select one of your state machines
   - Click on the "Monitoring" tab
   - Explore the execution metrics and logs
   - Click on an execution to view its event history

10. **Clean Up**
    - Delete all state machines created
    - Delete the IAM role if no longer needed
    - Delete any SQS queues created solely for this exercise

## Exercise 5: Creating API Endpoints with Amazon API Gateway

### Objective
Create a RESTful API using Amazon API Gateway and integrate it with other AWS services.

### Steps

1. **Navigate to API Gateway Service**
   - In the AWS Management Console, search for "API Gateway"
   - Select "API Gateway" from the dropdown

2. **Create a REST API**
   - Click "Create API"
   - Select "REST API" and "Build"
   - For "Choose the protocol", select "REST"
   - For "Create new API", select "New API"
   - Enter an API name (e.g., "MyFirstAPI")
   - Select "Regional" for "Endpoint Type"
   - Click "Create API"

3. **Create a Resource and Method**
   - In the Resources pane, click "Actions" and select "Create Resource"
   - Enter "items" for "Resource Name"
   - The "Resource Path" should automatically become "/items"
   - Click "Create Resource"
   
   - With the "/items" resource selected, click "Actions" and select "Create Method"
   - From the dropdown that appears, select "GET" and click the checkmark
   - For "Integration type", select "Mock"
   - Click "Create Method"

4. **Configure the Mock Integration**
   - In the Method Execution pane, click on "Integration Response"
   - Expand the 200 response
   - Click on "Mapping Templates"
   - For "Content-Type", enter "application/json"
   - In the template body, enter:
     ```json
     {
       "items": [
         {
           "id": "1",
           "name": "Item 1",
           "price": 10.99
         },
         {
           "id": "2",
           "name": "Item 2",
           "price": 24.99
         },
         {
           "id": "3",
           "name": "Item 3",
           "price": 5.99
         }
       ]
     }
     ```
   - Click "Save"

5. **Create a Resource Path Parameter**
   - With the "/items" resource selected, click "Actions" and select "Create Resource"
   - Check "Configure as proxy resource"
   - Enter "item" for "Resource Name"
   - Enter "{id}" for "Resource Path"
   - Click "Create Resource"
   
   - With the "/items/{id}" resource selected, click "Actions" and select "Create Method"
   - Select "GET" and click the checkmark
   - For "Integration type", select "Mock"
   - Click "Create Method"
   
   - Configure the integration response similar to before, but use this template:
     ```json
     {
       "id": "$input.params('id')",
       "name": "Item $input.params('id')",
       "price": 10.99,
       "description": "This is item $input.params('id')"
     }
     ```
   - Click "Save"

6. **Deploy the API**
   - Click "Actions" and select "Deploy API"
   - For "Deployment stage", select "New Stage"
   - Enter "v1" for "Stage name"
   - (Optional) Add a description
   - Click "Deploy"
   
   - Note the "Invoke URL" at the top of the stage editor
   - This is the base URL for your API

7. **Test the API**
   - In a new browser tab, paste the Invoke URL followed by "/items"
   - You should see the JSON response with the list of items
   
   - Try accessing a specific item by appending "/1" to the URL
   - You should see the details for item 1

8. **Create an SQS Integration**
   - Create a new resource called "messages"
   - Create a POST method for this resource
   - For "Integration type", select "AWS Service"
   - For "AWS Region", select your current region
   - For "AWS Service", select "Simple Queue Service (SQS)"
   - For "HTTP method", select "POST"
   - For "Action", enter "SendMessage"
   - For "Execution role", enter the ARN of a role with SQS permissions (create one if needed)
   - Click "Save"
   
   - In the Method Execution pane, click on "Integration Request"
   - Expand "URL Path Parameters" and add:
     - Name: "QueueUrl"
     - Mapped from: "method.request.querystring.queueUrl"
   
   - Expand "Mapping Templates"
   - Add a mapping template for "application/json":
     ```
     Action=SendMessage&MessageBody=$input.body
     ```
   - Click "Save"

9. **Enable CORS**
   - Select the "/items" resource
   - Click "Actions" and select "Enable CORS"
   - Leave the default settings
   - Click "Enable CORS and replace existing CORS headers"
   - Click "Yes, replace existing values"

10. **Create Usage Plans and API Keys**
    - In the left navigation pane, click "Usage Plans"
    - Click "Create"
    - Enter a name (e.g., "BasicPlan")
    - Set rate limiting:
      - Rate: 10 requests per second
      - Burst: 20 requests
    - Set quota:
      - Limit: 1000 requests
      - Period: Month
    - Click "Next"
    
    - Click "Create API Key and add to Usage Plan"
    - Enter a name (e.g., "MyAPIKey")
    - Click "Save"
    
    - Click "Next", then "Done"
    
    - Return to your API's "Resources" page
    - Select one of your methods
    - Click "Method Request"
    - Set "API Key Required" to "true"
    - Click "Save"
    
    - Re-deploy your API for changes to take effect

11. **Clean Up**
    - Delete the API
    - Delete any associated resources (SQS queues, IAM roles)

## Conclusion

In this hands-on lab, you've learned how to:
- Set up and use Amazon SQS for asynchronous messaging
- Create a pub/sub pattern with Amazon SNS for event notifications
- Build event-driven architectures with Amazon EventBridge
- Orchestrate complex workflows with AWS Step Functions
- Create and deploy RESTful APIs with Amazon API Gateway

These application integration services are essential for building decoupled, resilient, and scalable applications in AWS. They allow you to connect different components of your application together while maintaining loose coupling, which improves maintainability and allows for independent scaling of components. 