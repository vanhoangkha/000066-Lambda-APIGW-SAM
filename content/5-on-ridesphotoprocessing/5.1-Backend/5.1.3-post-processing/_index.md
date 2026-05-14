---
title : "Post-processing"
date: 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.1.3 </b> "
---

#### Post-processing

The last lambda function in the image processing is fired when the final image is rendered and saved to the S3 image processor. It will save the image object information to the DynamoDB table and send a notification to the IoT topic for the front-end application to be notified.

#### Create Lambda function


1. Access to **Lambda**
2. Select **Create function**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00098.png?featherlight=false&width=90pc)

3. In the **Create function** interface

   + Select **Author from catch**
   + For **Basic information**, enter **theme-park-photos-postprocess**
   + For **Runtime**, select **Node.js 14.x**
   + For **Architecture**, choose **x86_64**
   + Choose to expand **Change default execution role**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00099.png?featherlight=false&width=90pc)

4. In the **Change default execution role** interface

   + Select **Use an existing role**
   + Select **theme-park-backend-ThemeParkLambdaRole**
   + Select **Create function**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000100.png?featherlight=false&width=90pc)

5. Finish creating **Lambda function**

    + Select **+ Add trigger**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000101.png?featherlight=false&width=90pc)

6. In the **Trigger configuration** page

   + Select **S3**
   + **Bucket**, select **theme-park-backend-finalbucket**
   + For **Event Type**, select **All object create events**
   + Select **Recursive invocation acknowledgment**
   + Select **Add**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000102.png?featherlight=false&width=90pc)

7. Complete **Add trigger S3**

   + Select **Code** tab.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000103.png?featherlight=false&width=90pc)

8. Return to **Cloud9** interface

    + Select **3-postprocess/app.js**
    + Copy the code of **app.js**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000104.png?featherlight=false&width=90pc)

9. Paste the code of **app.js** into **index.js**

   + Save
   + Select **Deploy**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000105.png?featherlight=false&width=90pc)

#### Add environment variables.

This function uses two environment variables:

+ IOT_DATA_ENDPOINT: IoT endpoint hostname.
+ DDB_TABLE_NAME: DynamoDB table name used by the application.

1. Return to your browser tab while Cloud9 is running
2. In the terminal, enter the following command to retrieve the value for IOT_DATA_ENDPOINT:

```
aws iot describe-endpoint --endpoint-type iot:Data-ATS
```
3. Next, enter the following command to retrieve the value for DDB_TABLE_NAME:

```
aws dynamodb list-tables | grep backend
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000106.png?featherlight=false&width=90pc)

4. Go back to **theme-park-photos-postprocess** Lambda function.

+ Select **Configuration tab**
+ Select **Environment variables**
+ Select **Edit**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000107.png?featherlight=false&width=90pc)

5. Enter the two environment variable names along with the values ​​you retrieved in Cloud9 (without the quotes):

+ Select **Save**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000108.png?featherlight=false&width=90pc)

6. Finish adding environment variables.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000109.png?featherlight=false&width=90pc)

#### Front-end update

1. Return to your browser tab while Cloud9 is running
2. In the terminal , run this command to expose the upload API since the backend is implemented in module 1:

```
aws cloudformation describe-stacks --stack-name theme-park-backend --query "Stacks[0].Outputs[?OutputKey=='UploadApi'].OutputValue" --output text
```


![Cloud9](/images/2-Prerequiste/2.1-cloud9/000110.png?featherlight=false&width=90pc)

3. Copy the output URL to the clipboard:


![Cloud9](/images/2-Prerequiste/2.1-cloud9/000111.png?featherlight=false&width=90pc)


4. In Terminal Cloud9, in the left directory panel, navigate to **theme-park-frontend/src**

5. Find **config.js** and double click to open it in the editor. This file contains the JSON configuration for the front-end.
6. In the MODULE 3 section, between the quotes '', update **photoUploadUR**L with **API Endpoint URL** in the clipboard.
7. Then save the file.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/000112.png?featherlight=false&width=90pc)

#### Push CodeCommit and Deploy via Amplify

1. In Terminal Cloud9, change to the front-end directory with the following command:

```
cd ~/environment/sampleapp/theme-park-frontend/
```
2. Commit CodeCommit by executing the following commands:

```
git commit -am "Module 3 - Photo compositing"
git push
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000113.png?featherlight=false&width=90pc)

3. After the commit is complete, go to [Amplify Console](https://console.aws.amazon.com/amplify/)

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000114.png?featherlight=false&width=90pc)

4. On **All apps** page

+ Select **theme-park-frontend**

5. You should see a new build automatically started as a result of the new commit in the underlying code repo. This build will take a few minutes until the Verification phase is complete

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000115.png?featherlight=false&width=90pc)

6. Open the published application URL in the browser.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000116.png?featherlight=false&width=90pc)

7. Click one of the trip profiles to open the trip description page. You can see a new button has appeared - **“Add ride photo”.**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000117.png?featherlight=false&width=90pc)