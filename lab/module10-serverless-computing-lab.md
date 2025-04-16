# Module 10: Serverless Computing - Hands-on Lab Guide

This guide provides detailed instructions for completing the hands-on exercises in Module 10.

## Exercise 1: Creating and Configuring AWS Lambda Functions

### Objective
Create, configure, and test AWS Lambda functions using different runtimes and triggers.

### Steps

1. **Sign in to the AWS Management Console**
   - Go to https://console.aws.amazon.com/
   - Sign in with your AWS account credentials

2. **Navigate to AWS Lambda Service**
   - In the search bar at the top, type "Lambda"
   - Select "Lambda" from the dropdown menu

3. **Create a Simple Lambda Function**
   - Click "Create function"
   - Select "Author from scratch"
   - Enter a function name (e.g., "HelloWorldFunction")
   - For "Runtime", select "Node.js 16.x"
   - For "Architecture", keep the default (x86_64)
   - Under "Permissions", select "Create a new role with basic Lambda permissions"
   - Click "Create function"

4. **Edit the Lambda Function Code**
   - In the "Code source" section, you'll see the default code editor
   - Replace the existing code with:
     ```javascript
     exports.handler = async (event) => {
         console.log('Event:', JSON.stringify(event, null, 2));
         
         const response = {
             statusCode: 200,
             body: JSON.stringify({
                 message: 'Hello from Lambda!',
                 input: event,
                 timestamp: new Date().toISOString()
             }),
         };
         
         return response;
     };
     ```
   - Click "Deploy" to save your changes

5. **Test the Lambda Function**
   - Click "Test" near the top of the page
   - If this is your first test, you'll need to configure a test event
   - Enter an event name (e.g., "TestEvent")
   - For the event template, select "hello-world"
   - Modify the JSON to:
     ```json
     {
       "key1": "value1",
       "key2": "value2",
       "key3": "value3"
     }
     ```
   - Click "Save"
   - Click "Test" again to execute the function with your test event
   - Review the execution results, including the response and log output

6. **Configure Function Settings**
   - Click the "Configuration" tab
   - Click "General configuration" and then "Edit"
   - Change the memory allocation to 256 MB
   - Set the timeout to 10 seconds
   - (Optional) Add a description
   - Click "Save"

7. **Create an Environment Variable**
   - Under the "Configuration" tab, click "Environment variables"
   - Click "Edit"
   - Click "Add environment variable"
   - Enter "GREETING" for the key and "Hello from the environment!" for the value
   - Click "Save"

8. **Update Your Function to Use the Environment Variable**
   - Return to the "Code" tab
   - Modify your function code:
     ```javascript
     exports.handler = async (event) => {
         console.log('Event:', JSON.stringify(event, null, 2));
         
         const greeting = process.env.GREETING || 'Hello from Lambda!';
         
         const response = {
             statusCode: 200,
             body: JSON.stringify({
                 message: greeting,
                 input: event,
                 timestamp: new Date().toISOString()
             }),
         };
         
         return response;
     };
     ```
   - Click "Deploy"
   - Test the function again to see the new greeting

9. **Create a Python Lambda Function**
   - Return to the Lambda console main page
   - Click "Create function"
   - Select "Author from scratch"
   - Enter a function name (e.g., "PythonFunction")
   - For "Runtime", select "Python 3.9"
   - Use the same permissions as before or create a new role
   - Click "Create function"
   
   - Replace the default code with:
     ```python
     import json
     import logging
     import datetime
     
     logger = logging.getLogger()
     logger.setLevel(logging.INFO)
     
     def lambda_handler(event, context):
         logger.info('Event: %s', json.dumps(event))
         
         current_time = datetime.datetime.now().isoformat()
         
         response = {
             'statusCode': 200,
             'body': json.dumps({
                 'message': 'Hello from Python Lambda!',
                 'timestamp': current_time,
                 'event': event
             })
         }
         
         return response
     ```
   - Click "Deploy"
   - Create a test event similar to before and test the function

10. **Add Layer to Your Lambda Function**
    - In the Lambda console, select your Python function
    - Click the "Layers" section in the Designer panel
    - Click "Add a layer"
    - Select "AWS Layers"
    - Choose "NumPy" or another AWS-provided layer
    - Select the appropriate version for your runtime
    - Click "Add"
    
    - Update your Python function to use the layer:
      ```python
      import json
      import logging
      import datetime
      import numpy as np
      
      logger = logging.getLogger()
      logger.setLevel(logging.INFO)
      
      def lambda_handler(event, context):
          logger.info('Event: %s', json.dumps(event))
          
          # Using NumPy to generate random numbers
          random_numbers = np.random.rand(5).tolist()
          
          current_time = datetime.datetime.now().isoformat()
          
          response = {
              'statusCode': 200,
              'body': json.dumps({
                  'message': 'Hello from Python Lambda!',
                  'timestamp': current_time,
                  'random_numbers': random_numbers,
                  'event': event
              })
          }
          
          return response
      ```
    - Click "Deploy" and test the function again

