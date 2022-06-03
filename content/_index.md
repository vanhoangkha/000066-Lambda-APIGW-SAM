---
title : "Serverless with Lambda, API Gateway and SAM"
date : "`r Sys.Date()`"
weight : 1
chapter : false
---

# Serverless with Lambda, API Gateway and SAM

This exciting new theme park, built on the remote island of Isla Sanserver, combines rollercoasters and rides with shows and exhibits. The park will open every day and expects up to 50,000 visitors daily. It’s self-guided, using a web application that guests can browse on their smartphones.

The only “slight” problem is that the development team has suddenly left and the park’s Grand Opening is today! You only have hours to finish assembling the remaining pieces of the application before the gates open. But don’t worry, serverless is at hand! These instructions will guide you through using AWS services to assemble a complete application so you can save the day.

#### Application structure
You will be using a micro-services approach to configure a frontend and build a backend serverless application.

#### Frontend
The frontend web application consists of an existing JavaScript web application managed with AWS Amplify Console that interfaces with services on the backend. You will only need to make minor changes to a configuration file in the frontend code to complete this workshop.

Amplify Console provides a simple, Git-based workflow for deploying and hosting fullstack serverless web applications. Amplify Console can create both the frontend and backend but for this workshop we will be using Amplify Console for only the frontend.

Amplify will be used to host static web resources including HTML, CSS, JavaScript, and image files which are loaded in the user’s browser via S3.

#### Backend
The backend application architecture uses AWS Lambda, Amazon API Gateway, Amazon S3, Amazon DynamoDB, and Amazon Cognito.

JavaScript executed in the frontend browser application sends and receives data from a public backend API built using API Gateway and Lambda. DynamoDB provides a persistence data storage layer which is used by the API’s Lambda functions.

See the diagram below for the complete architecture.

![Serverless](/images/1-Introduce/architecture-light.png?featherlight=false&width=90pc)

#### Content

1. [Introduction](1-introduce/)
2. [Preparation](2-prerequiste/)
3. [Deploy sample application](3-deployapp/)
4. [Implement ride times system](4-realtime-ridewaittimes/)
5. [Vehicle Image Processing](5-on-ridesphotoprocessing/)
6. [Resource Cleanup](6-cleanup/)