# AWS Signup Notification System

A **serverless event-driven application** built on AWS that automatically sends email notifications whenever a new user signs up.

This project demonstrates real-world usage of **AWS Lambda, API Gateway, SNS, DynamoDB, IAM, and S3 Static Hosting**.

---

# Project Overview

When a user submits their details through a web interface:

1.User interact with website which is hosted via S3 bucket
2.  Request is sent via API Gateway
3. Lambda function processes the request
4. User data is stored in DynamoDB
5. SNS triggers an email notification
6. User receives confirmation email

---

# Architecture

```
User (Browser) ◀ ---------------------------------------
(If user also subscribed to SNS)                        |
     │                                                  |
     ▼                                                  |
S3 Static Website (Frontend)                            |
     │                                                  |
     ▼                                                  |
API Gateway (POST /signup)                              |
     │                                                  |
     ▼                                                  |
AWS Lambda                                              |
     │        │                                         |
     │        ├──► DynamoDB (Store User Data)           |
     │        └──► SNS Topic (Send Notification)        |
     │                         │                        |
     ▼                         ▼                        |
 IAM Role                Email Notification--------------
```

---

# AWS Services Used

* **Amazon S3** – Static website hosting
* **Amazon API Gateway** – HTTP API endpoint
* **AWS Lambda** – Backend logic
* **Amazon DynamoDB** – NoSQL database
* **Amazon SNS** – Email notifications
* **AWS IAM** – Role & permission management

---

# Application Flow

1. User opens the hosted website (S3)
2. Submits signup form
3. API Gateway receives POST request
4. Lambda function executes:

   * Validates input
   * Stores user in DynamoDB
   * Publishes message to SNS
5. SNS sends email to subscribed users

---

# Steps I Followed  In This Project

## 1. DynamoDB Setup
Go with defaultsettings while creating the table

![Table Creation](Key_Screenshots/01-Table_Creation.jpg)

You can add as many attributes that you want in table

![Item Creation](Key_Screenshots/02-Item_Creation_In_Table.jpg)

## 2. SNS Configuration

Create a **Standard** topic 

![Create Topic](Key_Screenshots/03-Using_SNS_Create_Topic.jpg)

Create a **Suscription**, select the your topic ARN and Choose Protocol as **Email**  
**NOTE: Give the email where you are going to get the mails from AWS SNS.**

![Notification](Key_Screenshots/04-Notification_From_AWS.jpg)

Once you create the subscription. You are going to det the Email from
AWS SNS to confirm Your Subsription as shown above. Confirm your 
Subscription to get Notifications.

![Subscription Confirm](Key_Screenshots/05-Confirm_Subscription.jpg)

## 3. IAM Role Setup

Create an IAM Role for **Lambda** and give the permissions to the role
1. AmazonDynamoDBFullAccess
2. AmazonSNSFullAccess
3. AWSLambdaBasicExecutionRole

![IAM Permission](Key_Screenshots/06-Permission_Of_IAM_Role.png)

## 4. Lambda Function
create a Lambda function select **Author from scratch** with 
**runtime as python 3.12**
but you can use other versions of python or other languages like 
Node.js ,java , etc.. to write the code.
Now let's use the IAM role which we created previously
Change the default execution role to **Use another role**
Choose your IAM role as the execution role.
![Lambda Role](Key_Screenshots/07-Use_IAM_Role_In_Lambda.jpg)