11. **Monitor the Lambda Function**
    - Click the "Monitor" tab
    - Explore the CloudWatch metrics for your function
    - Click "View CloudWatch logs" to see the logs from your function executions
    - Return to the Lambda console

12. **Clean Up (Optional)**
    - If you want to delete your functions, select each function
    - Click "Actions" and select "Delete"
    - Confirm the deletion by typing the function name

## Exercise 2: Creating Event Source Mappings

### Objective
Configure different event sources to trigger your Lambda functions automatically.

### Prerequisites
- Lambda functions created in Exercise 1
- Basic understanding of AWS services like S3, DynamoDB, and SQS

### Steps

1. **Set Up an S3 Trigger**
   - Navigate to your "HelloWorldFunction" Lambda function
   - Click the "Configuration" tab, then "Triggers"
   - Click "Add trigger"
   - From the dropdown, select "S3"
   - Select an existing bucket or create a new one
   - For "Event type", select "All object create events"
   - (Optional) Specify a prefix or suffix filter
   - Check the acknowledgment box
   - Click "Add"

2. **Create an S3 Bucket (if needed)**
   - If you need to create a new bucket, navigate to the S3 service
   - Click "Create bucket"
   - Enter a globally unique bucket name (e.g., "lambda-trigger-bucket-[your-initials]-[random-number]")
   - Keep the default settings
   - Click "Create bucket"
   - Return to the Lambda function configuration and add the trigger pointing to your new bucket

3. **Modify the Lambda Function for S3 Events**
   - Navigate to the "Code" tab of your Lambda function
   - Update your code to handle S3 events:
     ```javascript
     exports.handler = async (event) => {
         console.log('Event:', JSON.stringify(event, null, 2));
         
         // Process S3 events
         const s3Events = event.Records || [];
         const processedItems = s3Events.map(record => {
             if (record.eventSource === 'aws:s3') {
                 return {
                     bucket: record.s3.bucket.name,
                     key: record.s3.object.key,
                     size: record.s3.object.size,
                     eventTime: record.eventTime
                 };
             }
             return record;
         });
         
         const response = {
             statusCode: 200,
             body: JSON.stringify({
                 message: 'S3 event processed',
                 processedItems: processedItems,
                 timestamp: new Date().toISOString()
             }),
         };
         
         return response;
     };
     ```
   - Click "Deploy"

4. **Test the S3 Trigger**
   - Navigate to the S3 service
   - Select your bucket
   - Click "Upload"
   - Select a small test file from your computer
   - Click "Upload"
   - Return to your Lambda function
   - Click the "Monitor" tab, then "View CloudWatch logs"
   - Check the latest log stream to see the processed S3 event

5. **Set Up an SQS Trigger**
   - First, create an SQS queue:
     - Navigate to the SQS service
     - Click "Create queue"
     - Select "Standard" queue type
     - Enter a name (e.g., "LambdaTriggerQueue")
     - Keep default settings
     - Click "Create queue"
   
   - Navigate back to your Python Lambda function
   - Click the "Configuration" tab, then "Triggers"
   - Click "Add trigger"
   - From the dropdown, select "SQS"
   - Select your queue
   - Keep default batch size
   - Click "Add"

6. **Modify the Python Function for SQS Events**
   - Navigate to the "Code" tab
   - Update your code to handle SQS events:
     ```python
     import json
     import logging
     import datetime
     import numpy as np
     
     logger = logging.getLogger()
     logger.setLevel(logging.INFO)
     
     def lambda_handler(event, context):
         logger.info('Event: %s', json.dumps(event))
         
         # Process SQS events
         sqs_events = event.get('Records', [])
         processed_messages = []
         
         for record in sqs_events:
             if record.get('eventSource') == 'aws:sqs':
                 body = json.loads(record.get('body', '{}'))
                 processed_messages.append({
                     'messageId': record.get('messageId'),
                     'body': body
                 })
         
         # Using NumPy to generate random numbers
         random_numbers = np.random.rand(3).tolist()
         
         current_time = datetime.datetime.now().isoformat()
         
         response = {
             'statusCode': 200,
             'body': json.dumps({
                 'message': 'SQS events processed',
                 'timestamp': current_time,
                 'random_numbers': random_numbers,
                 'processed_messages': processed_messages
             })
         }
         
         return response
     ```
   - Click "Deploy"

