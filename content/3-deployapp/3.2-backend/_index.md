---
title : "Backend Deployment"
date: 2024-01-01
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

#### Backend Deployment

In this section, we will proceed to implement the backend for our sample application.

The front end needs to display details about the rides and attractions in the park to be useful to visitors. Once you've built the backend in this section, our guests will be able to see more useful information in the app.

#### Backend Architecture

![Serverless](/images/3-deployapp/3.2-backend/module1-backend-arch.png?featherlight=false&width=60pc)

Our backend architecture includes:
+ A DynamoDB table that you will fill with information about all the rides and attractions in the park.
+ A Lambda function that performs a table scan on DynamoDB to return all entries.
An API Gateway API creates a public http endpoint for front-end applications to query. This calls the Lambda function to return a list of rides and attractions.

After we build the necessary backend resources, we will update the front-end application configuration to query the API Gateway endpoint in the next section.

#### Content
- [Deploy Infrastructure](3.2.1-infastructure/)
- [Populate DynamoDB](3.2.2-populatedata/)
- [Test Configuration](3.2.3-testconfig)