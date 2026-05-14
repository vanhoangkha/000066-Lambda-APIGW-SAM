---
title : "Tạo Photo-Compositing Lambda function"
date: 2024-01-01
weight : 2
chapter : false
pre : " <b> 5.1.2 </b> "
---

#### Tạo Ghép ảnh Lambda function

+ Hàm lấy hình ảnh đã xử lý từ phần trước - người bị xóa phông xanh - và kết hợp các yếu tố đồ họa khác nhau thành hình ảnh cuối cùng. Hình ảnh cuối cùng này được lưu trong Final bucket.

+ Trong phần cuối cùng, bạn đã xây dựng một hàm Lambda theo cách thủ công thông qua bảng điều khiển. Tuy nhiên, nếu hàm của bạn chứa các gói hoặc thư viện, thì việc sử dụng tự động hóa sẽ dễ dàng hơn để giúp bạn nhóm các nội dung lại với nhau để triển khai.

+ Trong phần này, bạn sẽ sử dụng **AWS Serverless Application Model (SAM)** để tự động hóa gói và triển khai function.

#### SAM và YAML

Tệp mẫu AWS SAM là tệp cấu hình **YAML hoặc JSON**. Bạn sử dụng mẫu để khai báo tất cả các resource AWS tạo nên serverless của bạn.

1. Chuyển đến tab trình duyệt của bạn khi Cloud9 đang chạy. 

2. Trong bảng điều khiển trình khám phá tệp Cloud9, hãy điều hướng đến và mở **theme-park-backend\3-photos\2-compositing\template.yaml** để xem lại nội dung của nó.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00086.png?featherlight=false&width=90pc)

SAM sẽ đọc tệp này và chuyển đổi YAML này thành cơ sở hạ tầng. Một số phần quan trọng  bao gồm:

+ **Parameters** : hàm cần biết tên của final bucket, vì vậy bạn có thể cung cấp thông số này làm tham số cho mẫu.
+ **Globals** : mọi cài đặt ở đây sẽ áp dụng trên toàn bộ mẫu.
+ **Resources** : xác định resource AWS để tạo.

Trong phần **Resources**:

+ Mẫu xác định một hàm Lambda duy nhất được gọi là CompositeFunction .
+ Nó chỉ định thời gian chạy, kích thước bộ nhớ và nơi mã có thể được đặt (trong thư mục lambdaFunction).
+ Nó định nghĩa một biến môi trường, sử dụng FinalBucketName tham số làm đầu vào.
+ Nó cung cấp chính sách IAM để cho phép truy cập function tới S3.

#### Tạo Lambda function sử dụng **SAM**

1. Thay đổi thư mục:

```
cd ~/environment/sampleapp/theme-park-backend/3-photos/2-compositing
```

2. Bạn có thể thấy tên của final S3 bucket  bằng cách sử dụng lệnh sau đây đã được lưu trữ dưới dạng một biến môi trường **$FINAL_BUCKET**. **SAM** sẽ được cấu hình để sử dụng tên s3 bucket này để đặt biến môi trường trong hàm Lambda.

```
aws s3 ls | grep finalbucket
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00087.png?featherlight=false&width=90pc)

3. Trong  terminal , thực hiện các lệnh SAM CLI sau đây sẽ tạo ứng dụng SAM, đóng gói mã với cùng một nhóm triển khai SAM S3 đã được sử dụng trước đó và sau đó triển khai ứng dụng chỉ định tên Final bucket S3 cho hàm Lambda để sử dụng:

```
sam build

sam package --output-template-file packaged.yaml --s3-bucket $s3_deploy_bucket

sam deploy --template-file packaged.yaml --stack-name theme-park-photos --capabilities CAPABILITY_IAM --parameter-overrides "FinalBucketName"=$FINAL_BUCKET
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00088.png?featherlight=false&width=90pc)

4. Quá trình này sẽ mất vài phút để triển khai - hãy đợi thông báo xác nhận trong bảng điều khiển trước khi tiếp tục.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00089.png?featherlight=false&width=90pc)

#### Thêm S3 trigger

Bây giờ bạn đã tạo hàm Lambda, bạn cần phải cấu hình cách nó được gọi. Hàm tổng hợp này cần thực thi khi một  object  mới được đưa vào bộ xử lý . Trong phần này, bạn sẽ tạo trình kích hoạt này.

1. Đi tới bảng điều khiển Lambda - từ Bảng điều khiển quản lý AWS, chọn **Service** rồi chọn **Lambda**. Đảm bảo rằng region của bạn là chính xác.
2. Chọn function có tên **theme-park-photos-CompositeFunction-XXXXXXXXX**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00090.png?featherlight=false&width=90pc)

3. Trong giao diện Lambda function

+ Chọn **+ Add Trigger**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00091.png?featherlight=false&width=90pc)

4. Trong **Trigger configuration**

+ Chọn **S3**
+ **Bucket**, chọn **theme-park-backend-processingbucket-xxx**
+ Đối với **Event Type**, chọn **All  object  create events**
+ Kiểm tra **Recursive invocation acknowledgement**
+ Chọn **Add**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00092.png?featherlight=false&width=90pc)

5. Hoàn thành **Add trigger S3**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00093.png?featherlight=false&width=90pc)

#### Kiểm tra function

Bây giờ bạn sẽ kiểm tra function bằng cách sử dụng hình ảnh thử nghiệm có chứa ảnh của một người trên nền màu xanh lá cây. Bạn sẽ sao chép hình ảnh này vào uploadbucket và xem kết quả cuối cùng trong finalbucket .

1. Quay lại tab trình duyệt của bạn khi Cloud9 đang chạy. 
2. Trong  terminal , nhập lệnh sau để thay đổi thư mục:

```
cd ~/environment/sampleapp/theme-park-backend/3-photos/
```
3. Bạn có thể thấy tên của upload bucket S3 của mình bằng lệnh sau:

```
aws s3 ls | grep uploadbucket
```
4. Sao chép hình ảnh thử nghiệm  local   vào upload bucket của bạn. Lệnh sẽ sử dụng biến môi trường đã tạo trước $UPLOAD_BUCKETđó được đặt thành tên nhóm này:

```
aws s3 cp ./green-screen-test.png s3://$UPLOAD_BUCKET
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00094.png?featherlight=false&width=90pc)

5. Trong một tab khác, mở bảng điều khiển S3.
6. Chọn **theme-park-backend-finalbucket**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00095.png?featherlight=false&width=90pc)

7. Kiểm tra **green-screen-test.jpg**,

   + Sau đó chọn **Download**
   + Lưu tệp  local   và mở trong trình xem ảnh.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00096.png?featherlight=false&width=90pc)

8. Bạn sẽ thấy ảnh của một người đã bị xóa nền xanh và bây giờ được ghép với nền công viên giải trí và đồ họa logo.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00097.png?featherlight=false&width=30pc)