7. **Test the SQS Trigger**
   - Navigate to the SQS service
   - Select your queue
   - Click "Send and receive messages"
   - Enter a test message:
     ```json
     {
       "test": "This is a test message",
       "priority": "high",
       "timestamp": "2023-06-30T12:00:00Z"
     }
     ```
   - Click "Send message"
   - Return to your Lambda function
   - Check the CloudWatch logs to see the processed SQS event

8. **Create a DynamoDB Table for Trigger**
   - Navigate to the DynamoDB service
   - Click "Create table"
   - Enter a table name (e.g., "LambdaTriggerTable")
   - For the primary key, enter "id" with type "String"
   - Leave default settings
   - Click "Create"

9. **Enable DynamoDB Streams**
   - Once the table is created, click on it
   - Click the "Exports and streams" tab
   - In the "DynamoDB stream details" section, click "Enable"
   - For "View type", select "New and old images"
   - Click "Enable stream"

10. **Create a New Lambda Function for DynamoDB**
    - Navigate to the Lambda service
    - Click "Create function"
    - Select "Author from scratch"
    - Enter a name (e.g., "DynamoDBStreamProcessor")
    - For "Runtime", select "Node.js 16.x"
    - Create or select an appropriate role with DynamoDB access
    - Click "Create function"
    
    - Use the following code:
      ```javascript
      exports.handler = async (event) => {
          console.log('Event:', JSON.stringify(event, null, 2));
          
          const dynamodbEvents = event.Records || [];
          const processedItems = dynamodbEvents.map(record => {
              if (record.eventSource === 'aws:dynamodb') {
                  return {
                      eventName: record.eventName,
                      tableName: record.eventSourceARN.split('/')[1],
                      keys: record.dynamodb.Keys,
                      newImage: record.dynamodb.NewImage,
                      oldImage: record.dynamodb.OldImage
                  };
              }
              return record;
          });
          
          const response = {
              statusCode: 200,
              body: JSON.stringify({
                  message: 'DynamoDB stream events processed',
                  processedItems: processedItems,
                  count: processedItems.length,
                  timestamp: new Date().toISOString()
              }),
          };
          
          return response;
      };
      ```
    - Click "Deploy"

11. **Add DynamoDB Trigger to the Function**
    - In the "Configuration" tab, click "Triggers"
    - Click "Add trigger"
    - Select "DynamoDB" from the dropdown
    - Select your DynamoDB table
    - Keep default batch size and starting position
    - Click "Add"

12. **Test the DynamoDB Trigger**
    - Navigate to the DynamoDB service
    - Select your table
    - Click "Explore table items"
    - Click "Create item"
    - For "id", enter a unique string (e.g., "item1")
    - Click "Add new attribute" and add some test attributes
    - Click "Create item"
    
    - Return to your Lambda function
    - Check the CloudWatch logs to see the processed DynamoDB event
    
    - Go back to the DynamoDB table and modify the item
    - Verify that the Lambda function processes the update event

13. **Clean Up (Optional)**
    - Delete the S3 bucket if created for this exercise
    - Delete the SQS queue
    - Delete the DynamoDB table
    - Remove triggers from your Lambda functions or delete the functions entirely 

## Exercise 3: Building Serverless Applications with AWS SAM

### Objective
Use the AWS Serverless Application Model (SAM) to define and deploy serverless applications.

### Prerequisites
- AWS CLI installed and configured
- AWS SAM CLI installed (https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/install-sam-cli.html)
- Docker installed (for local testing)
- Git installed

### Steps

1. **Initialize a SAM Project**
   - Open a terminal or command prompt
   - Create a directory for your project and navigate to it:
     ```
     mkdir sam-hello-world
     cd sam-hello-world
     ```
   - Initialize a new SAM application:
     ```
     sam init
     ```
   - Follow the prompts:
     - Select "AWS Quick Start Templates"
     - Choose "Hello World Example"
     - Select your preferred runtime (e.g., "nodejs16.x" or "python3.9")
     - For package type, select "Zip"
     - Name your project (e.g., "sam-hello-world")

2. **Explore the Project Structure**
   - Open the project directory in your code editor
   - Examine the key files:
     - `template.yaml`: SAM template defining your serverless resources
     - `hello-world/` directory: Contains your Lambda function code
     - `events/`: Contains sample event payloads for testing
     - `README.md`: Contains instructions for deploying and testing

3. **Understand the SAM Template**
   - Open `template.yaml` in your editor
   - Review the Resources section, which defines your Lambda function
   - Note how the template also defines an API Gateway endpoint
   - The template uses a simpler syntax than raw CloudFormation but compiles to CloudFormation

4. **Build the SAM Application**
   - In your terminal, run:
     ```
     sam build
     ```
   - This will compile your application and prepare it for deployment
   - The build process creates a `.aws-sam` directory with the packaged application