The Lambda function that I used -----> [Lambda Function](https://github.com/jaikishan-2007/AWS-signup-notification-system/blob/main/LambdaFunction.txt)

![Lambda Deploy](Key_Screenshots/08-Create_And_Deploy_LambdaFunction.jpg)

## 5. API Gateway

Create the API gateway, Build **HTTP API**
Add the integration Choose **Lambda**
Select the correct region and Lambda function
![HTTP_API](Key_Screenshots/09-HTTP_Api_Creation_with_Integration.jpg)
Select the Method as **POST**
I gave Resource path as **/signup** but you can give anything
like /sign,/xyz...
**NOTE:** The resource path which you give now will be used
to add the same resource path at the end of Inovke URL.
In stage keep the default settings.

![Route Setup](Key_Screenshots/10-Adding_Route_to_Api.jpg)

Once you are done with creation of API Gateway
In navigation panel you select the **stages** in deploy section
Here you are going to find your Invoke URL.

![Invoke URL](Key_Screenshots/11-Api_with_InvokeURL.jpg)

Give the permissions in CORS as show below.
These permissions helps us to link between API and S3.

![CORS](Key_Screenshots/19-Enabling_CORS_in_API.jpg)

## 6. Integration

**Copy the ARN** Which is present in your subcription
**copy till the name of your topic as shown below**

![SNS_ARN](Key_Screenshots/12-ARN_In_SNS.jpg)

Get back to lambda, In the Configuration tab 
choose Environment variables from the left menu
Here key **SNS_TOPIC_ARN** which is a varible that I used in my Lamda function 
If you have used differet variable kindly enter that variable name in key.
The value is the ARN which you copied previously.

![SNS Integration](Key_Screenshots/13-Integration_of_SNS_with_Lambda.jpg)

## 7. Testing

You can test the code in lambda function itself. Here I used **Post man**
to test

![Postman Test](Key_Screenshots/14-Testing_Using_Postman_Application.jpg)
Success in test

![Test1](Key_Screenshots/15-Test_complete.jpg)

Data in Dynamo DB updated
![Data Stored](Key_Screenshots/16-User_Added_To_List_in_DynamoDB.jpg)

Mail from AWS SNS

![Notification Sent](Key_Screenshots/17-User_Added_Notification.jpg)

## 8. Frontend Hosting
Create a S3 bucket and enable the **Static WebsiteHosting**
Upload the html file make sure in html you connected the SignUp
button with api invoke url like -->https://vbqzd30a1j.execute-api.ap-south-2.amazonaws.com/signup  
NOTE: Don't forgot to mention your Resource path /xyz.. In my case it is **/signup** 

![S3 Static Website](Key_Screenshots/18-S3bucket_With_Enabled_StaticWebsite.jpg)

S3 bucket policy and Frontend html------>[S3](https://github.com/jaikishan-2007/AWS-signup-notification-system/tree/main/S3_bucket)

## 9. Output
Now go to the the staticwebsite and enter the credentials

![Website Test](Key_Screenshots/20-Testing_website.jpg)
![Success](Key_Screenshots/21-Test_Successful.jpg)
![Email](Key_Screenshots/22-Email_sent_by_SNS.jpg)

Successfully table in DynamoDB is updated

![Data Updated](Key_Screenshots/23-Data_Updated_In_Table.jpg)

---

# IAM Permissions

Lambda is assigned an IAM role with permissions to:

* Write to DynamoDB
* Publish to SNS
* Log to CloudWatch

---

# SNS Email Notification

* Users must confirm subscription
* Once confirmed, they receive real-time alerts

---

# Testing

* API tested using Postman
* End-to-end testing via hosted website

---

#  Key Learnings

* Serverless architecture design
* Event-driven workflows
* AWS service integration
* Managing environment variables and IAM roles securely

---

# Future Improvements

* Add user authentication (Cognito)
* Add input validation & error handling
* Store logs for analytics
* Add SMS notifications via SNS
* Deploy using Infrastructure as Code (Terraform / CloudFormation)

---

# Challenges Faced & Solutions during project

## 1. CORS Issues Between Frontend and API

Problem:
The frontend hosted on S3 was unable to communicate with API Gateway due to CORS restrictions, resulting in “Network Error” in the browser.

Solution:
Enabled CORS in API Gateway give the permissions.
[CORS permissions](https://github.com/jaikishan-2007/AWS-signup-notification-system/blob/main/Key_Screenshots/19-Enabling_CORS_in_API.jpg)

## 2. Environment Variable Misconfiguration

Problem:
Lambda function failed with KeyError due to incorrect environment variable usage (e.g., using ARN directly instead of variable key).

Solution:
Properly defined environment variables:

TABLE_NAME
SNS_TOPIC_ARN

and accessed them correctly using os.environ.

## 3. Data Mismatch Between Frontend and Backend

Problem:
Mismatch in lambda keys (UserId vs userId) caused data not to be processed correctly.

Solution:
Standardized request format across frontend and Lambda function.

## 4. SNS Notification Not Triggering

Problem:
Email notifications were not received.

Solution:

Verified SNS topic ARN configuration

Confirmed email subscription

Ensured proper IAM permissions for Lambda

---
# Conclusion

This project demonstrates how to build a **scalable, serverless notification system** using AWS services. It is a great project for understanding cloud-native application design.

---

# Author

**Korada Jaikishan**
Cloud Computing Learner | AWS Beginner
---

