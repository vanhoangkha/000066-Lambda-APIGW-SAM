---
title : "Test the configuration"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.2.3 </b> "
---

#### Check configuration

1. Check the data has been inserted into the DynamoDB table with the following command.
```
aws dynamodb scan --table-name $DDB_TABLE --select "COUNT"
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00033.png?featherlight=false&width=90pc)

2. Run the command below to check the endpoint URL
```
aws cloudformation describe-stacks --stack-name theme-park-backend --query "Stacks[0].Outputs[?OutputKey=='InitStateApi'].OutputValue" --output text
```
{{% notice tip %}}
We will save the endpoint URL information in the notes for the following steps.
{{% /notice %}}

+ Click on the URL endpoint and select **Open**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00034.png?featherlight=false&width=90pc)

3. A browser tab will be opened and all trip information will be displayed in raw format.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00035.png?featherlight=false&width=90pc)

So in this step, we have created a public API for our Frontend to use.