5. **Test Locally with SAM**
   - Invoke the function locally:
     ```
     sam local invoke HelloWorldFunction --event events/event.json
     ```
   - This will start a Docker container to simulate the Lambda environment
   - The function will execute, and you'll see the output in your terminal
   
   - Start a local API Gateway:
     ```
     sam local start-api
     ```
   - This will simulate API Gateway locally
   - Visit `http://localhost:3000/hello` in your browser to test the API
   - Press Ctrl+C to stop the local API

6. **Deploy the SAM Application**
   - Deploy your application to AWS:
     ```
     sam deploy --guided
     ```
   - Follow the deployment prompts:
     - Enter a stack name (e.g., "sam-hello-world")
     - Choose your AWS region
     - Confirm changes before deployment (Y)
     - Allow SAM CLI to create IAM roles (Y)
     - Disable rollback (N)
     - Save arguments to samconfig.toml (Y)
   - Wait for the deployment to complete
   - Note the outputs, including the API Gateway endpoint URL

7. **Test the Deployed Application**
   - Open a web browser and navigate to the API Gateway URL provided in the deployment outputs
   - You should see a JSON response from your Lambda function
   - Test the API with different parameters by appending query strings (e.g., `?name=YourName`)

8. **Modify the SAM Application**
   - Open your function code (e.g., `hello-world/app.js` or `hello-world/app.py`)
   - Make changes to the response
   - For example, in a Node.js function, modify the response to include a timestamp:
     ```javascript
     let response = {
         statusCode: 200,
         body: JSON.stringify({
             message: `Hello ${name}!`,
             timestamp: new Date().toISOString()
         }),
     };
     ```
   - Save your changes

9. **Update the Deployed Application**
   - Rebuild the application:
     ```
     sam build
     ```
   - Deploy the updates:
     ```
     sam deploy
     ```
   - Test the updated application by visiting the API Gateway URL again

10. **Add a DynamoDB Table Resource**
    - Open `template.yaml`
    - Add a DynamoDB table resource:
      ```yaml
      Resources:
        # ... existing resources ...
        
        MyTable:
          Type: AWS::Serverless::SimpleTable
          Properties:
            PrimaryKey:
              Name: id
              Type: String
            ProvisionedThroughput:
              ReadCapacityUnits: 5
              WriteCapacityUnits: 5
      ```
    - Update your function's permissions to access the table:
      ```yaml
      HelloWorldFunction:
        # ... existing configuration ...
        Policies:
          - DynamoDBCrudPolicy:
              TableName: !Ref MyTable
        Environment:
          Variables:
            TABLE_NAME: !Ref MyTable
      ```
    - Deploy the updates:
      ```
      sam build
      sam deploy
      ```

11. **Update Your Function to Use DynamoDB**
    - Modify your function code to interact with DynamoDB
    - For a Node.js function:
      ```javascript
      const AWS = require('aws-sdk');
      const dynamodb = new AWS.DynamoDB.DocumentClient();
      const tableName = process.env.TABLE_NAME;
      
      exports.lambdaHandler = async (event, context) => {
          const name = event.queryStringParameters && event.queryStringParameters.name ? event.queryStringParameters.name : 'World';
          
          // Store visit in DynamoDB
          const item = {
              id: Date.now().toString(),
              name: name,
              timestamp: new Date().toISOString()
          };
          
          try {
              await dynamodb.put({
                  TableName: tableName,
                  Item: item
              }).promise();
              
              // Get count of visits
              const result = await dynamodb.scan({
                  TableName: tableName,
                  Select: 'COUNT'
              }).promise();
              
              const response = {
                  statusCode: 200,
                  body: JSON.stringify({
                      message: `Hello ${name}!`,
                      visitCount: result.Count,
                      timestamp: item.timestamp
                  }),
              };
              
              return response;
          } catch (err) {
              console.log(err);
              return {
                  statusCode: 500,
                  body: JSON.stringify({
                      message: 'Error storing data in DynamoDB',
                      error: err.message
                  }),
              };
          }
      };
      ```
    - For a Python function:
      ```python
      import json
      import boto3
      import os
      import datetime
      
      dynamodb = boto3.resource('dynamodb')
      table = dynamodb.Table(os.environ['TABLE_NAME'])
      
      def lambda_handler(event, context):
          query_params = event.get('queryStringParameters', {}) or {}
          name = query_params.get('name', 'World')
          
          # Store visit in DynamoDB
          timestamp = datetime.datetime.now().isoformat()
          item = {
              'id': str(int(datetime.datetime.now().timestamp() * 1000)),
              'name': name,
              'timestamp': timestamp
          }
          
          try:
              table.put_item(Item=item)
              
              # Get count of visits
              response = table.scan(Select='COUNT')
              count = response.get('Count', 0)
              
              return {
                  'statusCode': 200,
                  'body': json.dumps({
                      'message': f'Hello {name}!',
                      'visitCount': count,
                      'timestamp': timestamp
                  })
              }
          except Exception as e:
              print(e)
              return {
                  'statusCode': 500,
                  'body': json.dumps({
                      'message': 'Error storing data in DynamoDB',
                      'error': str(e)
                  })
              }
      ```
    - Build and deploy the updates:
      ```
      sam build
      sam deploy
      ```

