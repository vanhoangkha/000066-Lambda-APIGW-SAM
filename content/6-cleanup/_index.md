---
title : "Clean up resources"
date: 2024-01-01
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

#### Clean up resources

#### S3 bucket

1. From Cloud9, to get a list of buckers used for theme parks, enter:

```
aws s3 ls | grep theme-park
```

2. Delete each bucket and its contents, replacing **your-bucket-name** with the name of each bucket:

```
aws s3 rb --force s3://your-bucket-name
```

#### CloudFormation

1. From Cloud9, enter the command

```
aws cloudformation list-stacks | grep theme-park
```

2. Clear each stack

```
aws cloudformation delete-stack --stack-name your-stack-name
```

#### Lambda function

1. From cloud9, enter the command

```
aws lambda list-functions | grep theme-park
```

2. Delete each function

```
aws lambda delete-function --function-name your-function-name
```

#### Code Commit

1. From cloud9, enter the command

```
aws codecommit delete-repository --repository-name 'theme-park-frontend'
```

#### Amplify Console

1. Enter the command

```
aws amplify list-apps
```

2. Delete the app

```
aws amplify delete-app --app-id 'your-app-id'
```