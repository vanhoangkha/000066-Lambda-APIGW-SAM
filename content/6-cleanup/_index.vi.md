---
title : "Dọn dẹp tài nguyên"
date :  "`r Sys.Date()`" 
weight : 6
chapter : false
pre : " <b> 6. </b> "
---

#### Dọn dẹp tài nguyên

#### S3 bucket

1. Từ Cloud9, để nhận danh sách các bucker được sử dụng cho công viên giải trí, hãy nhập:

```
aws s3 ls | grep theme-park
```

2. Xóa từng bucket và nội dung của nó, thay thế **your-bucket-name** bằng tên từng bucket:

```
aws s3 rb --force s3://your-bucket-name
```

#### CloudFormation

1. Từ Cloud9,  nhập lệnh

```
aws cloudformation list-stacks | grep theme-park
```

2. Xóa từng stack

```
aws cloudformation delete-stack --stack-name your-stack-name
```

#### Lambda function

1. Từ cloud9, nhập lệnh

```
aws lambda list-functions | grep theme-park
```

2. Xóa từng function

```
aws lambda delete-function --function-name your-function-name
```

#### Code Commit

1. Từ cloud9, nhập lệnh

```
aws codecommit delete-repository --repository-name 'theme-park-frontend'
```

####  Amplify Console

1. Nhập lệnh 

```
aws amplify list-apps
```

2. Xóa ứng dụng

```
aws amplify delete-app --app-id 'your-app-id'
```