12. **Clean Up**
    - When you're done, delete the SAM application:
      ```
      sam delete
      ```
    - This will remove all AWS resources created by SAM

## Exercise 4: Building a Serverless REST API with API Gateway and Lambda

### Objective
Create a complete REST API for a simple resource using API Gateway, Lambda, and DynamoDB.

### Steps

1. **Create a DynamoDB Table for Products**
   - Navigate to the DynamoDB service
   - Click "Create table"
   - Enter "Products" for the table name
   - Set "id" as the partition key with type "String"
   - Leave default settings
   - Click "Create table"

2. **Create Lambda Functions for CRUD Operations**
   - Navigate to the Lambda service
   - Create a new Lambda function:
     - Name: "ProductsFunction"
     - Runtime: "Node.js 16.x"
     - Create a role with DynamoDB permissions (or use an existing one)
   
   - Use the following code:
     ```javascript
     const AWS = require('aws-sdk');
     const dynamodb = new AWS.DynamoDB.DocumentClient();
     const TABLE_NAME = 'Products';
     
     exports.handler = async (event) => {
         console.log('Event:', JSON.stringify(event, null, 2));
         
         let response;
         const method = event.httpMethod;
         const path = event.path;
         
         try {
             if (path === '/products') {
                 if (method === 'GET') {
                     // List all products
                     response = await listProducts();
                 } else if (method === 'POST') {
                     // Create new product
                     const product = JSON.parse(event.body);
                     response = await createProduct(product);
                 }
             } else if (path.startsWith('/products/')) {
                 const productId = path.split('/')[2];
                 
                 if (method === 'GET') {
                     // Get product by ID
                     response = await getProduct(productId);
                 } else if (method === 'PUT') {
                     // Update product
                     const product = JSON.parse(event.body);
                     response = await updateProduct(productId, product);
                 } else if (method === 'DELETE') {
                     // Delete product
                     response = await deleteProduct(productId);
                 }
             }
             
             if (!response) {
                 response = {
                     statusCode: 400,
                     body: JSON.stringify({ message: 'Invalid request' })
                 };
             }
             
         } catch (error) {
             console.error('Error:', error);
             response = {
                 statusCode: 500,
                 body: JSON.stringify({ message: 'Internal server error', error: error.message })
             };
         }
         
         return response;
     };
     
     async function listProducts() {
         const result = await dynamodb.scan({
             TableName: TABLE_NAME
         }).promise();
         
         return {
             statusCode: 200,
             body: JSON.stringify(result.Items)
         };
     }
     
     async function getProduct(productId) {
         const result = await dynamodb.get({
             TableName: TABLE_NAME,
             Key: { id: productId }
         }).promise();
         
         if (!result.Item) {
             return {
                 statusCode: 404,
                 body: JSON.stringify({ message: 'Product not found' })
             };
         }
         
         return {
             statusCode: 200,
             body: JSON.stringify(result.Item)
         };
     }
     
     async function createProduct(product) {
         // Generate an ID if not provided
         if (!product.id) {
             product.id = Date.now().toString();
         }
         
         await dynamodb.put({
             TableName: TABLE_NAME,
             Item: product
         }).promise();
         
         return {
             statusCode: 201,
             body: JSON.stringify(product)
         };
     }
     
     async function updateProduct(productId, product) {
         // Ensure the ID in the path matches the product
         product.id = productId;
         
         // Check if product exists
         const existingProduct = await dynamodb.get({
             TableName: TABLE_NAME,
             Key: { id: productId }
         }).promise();
         
         if (!existingProduct.Item) {
             return {
                 statusCode: 404,
                 body: JSON.stringify({ message: 'Product not found' })
             };
         }
         
         await dynamodb.put({
             TableName: TABLE_NAME,
             Item: product
         }).promise();
         
         return {
             statusCode: 200,
             body: JSON.stringify(product)
         };
     }
     
     async function deleteProduct(productId) {
         // Check if product exists
         const existingProduct = await dynamodb.get({
             TableName: TABLE_NAME,
             Key: { id: productId }
         }).promise();
         
         if (!existingProduct.Item) {
             return {
                 statusCode: 404,
                 body: JSON.stringify({ message: 'Product not found' })
             };
         }
         
         await dynamodb.delete({
             TableName: TABLE_NAME,
             Key: { id: productId }
         }).promise();
         
         return {
             statusCode: 204,
             body: ''
         };
     }
     ```
   - Click "Deploy"

