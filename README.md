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

# ⚙️ AWS Services Used

* **Amazon S3** – Static website hosting
* **Amazon API Gateway** – HTTP API endpoint
* **AWS Lambda** – Backend logic
* **Amazon DynamoDB** – NoSQL database
* **Amazon SNS** – Email notifications
* **AWS IAM** – Role & permission management

---

# 🔄 Application Flow

1. User opens the hosted website (S3)
2. Submits signup form
3. API Gateway receives POST request
4. Lambda function executes:

   * Validates input
   * Stores user in DynamoDB
   * Publishes message to SNS
5. SNS sends email to subscribed users

---

# 📸 Key Screenshots

## 1. DynamoDB Setup

![Table Creation](Key_Screenshots/01-Table_Creation.jpg)
![Item Creation](Key_Screenshots/02-Item_Creation_In_Table.jpg)

## 2. SNS Configuration

![Create Topic](Key_Screenshots/03-Using_SNS_Create_Topic.jpg)
![Notification](Key_Screenshots/04-Notification_From_AWS.jpg)
![Subscription Confirm](Key_Screenshots/05-Confirm_Subscription.jpg)

## 3. IAM Role Setup

![IAM Permission](Key_Screenshots/06-Permission_Of_IAM_Role.png)
![Lambda Role](Key_Screenshots/07-Use_IAM_Role_In_Lambda.jpg)

## 4. Lambda Function

![Lambda Deploy](Key_Screenshots/08-Create_And_Deploy_LambdaFunction.jpg)

## 5. API Gateway

![Invoke URL](Key_Screenshots/11-Api_with_InvokeURL.jpg)
![CORS](Key_Screenshots/19-Enabling_CORS_in_API.jpg)

## 6. Integration

![SNS Integration](Key_Screenshots/13-Integration_of_SNS_with_Lambda.jpg)

## 7. Testing

![Postman Test](Key_Screenshots/14-Testing_Using_Postman_Application.jpg)
![Website Test](Key_Screenshots/20-Testing_website.jpg)
![Success](Key_Screenshots/21-Test_Successful.jpg)

## 8. Output

![Data Stored](Key_Screenshots/16-User_Added_To_List_in_DynamoDB.jpg)
![Notification Sent](Key_Screenshots/17-User_Added_Notification.jpg)
![Email](Key_Screenshots/22-Email_sent_by_SNS.jpg)
![Data Updated](Key_Screenshots/23-Data_Updated_In_Table.jpg)

## 9. Frontend Hosting

![S3 Static Website](Key_Screenshots/18-S3bucket_With_Enabled_StaticWebsite.jpg)

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
* IAM role usage

---

# Future Improvements

* Add user authentication (Cognito)
* Add input validation & error handling
* Store logs for analytics
* Add SMS notifications via SNS
* Deploy using Infrastructure as Code (Terraform / CloudFormation)

---

# ⭐ Conclusion

This project demonstrates how to build a **scalable, serverless notification system** using AWS services. It is a great project for understanding cloud-native application design.

---

# Author

**Jaikishan Korada**

---

