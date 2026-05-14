---
title : "Frontend Deployment"
date: 2024-01-01
weight : 1
chapter : false
pre : " <b> 3.1 </b> "
---

#### Clone code base

1. Return to the terminal interface of **Cloud9**.

2. Copy the commands below and paste them into the **terminal** interface.

```
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
```

   + To save login information.
   + Use for different repositories should configure the credentials to use the full repository path.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00013.png?featherlight=false&width=90pc)

3. Create a directory to contain **source code front-end**.

```
mkdir ~/environment/sampleapp/theme-park-frontend
cd ~/environment/sampleapp/theme-park-frontend
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00014.png?featherlight=false&width=90pc)
4. Run the commands below to download the source code from **GitHub** and copy it to a subdirectory.

   + Then, extract with **unzip**

```
wget --no-check-certificate https://github.com/First-Cloud-Journey/000066-serverlesssample/raw/8b8f82165b55afcd4e4ccb2066511d1ab425b646/theme-park-frontend-202110.zip
unzip theme-park-frontend-202110.zip
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00015.png?featherlight=false&width=90pc)

5. Run the following command to configure **local Git repository and commit**.

```
git init -b main
git add .
git commit -am "First commit"
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00016.png?featherlight=false&width=90pc)

6. Push the code to the **CodeCommit repository** we created for the Frontend part.

```
AWS_REGION=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone | sed 's/\(.*\)[a-z]/\1/')
git push --set-upstream https://git-codecommit.$AWS_REGION.amazonaws.com/v1/repos/theme-park-frontend main
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00017.png?featherlight=false&width=90pc)

#### Deploy with Amplify

1. Access the **Amplify** service administration interface

2. Change the Region to **Region** you use for the exercise.
   + Scroll down, click **Get started** at **Host your web app**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00018.png?featherlight=false&width=90pc)

3. Click **AWS CodeCommit**.
   + Click **Continue**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00019.png?featherlight=false&width=90pc)

4. At the **Add repository branch** page.
   + Click to select **repository theme-park-frontend**.
   + In the **Branch** section, click **main**.
   + Click **Next**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00020.png?featherlight=false&width=90pc)

5. On the **Configure build settings** page, keep the default options and click **Next**.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/00021.png?featherlight=false&width=90pc)

6. On the **Review** page, check the configuration again and click **Save and deploy**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00022.png?featherlight=false&width=90pc)

7. Once the build process has completed the **Verify** stage, choose to open the link provided for your app in a new tab.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00023.png?featherlight=false&width=90pc)

8. You will see a blank park map with a navigation bar. We will add those features next.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00024.png?featherlight=false&width=90pc)

9. Copy **web app's published URL** into your notes file. You will need this information in the next steps.