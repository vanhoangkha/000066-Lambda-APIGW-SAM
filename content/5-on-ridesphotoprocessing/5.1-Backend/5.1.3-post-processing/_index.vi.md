---
title : "Post-processing"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 5.1.3 </b> "
---

#### Xử lý hậu kỳ

Lambda function cuối cùng trong quy trình xử lý ảnh được kích hoạt khi ảnh cuối cùng được hiển thị và lưu vào bộ xử lý ảnh S3 . Nó sẽ lưu thông tin  object ảnh vào bảng DynamoDB và gửi thông báo đến ioT topic để ứng dụng front-end được thông báo.

#### Tạo hàm Lambda


1. Truy cập vào **Lambda**
2. Chọn **Create function**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00098.png?featherlight=false&width=90pc)

3. Trong giao diện **Create function**

   + Chọn **Author from scatch**
   + Đối với **Basic information**, nhập **theme-park-photos-postprocess**
   + Đối với **Runtime**, chọn **Node.js 14.x**
   + Đối với **Architecture**, chọn **x86_64**
   + Chọn mở rộng **Change default execution role**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00099.png?featherlight=false&width=90pc)

4. Trong giao diện **Change default execution role**

   + Chọn **Use an existing role**
   + Chọn **theme-park-backend-ThemeParkLambdaRole**
   + Chọn **Create function**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000100.png?featherlight=false&width=90pc)

5. Hoàn thành tạo **Lambda function**

    + Chọn **+ Add trigger**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000101.png?featherlight=false&width=90pc)

6. Trong trang **Trigger configuration**

   + Chọn **S3**
   + **Bucket**, chọn **theme-park-backend-finalbucket**
   + Đối với **Event Type**, chọn  **All  object  create events**
   + Chọn **Recursive invocation acknowledgement**
   + Chọn **Add**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000102.png?featherlight=false&width=90pc)

7. Hoàn thành **Add trigger S3**

   + Chọn **Code** tab.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000103.png?featherlight=false&width=90pc)

8. Quay lại giao diện **Cloud9**

    + Chọn **3-postprocess/app.js**
    + Sao chép code của **app.js**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000104.png?featherlight=false&width=90pc)

9. Thực hiện dán code của **app.js** vào **index.js**

   + Lưu lại
   + Chọn **Deploy**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000105.png?featherlight=false&width=90pc)

#### Thêm các biến môi trường.

Hàm này sử dụng hai biến môi trường:

+ IOT_DATA_ENDPOINT: ioT endpoint hostname.
+ DDB_TABLE_NAME: tên bảng DynamoDB được ứng dụng sử dụng.

1. Quay lại tab trình duyệt của bạn khi Cloud9 đang chạy
2. Trong  terminal , hãy nhập lệnh sau để truy xuất giá trị cho IOT_DATA_ENDPOINT:

```
aws iot describe-endpoint --endpoint-type iot:Data-ATS
```
3. Tiếp theo, nhập lệnh sau để truy xuất giá trị cho DDB_TABLE_NAME:

```
aws dynamodb list-tables | grep backend
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000106.png?featherlight=false&width=90pc)

4. Quay lại **theme-park-photos-postprocess** Lambda function.

+  Chọn **Configuration tab**
+  Chọn **Environment variables**
+  Chọn **Edit**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000107.png?featherlight=false&width=90pc)

5. Nhập tên hai biến môi trường cùng với các giá trị bạn đã truy xuất trong Cloud9 (không có dấu ngoặc kép):

+ Chọn **Save**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000108.png?featherlight=false&width=90pc)

6. Hoàn thành thêm biến môi trường.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000109.png?featherlight=false&width=90pc)

#### Cập nhật Front-end

1. Quay lại tab trình duyệt của bạn khi Cloud9 đang chạy
2. Trong  terminal , hãy chạy lệnh này để hiển thị API tải lên từ khi chương trình backend được triển khai trong mô-đun 1:

```
aws cloudformation describe-stacks --stack-name theme-park-backend --query "Stacks[0].Outputs[?OutputKey=='UploadApi'].OutputValue" --output text
```


![Cloud9](/images/2-Prerequiste/2.1-cloud9/000110.png?featherlight=false&width=90pc)

3. Sao chép URL đầu ra vào khay nhớ tạm:


![Cloud9](/images/2-Prerequiste/2.1-cloud9/000111.png?featherlight=false&width=90pc)


4. Trong  Terminal  Cloud9, trong bảng điều khiển thư mục bên trái, điều hướng đến **theme-park-frontend/src**

5. Tìm **config.js** và nhấp đúp để mở trong trình chỉnh sửa. Tệp này chứa cấu hình JSON cho front-end.
6. Trong phần MODULE 3 , giữa các dấu ngoặc kép '', hãy cập nhật **photoUploadUR**L với **API Endpoint URL** trong khay nhớ tạm.
7. Sau đó, lưu tệp.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/000112.png?featherlight=false&width=90pc)

#### Push CodeCommit và triển khai qua Amplify

1. Trong  Terminal  Cloud9, thay đổi thành thư mục front-end bằng lệnh sau:

```
cd ~/environment/sampleapp/theme-park-frontend/
```
2. Commit  CodeCommit bằng cách thực hiện các lệnh sau:

```
git commit -am "Module 3 - Photo compositing"
git push
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000113.png?featherlight=false&width=90pc)

3. Sau khi commit hoàn tất, chuyển đến [Amplify Console](https://console.aws.amazon.com/amplify/)

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000114.png?featherlight=false&width=90pc)

4. Trong trang **All apps**

+ Chọn **theme-park-frontend**

5. Bạn sẽ thấy một bản dựng mới đã tự động bắt đầu do kết quả của cam kết mới trong repo mã cơ bản. Quá trình xây dựng này sẽ mất vài phút cho đến khi giai đoạn Xác minh hoàn tất

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000115.png?featherlight=false&width=90pc)

6. Mở URL ứng dụng đã publish trong trình duyệt.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000116.png?featherlight=false&width=90pc)

7. Nhấp vào một trong các cấu hình chuyến đi để mở trang mô tả chuyến đi. Bạn có thể thấy một nút mới đã xuất hiện - **“Add ride photo”.**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/000117.png?featherlight=false&width=90pc)



