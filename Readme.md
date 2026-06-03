AWS Lambda communicates with DynamoDB using the Boto3 SDK. When the Lambda function executes, it assumes an IAM execution role that provides temporary credentials. Using these credentials, Lambda sends HTTPS API requests to DynamoDB. DynamoDB validates the IAM permissions, processes the request, stores the data in the table, and returns a response back to Lambda. Lambda then sends the final response to API Gateway, which returns it to the user.

# AWS Serverless Registration Application

## Project Overview

This project demonstrates a complete serverless web application built using AWS services. Users can register through a web form, and their details are securely stored in DynamoDB using AWS Lambda and API Gateway.

## Architecture Diagram

```text
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
```

---

# Detailed Request Flow

### Step 1: User Opens Website

1. User opens the application URL.
2. Browser sends a GET request.
3. API Gateway receives the request.
4. API Gateway invokes Lambda.
5. Lambda reads `index.html`.
6. Registration form is displayed to the user.

---

### Step 2: User Submits Registration Form

1. User enters:

   * First Name
   * Last Name
   * Email
   * Message

2. User clicks Submit.

3. Browser sends a POST request.

4. API Gateway receives the request.

5. API Gateway invokes Lambda.

6. Lambda extracts form data from the request body.

7. Lambda creates a DynamoDB client using Boto3:

```python
client = boto3.client('dynamodb')
```

8. Lambda assumes its IAM Execution Role.

9. AWS automatically provides temporary credentials.

10. Lambda sends an HTTPS API request to DynamoDB.

11. DynamoDB validates permissions.

12. DynamoDB stores the record in the `veera` table.

13. DynamoDB returns a success response.

14. Lambda loads `success.html`.

15. API Gateway returns the success page to the browser.

16. User sees the confirmation page.

---

# AWS Services Communication

```text
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
```

---

# How Lambda Communicates with DynamoDB

AWS Lambda communicates with DynamoDB using the Boto3 SDK.

The communication process is:

1. Lambda receives the request from API Gateway.
2. Lambda processes the user data.
3. Lambda creates a DynamoDB client using Boto3.
4. AWS automatically provides temporary credentials through the Lambda Execution Role.
5. Lambda sends HTTPS API requests to DynamoDB.
6. DynamoDB validates IAM permissions.
7. DynamoDB stores the item and returns a response.
8. Lambda returns the final response to API Gateway.

No Access Keys or Secret Keys are required inside the Lambda code.

---

# AWS Services Used

| Service     | Purpose                                   |
| ----------- | ----------------------------------------- |
| HTML/CSS    | Frontend User Interface                   |
| API Gateway | Entry Point for HTTP Requests             |
| AWS Lambda  | Serverless Compute Layer                  |
| DynamoDB    | NoSQL Database                            |
| IAM Role    | Secure Authorization                      |
| Boto3 SDK   | Communication Between Lambda and DynamoDB |

---

# Key Learning Outcomes

* Serverless Architecture Design
* API Gateway Integration with Lambda
* DynamoDB Data Storage
* IAM Roles and Permissions
* Boto3 SDK Usage
* Frontend and Backend Integration
* AWS Security Best Practices

---

# Interview Explanation

When the user submits the registration form, API Gateway invokes the Lambda function. Lambda processes the form data and uses the Boto3 SDK to call DynamoDB APIs. AWS automatically provides temporary credentials through the Lambda execution IAM role. DynamoDB validates the permissions, stores the record in the `veera` table, and returns a success response. Lambda then returns `success.html` through API Gateway back to the user's browser.

This is a classic Serverless Architecture using:

* API Gateway (Entry Point)
* AWS Lambda (Compute Layer)
* DynamoDB (Database Layer)
* HTML/CSS (Frontend Layer)
