---
title : "Populate the DynamoDB Table"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2.2 </b> "
---

#### Populate the DynamoDB Table

DynamoDB is the key-value database that we will use to store information about all rides and attractions throughout the park.

The SAM template created a DynamoDB table for the application. Next, you'll populate a DynamoDB table with data about park rides and attractions. You will run a local Node script in this repo to bring the data to DynamoDB.

1. Run the command below to change the path to **local-app**.

```
cd ~/environment/sampleapp/theme-park-backend/1-app-deploy/local-app/
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00029.png?featherlight=false&width=90pc)

2. Install the necessary NPM packages.

```
npm install
```

{{% notice info %}}
You can ignore warnings or errors at this step, no need to run an npm audit.
{{% /notice %}}

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00030.png?featherlight=false&width=90pc)

3. Run the script to proceed with the import

```
node ./importData.js $AWS_REGION $DDB_TABLE

```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00031.png?featherlight=false&width=90pc)


Next, we will test some configurations before updating the Frontend configuration.

4. Check that the data has been entered into the DynamoDB table with the following command.

```
aws dynamodb scan --table-name $DDB_TABLE --select "COUNT"
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00032.png?featherlight=false&width=90pc)