3. **Create an API Gateway REST API**
   - Navigate to the API Gateway service
   - Click "Create API"
   - Select "REST API" (not private)
   - Click "Build"
   - For "Create new API", select "New API"
   - Enter "ProductsAPI" for the API name
   - Select "Regional" for endpoint type
   - Click "Create API"

4. **Create Resources and Methods**
   - Click "Actions" and select "Create Resource"
   - Enter "products" for "Resource Name"
   - Resource Path should be "/products"
   - Click "Create Resource"
   
   - With the "/products" resource selected:
     - Click "Actions" and select "Create Method"
     - Select "GET" and click the checkmark
     - For "Integration type", select "Lambda Function"
     - Check "Use Lambda Proxy integration"
     - Select your region and enter "ProductsFunction" for the Lambda function
     - Click "Save"
     
   - Repeat to create a POST method for "/products" with the same Lambda function
   
   - Create a child resource under "/products":
     - Click "Actions" and select "Create Resource"
     - Enter "Product ID" for "Resource Name"
     - Enter "{id}" for "Resource Path"
     - Click "Create Resource"
   
   - With the "/products/{id}" resource selected:
     - Create GET, PUT, and DELETE methods using the same Lambda function and proxy integration

5. **Deploy the API**
   - Click "Actions" and select "Deploy API"
   - For "Deployment stage", select "New Stage"
   - Enter "prod" for "Stage name"
   - Click "Deploy"
   - Note the "Invoke URL" at the top of the screen

6. **Test the API**
   - Use a tool like Postman, curl, or the browser to test your API
   - Test creating a product (POST to /products):
     ```
     curl -X POST -H "Content-Type: application/json" -d '{"name":"Test Product","price":19.99,"description":"A test product"}' https://your-api-id.execute-api.your-region.amazonaws.com/prod/products
     ```
   
   - List all products (GET /products):
     ```
     curl https://your-api-id.execute-api.your-region.amazonaws.com/prod/products
     ```
   
   - Get a specific product (replace {id} with an actual ID):
     ```
     curl https://your-api-id.execute-api.your-region.amazonaws.com/prod/products/{id}
     ```
   
   - Update a product (replace {id} with an actual ID):
     ```
     curl -X PUT -H "Content-Type: application/json" -d '{"name":"Updated Product","price":29.99,"description":"An updated product"}' https://your-api-id.execute-api.your-region.amazonaws.com/prod/products/{id}
     ```
   
   - Delete a product (replace {id} with an actual ID):
     ```
     curl -X DELETE https://your-api-id.execute-api.your-region.amazonaws.com/prod/products/{id}
     ```

7. **Enable CORS for the API**
   - Select your "/products" resource
   - Click "Actions" and select "Enable CORS"
   - Keep the default settings or customize as needed
   - Click "Enable CORS and replace existing CORS headers"
   - Click "Yes, replace existing values"
   
   - Repeat for the "/products/{id}" resource
   
   - Deploy the API again to apply the CORS settings

8. **Create an API Key and Usage Plan**
   - In the left navigation pane, click "API Keys"
   - Click "Actions" and select "Create API key"
   - Enter a name (e.g., "ProductsAPIKey")
   - Click "Save"
   
   - Click "Usage Plans" in the left navigation
   - Click "Create"
   - Enter a name (e.g., "ProductsAPIUsagePlan")
   - Configure throttling and quota if desired
   - Click "Next"
   
   - Click "Add API Stage"
   - Select your API and the "prod" stage
   - Click the checkmark
   - Click "Next"
   
   - Click "Add API Key to Usage Plan"
   - Select your API key
   - Click "Done"

9. **Require API Key for Methods**
   - Go back to your API's resources
   - Select each method (except OPTIONS, which is for CORS)
   - Click "Method Request"
   - Set "API Key Required" to "true"
   - Click the checkmark to save
   
   - Deploy the API again to apply the changes

10. **Test with API Key**
    - Test your API again, this time including the API key:
      ```
      curl -H "x-api-key: your-api-key-value" https://your-api-id.execute-api.your-region.amazonaws.com/prod/products
      ```

11. **Clean Up**
    - Delete the API Gateway API
    - Delete the Lambda function
    - Delete the DynamoDB table

## Exercise 5: Building a Serverless Web Application (Optional)

### Objective
Create a complete serverless web application using S3, API Gateway, Lambda, and DynamoDB.

