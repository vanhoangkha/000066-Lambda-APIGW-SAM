---
title : "Kiểm tra cấu hình"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 3.2.3 </b> "
---

#### Kiểm tra cấu hình

1. Kiểm tra dữ liệu đã được đưa vào DynamoDB table với lệnh sau.
```
aws dynamodb scan --table-name $DDB_TABLE --select "COUNT"
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00033.png?featherlight=false&width=90pc)

2. Chạy câu lệnh dưới đây để kiểm tra endpoint URL
```
aws cloudformation describe-stacks --stack-name theme-park-backend --query "Stacks[0].Outputs[?OutputKey=='InitStateApi'].OutputValue" --output text
```
{{% notice tip %}}
Chúng ta sẽ lưu lại thông tin endpoint URL vào ghi chú cho các bước sau.
{{% /notice %}}

+ Click vào endpoint URL và chọn **Open**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00034.png?featherlight=false&width=90pc)

3. Một browser tab sẽ được mở và hiện toàn bộ thông tin các chuyến đi dưới dạng raw.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00035.png?featherlight=false&width=90pc)

Như vậy ở bước này chúng ta đã tiến hành tạo public API để Frontend của chúng ta có thể sử dụng.

