---
title : "Create Cloud9 instance"
date: 2024-01-01
weight : 1
chapter : false
pre : " <b> 2.1 </b> "
---

#### Create Cloud9 instance


1. Go to [Cloud9 service administration interface](https://console.aws.amazon.com/cloud9/home)
    + Switch **Region** to **Region** you use for practice.
    + Click **Create environment**.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/0001.png?featherlight=false&width=90pc)

2. In the Name field, enter **serverless-development**.

    + Click **Next step**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/0002.png?featherlight=false&width=90pc)

3. At the **Configure Settings** page.

    + Click to select **t3.small**.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/0003.png?featherlight=false&width=90pc)

4. Select **Next step**.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/0004.png?featherlight=false&width=90pc)

5. Check again and select **Create environment**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/0005.png?featherlight=false&width=90pc)

6. Cloud9 instance creation will take a few minutes.

- Click on close Welcome tab.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/0006.png?featherlight=false&width=90pc)

7. Click **+**, then click **New Terminal** to open a new terminal interface.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/0007.png?featherlight=false&width=90pc)


8. Install **JQ** to support **JSON** format with the command below.

```
sudo yum install jq -y
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/0008.png?featherlight=false&width=90pc)

{{% notice warning %}}
Make sure you have successfully installed JQ before performing the next steps.
{{% /notice %}}

9. Copy the command below and paste it into the ** terminal ** interface.

    + This command is to create a directory containing **sampleapp**.
    + Change the path to the directory.

```
mkdir sampleapp
cd sampleapp
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/0009.png?featherlight=false&width=90pc)

Next we will proceed to create the CodeCommit repository.