### Prerequisites
- Completion of Exercise 4 (or have a working REST API)
- Basic knowledge of HTML, CSS, and JavaScript

### Steps

1. **Create an S3 Bucket for Web Hosting**
   - Navigate to the S3 service
   - Click "Create bucket"
   - Enter a globally unique name (e.g., "products-app-[your-initials]-[random-number]")
   - In the "Object Ownership" section, select "ACLs enabled"
   - In the "Block Public Access settings" section, uncheck "Block all public access"
   - Acknowledge the warning
   - Keep other settings as default
   - Click "Create bucket"

2. **Configure the Bucket for Static Website Hosting**
   - Select your bucket
   - Go to the "Properties" tab
   - Scroll down to "Static website hosting"
   - Click "Edit"
   - Select "Enable"
   - Enter "index.html" for both Index document and Error document
   - Click "Save changes"

3. **Create a Simple Web Application**
   - Create an `index.html` file on your local machine:
     ```html
     <!DOCTYPE html>
     <html lang="en">
     <head>
         <meta charset="UTF-8">
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>Products App</title>
         <style>
             body {
                 font-family: Arial, sans-serif;
                 max-width: 800px;
                 margin: 0 auto;
                 padding: 20px;
             }
             .product {
                 border: 1px solid #ddd;
                 padding: 10px;
                 margin-bottom: 10px;
                 border-radius: 5px;
             }
             .form-group {
                 margin-bottom: 15px;
             }
             label {
                 display: block;
                 margin-bottom: 5px;
             }
             input, textarea {
                 width: 100%;
                 padding: 8px;
                 box-sizing: border-box;
             }
             button {
                 padding: 8px 15px;
                 background-color: #4CAF50;
                 color: white;
                 border: none;
                 cursor: pointer;
             }
             .actions {
                 margin-top: 10px;
             }
             .actions button {
                 margin-right: 5px;
             }
             .actions button.delete {
                 background-color: #f44336;
             }
         </style>
     </head>
     <body>
         <h1>Products Management</h1>
         
         <div id="product-form">
             <h2>Add/Edit Product</h2>
             <form id="form">
                 <input type="hidden" id="product-id">
                 <div class="form-group">
                     <label for="name">Product Name:</label>
                     <input type="text" id="name" required>
                 </div>
                 <div class="form-group">
                     <label for="price">Price:</label>
                     <input type="number" id="price" step="0.01" required>
                 </div>
                 <div class="form-group">
                     <label for="description">Description:</label>
                     <textarea id="description" rows="3"></textarea>
                 </div>
                 <button type="submit">Save Product</button>
                 <button type="button" id="cancel" style="background-color: #ccc;">Cancel</button>
             </form>
         </div>
         
         <h2>Products List</h2>
         <div id="products-list"></div>
         
         <script>
             // Replace with your API Gateway URL
             const API_URL = 'https://your-api-id.execute-api.your-region.amazonaws.com/prod';
             // Replace with your API key
             const API_KEY = 'your-api-key';
             
             // DOM elements
             const productsList = document.getElementById('products-list');
             const form = document.getElementById('form');
             const productIdInput = document.getElementById('product-id');
             const nameInput = document.getElementById('name');
             const priceInput = document.getElementById('price');
             const descriptionInput = document.getElementById('description');
             const cancelButton = document.getElementById('cancel');
             
             // Fetch and display products
             async function fetchProducts() {
                 try {
                     const response = await fetch(`${API_URL}/products`, {
                         headers: {
                             'x-api-key': API_KEY
                         }
                     });
                     
                     if (!response.ok) {
                         throw new Error('Failed to fetch products');
                     }
                     
                     const products = await response.json();
                     displayProducts(products);
                 } catch (error) {
                     console.error('Error fetching products:', error);
                     alert('Failed to load products. See console for details.');
                 }
             }
             
             // Display products in the list
             function displayProducts(products) {
                 productsList.innerHTML = '';
                 
                 if (products.length === 0) {
                     productsList.innerHTML = '<p>No products found. Add a new product!</p>';
                     return;
                 }
                 
                 products.forEach(product => {
                     const productDiv = document.createElement('div');
                     productDiv.className = 'product';
                     productDiv.innerHTML = `
                         <h3>${product.name}</h3>
                         <p><strong>Price:</strong> $${product.price}</p>
                         <p>${product.description || 'No description'}</p>
                         <div class="actions">
                             <button class="edit" data-id="${product.id}">Edit</button>
                             <button class="delete" data-id="${product.id}">Delete</button>
                         </div>
                     `;
                     
                     // Edit button handler
                     productDiv.querySelector('.edit').addEventListener('click', () => {
                         editProduct(product);
                     });
                     
                     // Delete button handler
                     productDiv.querySelector('.delete').addEventListener('click', () => {
                         if (confirm(`Are you sure you want to delete ${product.name}?`)) {
                             deleteProduct(product.id);
                         }
                     });
                     
                     productsList.appendChild(productDiv);
                 });
             }
             
             // Prepare form for editing a product
             function editProduct(product) {
                 productIdInput.value = product.id;
                 nameInput.value = product.name;
                 priceInput.value = product.price;
                 descriptionInput.value = product.description || '';
                 
                 document.querySelector('#product-form h2').textContent = 'Edit Product';
                 document.querySelector('#form button[type="submit"]').textContent = 'Update Product';
             }
             
             // Reset form to add new product
             function resetForm() {
                 form.reset();
                 productIdInput.value = '';
                 document.querySelector('#product-form h2').textContent = 'Add Product';
                 document.querySelector('#form button[type="submit"]').textContent = 'Save Product';
             }
             
             // Add or update a product
             async function saveProduct(event) {
                 event.preventDefault();
                 
                 const isEditing = !!productIdInput.value;
                 const product = {
                     name: nameInput.value,
                     price: parseFloat(priceInput.value),
                     description: descriptionInput.value
                 };
                 
                 if (isEditing) {
                     product.id = productIdInput.value;
                 }
                 
                 try {
                     const url = isEditing ? 
                         `${API_URL}/products/${product.id}` : 
                         `${API_URL}/products`;
                     
                     const method = isEditing ? 'PUT' : 'POST';
                     
                     const response = await fetch(url, {
                         method: method,
                         headers: {
                             'Content-Type': 'application/json',
                             'x-api-key': API_KEY
                         },
                         body: JSON.stringify(product)
                     });
                     
                     if (!response.ok) {
                         throw new Error('Failed to save product');
                     }
                     
                     resetForm();
                     fetchProducts();
                 } catch (error) {
                     console.error('Error saving product:', error);
                     alert('Failed to save product. See console for details.');
                 }
             }
             
             // Delete a product
             async function deleteProduct(productId) {
                 try {
                     const response = await fetch(`${API_URL}/products/${productId}`, {
                         method: 'DELETE',
                         headers: {
                             'x-api-key': API_KEY
                         }
                     });
                     
                     if (!response.ok) {
                         throw new Error('Failed to delete product');
                     }
                     
                     fetchProducts();
                 } catch (error) {
                     console.error('Error deleting product:', error);
                     alert('Failed to delete product. See console for details.');
                 }
             }
             
             // Event listeners
             form.addEventListener('submit', saveProduct);
             cancelButton.addEventListener('click', resetForm);
             
             // Initialize
             resetForm();
             fetchProducts();
         </script>
     </body>
     </html>
     ```
   - Replace the placeholder API URL and API key with your actual values
   - Save the file

