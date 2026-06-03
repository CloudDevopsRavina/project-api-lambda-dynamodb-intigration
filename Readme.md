AWS Lambda communicates with DynamoDB using the Boto3 SDK. When the Lambda function executes, it assumes an IAM execution role that provides temporary credentials. Using these credentials, Lambda sends HTTPS API requests to DynamoDB. DynamoDB validates the IAM permissions, processes the request, stores the data in the table, and returns a response back to Lambda. Lambda then sends the final response to API Gateway, which returns it to the user.

A clean architecture diagram for your project would look like this:

┌─────────────────────┐
│      End User       │
│  (Web Browser)      │
└──────────┬──────────┘
           │
           │ GET / POST
           ▼
┌─────────────────────┐
│     API Gateway     │
│  Public Endpoint    │
└──────────┬──────────┘
           │
           │ Invokes
           ▼
┌─────────────────────┐
│    AWS Lambda       │
│  Python Backend     │
│                     │
│  lambda_handler()   │
└──────────┬──────────┘
           │
           │ Boto3 SDK
           │ HTTPS API Call
           ▼
┌─────────────────────┐
│      DynamoDB       │
│  Table : veera      │
│                     │
│ fname               │
│ lname               │
│ email               │
│ message             │
└──────────┬──────────┘
           │
           │ Success
           ▼
┌─────────────────────┐
│    AWS Lambda       │
│ Returns Success     │
│      HTML Page      │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│      End User       │
│ "Registration       │
│ Successful" Page    │
└─────────────────────┘
Detailed Request Flow
1. User opens website
   │
   ▼
2. API Gateway receives GET request
   │
   ▼
3. Lambda reads index.html
   │
   ▼
4. Registration Form displayed

------------------------------------------------

5. User enters details and clicks Submit
   │
   ▼
6. Browser sends POST request
   │
   ▼
7. API Gateway triggers Lambda
   │
   ▼
8. Lambda extracts form data
   │
   ▼
9. Lambda uses Boto3
      client = boto3.client('dynamodb')
   │
   ▼
10. IAM Role provides permissions
   │
   ▼
11. DynamoDB stores record
   │
   ▼
12. DynamoDB returns success
   │
   ▼
13. Lambda loads success.html
   │
   ▼
14. API Gateway returns response
   │
   ▼
15. User sees confirmation page
AWS Services Communication
                AWS Cloud
┌─────────────────────────────────────┐

    API Gateway
         │
         ▼
    AWS Lambda
         │
         │ IAM Role
         ▼
      Boto3 SDK
         │
         │ HTTPS API
         ▼
      DynamoDB

└─────────────────────────────────────┘
Interview Explanation

When the user submits the registration form, API Gateway invokes the Lambda function. Lambda processes the form data and uses the Boto3 SDK to call DynamoDB APIs. AWS automatically provides temporary credentials through the Lambda execution IAM role. DynamoDB validates the permissions, stores the record in the veera table, and returns a success response. Lambda then returns success.html through API Gateway back to the user's browser.

This is a classic Serverless Architecture using:

API Gateway (Entry Point)
AWS Lambda (Compute Layer)
DynamoDB (Database Layer)
HTML/CSS (Frontend Layer)
