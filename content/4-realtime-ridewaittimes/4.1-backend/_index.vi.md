---
title : "Triển khai Backend"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

#### Tổng quan 

+ Controller publish các bản cập nhật mỗi phút cho  Amazon  SNS topic  . 
+ Bạn sẽ tạo một hàm Lambda trong tài khoản của mình, hàm này được gọi bất cứ khi nào có thông báo về chủ đề này. Function này sẽ lưu trữ thông báo trong DynamoDB và chuyển tiếp thông báo đến IoT Core.
+ Cuối cùng, bạn sẽ cập nhật cấu hình ứng dụng front-end để listen endpoint IoT này và republish frontend.
+ Thông tin thêm về các dịch vụ được giới thiệu trong phần này:

    + [Dịch vụ thông báo đơn giản của Amazon](https://aws.amazon.com/sns/)
    + [AWS IoT Core](https://aws.amazon.com/iot-core/)

#### The Real-Time Serverless Backend

![The Real-Time Serverless Backend](/images/4-ridewaittimes/4.1-backend/module3-overview.png?featherlight=false&width=60pc)

+ Bộ điều khiển luồng & lưu lượng đã được triển khai và publish các bản cập nhật cho  SNS topic .
+ Hàm Lambda nhận thông báo mới dưới dạng  event payload  và phân tích cú pháp thông báo. Sau đó, nó lưu trữ thông báo trong bảng DynamoDB và chuyển tiếp đến ioT topic.
+ Bảng DynamoDB chỉ lưu trữ tin nhắn cuối cùng. Trạng thái ban đầu này là cần thiết khi ứng dụng front-end được tải lần đầu tiên.
+ IoT topic là đường dẫn từ chương trình backend serverless đến ứng dụng front-end. Bất kỳ tin nhắn nào được đăng ở đây sẽ được nhận bởi front-end.

#### Thiết lập các biến môi trường


1. Chạy các lệnh sau trong terminal  Cloud9 để đặt các biến môi trường 

```
AWS_REGION=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone | sed 's/\(.*\)[a-z]/\1/')
accountId=$(curl -s http://169.254.169.254/latest/dynamic/instance-identity/document | jq -r .accountId)
s3_deploy_bucket="theme-park-sam-deploys-${accountId}"
```

{{% notice info %}}
Các biến môi trường không được lưu trữ trong terminal . Bất kỳ lúc nào bạn đóng Cloud9 hoặc mở một terminal  mới, bạn sẽ cần chạy lại các lệnh này để đặt các biến môi trường. Phần này được cung cấp trong mỗi mô-đun.
{{% /notice %}}


![Cloud9](/images/2-Prerequiste/2.1-cloud9/00042.png?featherlight=false&width=90pc)

#### Tạo hàm Lambda

2. Đi tới bảng điều khiển **Lambda - từ Bảng điều khiển quản lý AWS**, chọn **Service** rồi chọn [Lambda](https://console.aws.amazon.com/lambda). Đảm bảo rằng **Region** của bạn là chính xác. 

   + Bạn sẽ thấy một số hàm Lambda mà **SAM** đã triển khai.
   + Chọn **Create function**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00043.png?featherlight=false&width=90pc)

3. Trong trang **Create function**

   + Chọn **Author from scratch**
   + Phần **Basic information**, **Function name**, nhập **```theme-park-ridetimes```**
   + Đối với **Run time**, chọn **Nodejs 14.x**
   + **Architecture**, chọn **arm64**
   + Chọn mở rộng **Change default execution**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00044.png?featherlight=false&width=90pc)

4. Đối với **Change default execution**

   + Chọn **Use an existing role**
   + Đối với **Existing role**, chọn **theme-park-backend-ThemeParkLambdaRole-XXX**
   + Chọn **Create function**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00045.png?featherlight=false&width=90pc)

5. Hoàn thành tạo function. Tiếp theo, chúng ta chọn **+ Add trigger**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00046.png?featherlight=false&width=90pc)

6. Thực hiện **Trigger configuration**

   + Chọn **SNS**
   + ** SNS topic **, chọn **theme-park-ride-times-XXX**
   + Chọn **Add**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00047.png?featherlight=false&width=90pc)

7. Sau khi **Trigger configuration** thành công. 

+ Chọn **Code**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00048.png?featherlight=false&width=90pc)

8. Quay trở lại **Cloud9 tab**. 

   + Chọn **theme-park-backend**.
   + Chọn **2-realtime**.
   + Chọn **app.js**.
   + Thực hiện sao chép code của **app.js**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00049.png?featherlight=false&width=90pc)

9. Quay trở lại **Lambda console**.

+ Chọn **index.js**.
+ Dán code của **app.js** vào.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00050.png?featherlight=false&width=90pc)

Mã hàm Lambda này đọc thông báo mới nhất từ ​​chủ đề SNS, ghi nó vào bảng DynamoDB, sau đó đẩy thông báo đến ứng dụng front-end thông qua ioT topic.

#### Thêm các biến môi trường

Hàm này sử dụng ba biến môi trường:

+ **IOT_DATA_ENDPOINT**: ioT endpoint hostname.
+ **IOT_TOPIC**: Tên của ioT topic để publish thông điệp **theme-park-rides**.
+ **DDB_TABLE_NAME**: Tên bảng DynamoDB của ứng dụng.

1. Quay lại tab trình duyệt của bạn khi Cloud9 đang chạy. 

2. Trong terminal , hãy nhập lệnh sau để truy xuất giá trị cho **IOT_DATA_ENDPOINT**:

```
aws iot describe-endpoint --endpoint-type iot:Data-ATS
```

3. Tiếp theo, nhập lệnh sau để truy xuất giá trị cho **DDB_TABLE_NAME**:

```
aws dynamodb list-tables | grep backend
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00051.png?featherlight=false&width=90pc)

4. Quay lại tab trình duyệt với **theme-park-ridetimes Lambda function** đang mở. 


   + Chọn tab **Configuration** , 
   + Sau đó chọn tùy chọn menu **Environment variables** ở bên trái.
   + Chọn **Edit**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00052.png?featherlight=false&width=90pc)

5. Chọn **Add environment variable** ba lần để nhập ba biến môi trường sau:

   + **IOT_DATA_ENDPOINT** - giá trị từ bước 2 ở trên (không có dấu ngoặc kép).
   + **DDB_TABLE_NAME** - giá trị từ bước 3 ở trên (không có dấu ngoặc kép).
   + **IOT_TOPIC** -**theme-park-rides**
   + Chọn **Save**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00053.png?featherlight=false&width=90pc)

6. Hoàn thành thêm biến môi trường.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00054.png?featherlight=false&width=90pc)