4. **Upload the Web Application to S3**
   - Navigate to your S3 bucket
   - Click "Upload"
   - Drag and drop your `index.html` file or click "Add files" to select it
   - Click "Upload"

5. **Update Bucket Policy for Public Access**
   - Select your bucket
   - Go to the "Permissions" tab
   - Scroll down to "Bucket policy" and click "Edit"
   - Enter the following policy (replace the bucket name with yours):
     ```json
     {
         "Version": "2012-10-17",
         "Statement": [
             {
                 "Sid": "PublicReadGetObject",
                 "Effect": "Allow",
                 "Principal": "*",
                 "Action": "s3:GetObject",
                 "Resource": "arn:aws:s3:::your-bucket-name/*"
             }
         ]
     }
     ```
   - Click "Save changes"

6. **Access the Web Application**
   - Go to the "Properties" tab of your bucket
   - Scroll down to "Static website hosting"
   - Click on the "Bucket website endpoint" URL
   - You should see your Products Management web application
   - Test the functionality:
     - Add a new product
     - Edit the product
     - Delete the product

7. **Enhance Your Application (Optional)**
   - Add search and filter capabilities
   - Implement pagination for large product lists
   - Add image upload for product photos
   - Implement user authentication

8. **Clean Up**
   - Delete objects from your S3 bucket
   - Delete the S3 bucket
   - Delete the API Gateway API, Lambda function, and DynamoDB table if you created them for this exercise

## Conclusion

In this hands-on lab, you've learned how to:
- Create and configure AWS Lambda functions with different runtimes
- Set up event source mappings to trigger Lambda functions
- Build serverless applications using AWS SAM
- Create a complete REST API using API Gateway and Lambda
- Develop a serverless web application using S3, API Gateway, Lambda, and DynamoDB

These serverless computing skills allow you to build highly scalable, cost-effective applications without managing servers. The serverless approach enables you to focus on your application code while AWS handles the infrastructure, scaling, and high availability. 