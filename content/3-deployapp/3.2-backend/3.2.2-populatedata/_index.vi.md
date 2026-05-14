---
title : "Điền thông tin chuyến đi"
date: 2024-01-01
weight : 2 
chapter : false
pre : " <b> 3.2.2 </b> "
---

#### Điền thông tin chuyến đi vào DynamoDB

DynamoDB là cơ sở dữ liệu key-value mà chúng tôi sẽ sử dụng để lưu trữ thông tin về tất cả các chuyến đi và điểm tham quan trong toàn bộ công viên.

SAM template đã tạo một bảng DynamoDB cho ứng dụng. Tiếp theo, bạn sẽ điền vào bảng DynamoDB với dữ liệu về các chuyến đi và điểm tham quan trong công viên. Bạn sẽ chạy một tập lệnh Node  local   trong repo này để đưa dữ liệu lên DynamoDB.

1. Chạy lệnh dưới đây để chuyển đường dẫn tới **local-app**.

```
cd ~/environment/sampleapp/theme-park-backend/1-app-deploy/local-app/
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00029.png?featherlight=false&width=90pc)

2. Tiến hành cài đặt các NPM packages cần thiết.

```
npm install
```

{{% notice info %}}
Bạn có thể bỏ qua các warning hoặc errors ở bước này, không cần phải chạy npm audit.
{{% /notice %}}

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00030.png?featherlight=false&width=90pc)

3. Chạy script để tiến hành import

```
node ./importData.js $AWS_REGION $DDB_TABLE

```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00031.png?featherlight=false&width=90pc)


Tiếp theo chúng ta sẽ kiểm tra một số cấu hình trước khi cập nhật cấu hình phần Frontend.

4. Kiểm tra dữ liệu đã được đưa vào DynamoDB table với lệnh sau.

```
aws dynamodb scan --table-name $DDB_TABLE --select "COUNT"
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00032.png?featherlight=false&width=90pc)

