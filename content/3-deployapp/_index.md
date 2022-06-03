---
title : "Deploying sample app"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3. </b> "
---

#### Deploy sample application

The sample application we will deploy is a theme park application that includes a front end and a backend. In this module, you will set up the frontend and then connect it to the backend.

The frontend is a Progressive Web App (PWA) developed using Vue.js.

The frontend source code is provided so you can simply download the code and deploy it to your repository in CodeCommit. Using the AWS Amplify Console, you will create a continuous deployment process to publish your application.

The backend is a collection of small serverless services. In this section, we will set up the following:

+ A DynamoDB table containing information about all rides and attractions in the park.
+ A Lambda function that performs a table scan on DynamoDB to return all entries.
An API Gateway API creates a public http endpoint for frontend applications to query. This calls the Lambda function to return a list of rides and attractions.

Once you have built the necessary back-end resources, you will update the front-end application configuration to query the API endpoint Gateway and display information about all rides and attractions.

#### Content
- [Frontend Deployment](3.1-frontend/)
- [Backend Deployment](3.2-backend/)
- [Update Frontend](3.3-updatefrontend)