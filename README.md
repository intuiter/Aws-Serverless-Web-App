                                               Serverless Web Application Hosting

**Architectural Diagram flow of stages/process involved in Web application functionality:**

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/1d099e02-01b3-4ee9-af5c-cee808d1f521)

# Overview:

In this serverless web app architecture, a user accesses the web app by entering its URL in a web browser. The request is sent to the CloudFront distribution associated with the app. CloudFront acts as the content delivery network (CDN) and client-facing edge network. It retrieves the front-end assets (HTML, CSS, JavaScript) from an S3 bucket configured as the origin for the CloudFront distribution. CloudFront delivers the static content (front-end assets) to the user's browser from the nearest edge location, ensuring low latency and faster response times.

The web app communicates with the backend through API Gateway, which serves as the entry point for the API requests. API Gateway is configured with two methods: GET and POST to handle incoming requests, which are integrated with Lambda functions. These Lambda functions are written in Python Runtime and contain the backend logic for the application.
The GET method in API Gateway is responsible for fetching employee details such as age, name, first name, and last name. When a user/client interacts with the web app to retrieve employee details, the request is first routed through CloudFront distribution. From there, GET method in API Gateway is triggered, which in turn invokes the corresponding Lambda function. The Lambda function interacts with the serverless database, DynamoDB executing necessary logic to retrieve the employee details. Once the details are fetched, the response is returned to the user through the CloudFront distribution, ensuring low-latency and efficient delivery. The API endpoint also performs necessary validations before returning the response to the user.

Similarly, the POST method in API Gateway is used for creating or updating items in the employee details database. When a user/client wants to update/create employee details they interact with web app and the request follows the same flow through CloudFront and API Gateway. The POST method in API Gateway invokes the associated Lambda function, which updates the database table in DynamoDB with the provided employee details. The response from the Lambda function is then sent back to the user through CloudFront distribution after passing through the API endpoint's validation process.

By utilizing this serverless architecture, the web app achieves scalability, reliability, and cost-efficiency and handles various levels of traffic. 
<p>&nbsp;</p>

# Implementation:

Initially, create S3 bucket to store frontend code files(Javascript, HTML) with name "dynamaicapp1" as shown below,
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/319554fd-5bea-47d2-bbf9-80a6e9897ca2)
<p>&nbsp;</p>
Secondly, IAM role has been created in a way Lamdba will have access to DynamoDB and permissions granted accordingly as read, write to list, update , get data etc:
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/e9df3c5c-f839-44b8-86d3-7c7b83cb4c71)
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/861444e5-e322-43c1-a4a3-72984265be83)
<p>&nbsp;</p>
DynamoDB created with name (WebAppdata_user) and required configurations applied and Primary key assigned is "emp ID" which uniquely identifies employees and their corresponding details.
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/908b2658-6a11-4634-b16c-c28686ab9729)

Then, two Lambda fucntions has been created one named as "InsertEmployee" function which as a result will create/update employee details to DynamoDB database as a record and other one "Getemployeedetails" function which as a result will fetch details from database and forward to cloud front distribution through API gateway which in return display details to the user.
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/cb6f1815-20b1-407c-9b02-5c6916ee07c9)
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/8d530d92-7896-4d2b-8571-abfa8bb8cd60)
 <p>&nbsp;</p>
 
From below figure it is shown that a table record of employee details has been updated to database,
 
![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/8239ae0a-b71a-4813-8a31-d869c82da441)
<p>&nbsp;</p>

Moving forward, CloudFront (Content delivery Network) distribution which will be used to traverse/deliver content through worldwide n/w called Edge Locations. Here S3 origin domain used with name dynamicapp1.s3.amazonaws.com. Here CloudFront allows you to create an Origin Access Identity (OAI) to restrict direct access to your S3 bucket from outside CloudFront. By configuring this we can ensure that content is only served through CloudFront, preventing direct access to the S3 bucket. A policy will be generated from cloud front for access content from S3 buckets through cloud front into public, it will be replaced in S3 policy. 
 
![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/8d1b9834-ef17-445b-93a2-772539ba39da)
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/297b338e-111c-407e-bfdf-59de875bf842)
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/85d006d6-badb-4476-aba1-62d81e6f6bd7)
<p>&nbsp;</p>

API gateway implemented with API type "REST API" and resources GET and POST method integrated with lambda functions. CORS is used when we want to allow client-side JavaScript code running in a web browser to make requests to that API from a different domain. By configuring CORS on the API Gateway, you can specify which domains are allowed to access the API endpoints.
 
![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/7717f649-b6bb-4d0d-8607-14c8841ca928)
![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/25c0881e-fc33-4791-9c00-5238ecb107c8)
<p>&nbsp;</p>

Below are the resource policies/permissions created for GET and POST methods that are integrated to lambda functions to invoke the API's.

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/2223311c-56cb-4779-9aac-8b794d572640)
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/281c4ee1-0271-47f4-8821-ae710a5176d5)
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/20c04de1-91d5-4519-9096-8fb01c96f856)
<p>&nbsp;</p>

API Throttling set to at the rate of 10000 requests per second to control the traffic to API requests from users/client side. API endpoint deployment has been done and URL as "https://wccchs8gme.execute-api.ap-south-1.amazonaws.com/Prod"

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/e8fa3a51-84af-412d-9591-5f60dde6aacf)
 <p>&nbsp;</p>
 
![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/a0569da5-c39f-4c71-9e75-318a4f9ee320)
<p>&nbsp;</p>

![image](https://github.com/intuiter/Aws-Serverless-Web-App/assets/135228471/0026e0f5-4426-40f0-93b2-10d5c82f179b)

Cloud Front distribution has been enabled and successfully hosted Serverless WebApp application to save user/employee details and fetch them accordingly from database.


