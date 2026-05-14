---
title : "Triển khai Backend"
date: 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---

#### Triển khai Backend.

Khách của công viên thích chụp ảnh trên các chuyến đi và các điểm tham quan. Tính năng ứng dụng này sẽ cho phép khách chụp ảnh tự sướng và xem ảnh ghép trong ứng dụng. Từ đây, họ có thể publish kiệt tác của mình trên phương tiện truyền thông xã hội.

#### Cách hoạt động

+ Front-end gọi một  API endpoint để lấy một URL được chỉ định trước để tải ảnh lên S3. Điều này cho phép ứng dụng front-end tải trực tiếp lên S3 mà không cần máy chủ web. Điều này dẫn đến một  object JPG mới trong **S3 Upload bucket** .
+ Khi một  object mới được ghi vào Upload bucket, hàm này sẽ gọi hàm Lambda đầu tiên trong chuỗi, hàm này sử dụng một quy trình có tên là Chromakey để xóa nền màn hình màu xanh lục khỏi hình ảnh. Hình ảnh kết quả được ghi vào processing bucket.
+ Khi một  object mới được ghi vào Processing bucket, điều này sẽ gọi hàm Lambda tiếp theo để kết hợp hình ảnh với nền mới và biểu trưng của công viên giải trí. Hình ảnh kết quả này được ghi vào Final bucket.
+ Cuối cùng, khi một  object mới được ghi vào Final bucket, điều này sẽ gọi hàm Lambda cuối cùng sẽ gửi thông báo đến  IoT core rằng tệp đã sẵn sàng. Điều này thông báo cho ứng dụng front-end thông qua các thông báo IoT topic.

#### The Serverless Backend

![Cloud9](/images/5-photoprocessing/5.1-Backend/module4-architecture.png?featherlight=false&width=90pc)

1. Hình ảnh được tải lên bởi front-end vào Upload bucket.
2. Quy trình chromakey loại bỏ nền và lưu  object vào Processing bucket.
3. Quá trình tổng hợp tạo ra hình ảnh cuối cùng được lưu vào Final bucket.
4. Một thông báo được gửi đến IoT Core để thông báo cho front-end rằng tệp hiện đã sẵn sàng.

Trong mô-đun này, bạn sẽ triển khai ba hàm Lambda. Sau khi thiết lập, bạn sẽ cập nhật mã front-end để kích hoạt quá trình tải lên.

Sau khi được định cấu hình, bạn sẽ:

+ Kiểm tra phần backend bằng cách sử dụng hình ảnh mẫu để đảm bảo luồng hoạt động như mong đợi.
+ Kiểm tra front-end bằng cách chụp ảnh tự sướng trước màn hình màu xanh lá cây và Tweet nó với mọi người!

#### Thiết lập các biến môi trường

Chạy các lệnh sau trong terminal  Cloud9 để đặt các biến môi trường được sử dụng:

```
AWS_REGION=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone | sed 's/\(.*\)[a-z]/\1/')
FINAL_BUCKET=$(aws cloudformation describe-stack-resource --stack-name theme-park-backend --logical-resource-id FinalBucket --query "StackResourceDetail.PhysicalResourceId" --output text)
UPLOAD_BUCKET=$(aws cloudformation describe-stack-resource --stack-name theme-park-backend --logical-resource-id UploadBucket --query "StackResourceDetail.PhysicalResourceId" --output text)
accountId=$(curl -s http://169.254.169.254/latest/dynamic/instance-identity/document | jq -r .accountId)
s3_deploy_bucket="theme-park-sam-deploys-${accountId}"
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00062.png?featherlight=false&width=90pc)

Các biến môi trường không được lưu trữ trong terminal . Bất kỳ lúc nào bạn đóng Cloud9 hoặc mở một terminal  mới, bạn sẽ cần chạy lại các lệnh này để đặt các biến môi trường. Phần này được cung cấp trong mỗi mô-đun.

#### Triển khai cơ sở hạ tầng

Mô-đun này có ba phần:

1. [Tạo hàm Lambda chromakey](5.1.1-chromakeylambda/)
2. [Tạo hàm Lambda tổng hợp](5.1.2-photocompositinglambda/)
3. [Tạo lambda function thông báo sau xử lý](5.1.3-post-processing/)