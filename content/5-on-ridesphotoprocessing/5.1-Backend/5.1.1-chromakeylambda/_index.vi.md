---
title : "Tạo Chroma Key Lambda function"
date: 2024-01-01
weight : 1
chapter : false
pre : " <b> 5.1.1 </b> "
---

#### Tạo Chroma Key Lambda function

Thực thi [chroma key processing](https://en.wikipedia.org/wiki/Chroma_key) còn thường được gọi là màn hình xanh. Nó lấy một hình ảnh đầu vào của một người trên nền màu xanh lục, loại bỏ màu xanh lá cây, sau đó lưu hình ảnh đầu ra.

#### Nội dung

Các hàm Lambda có thể được viết trong các thời gian chạy khác nhau và cũng có thể sử dụng các thư viện mã đóng gói sẵn được gọi là các lớp Lambda.

- Phần này cho biết cách bạn có thể sử dụng các thời gian chạy khác nhau cho các tác vụ khác nhau. Function xử lý  chroma key sử dụng một thư viện Python mã nguồn mở được gọi là OpenCV. Hàm này được triển khai bằng Python 3.7 trong khi các hàm khác được viết bằng Node.
- Thư viện OpenCV phải được biên dịch bằng hệ điều hành đích, dành cho Lambda là Amazon Linux 2. Để đơn giản hóa việc triển khai, điều này đã được tạo dưới dạng tệp zip để bạn tạo một lớp.
Thông tin thêm về các dịch vụ được giới thiệu trong phần này:

[AWS Lambda layers](https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html)

#### Tạo lớp Lambda OpenCV


1. Quay lại tab trình duyệt của bạn khi Cloud9 đang chạy. 

2. Trong  terminal , hãy nhập lệnh sau để tải xuống mã cho lớp:

```
mkdir ~/environment/lambda-layer
cd ~/environment/lambda-layer
wget https://innovator-island.s3-us-west-2.amazonaws.com/opencv-python-37.zip
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00063.png?featherlight=false&width=90pc)

3. Tải gói mã nén lên nhóm triển khai S3 của bạn:

```
aws s3 cp opencv-python-37.zip s3://$s3_deploy_bucket
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00064.png?featherlight=false&width=90pc)

4. Tạo lớp Lambda:

```
aws lambda publish-layer-version --layer-name python-opencv-37 --description "OpenCV for Python 3.7" --content S3Bucket=$s3_deploy_bucket,S3Key=opencv-python-37.zip --compatible-runtimes python3.7
```

Sau một vài giây, phản hồi JSON trong terminal  xác nhận **LayerArn** và **Version** của lớp mới.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00065.png?featherlight=false&width=90pc)

#### Tạo hàm Chromakey Lambda

1.  Đi tới bảng điều khiển Lambda - từ Bảng điều khiển quản lý AWS, chọn **Service** rồi chọn **Lambda** . Đảm bảo rằng Region của bạn là chính xác.


    + Chọn **Create function**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00066.png?featherlight=false&width=90pc)

2. Trong trang **Create function**

   + Chọn **Author from scratch** 
   + Nhập **theme-park-photos-chromakey** cho **Function name**.
   + Đảm bảo Python 3.7 được chọn trong **Runtime**.
   + Đối với **Architecture** , hãy chọn **x86_64**.
   + Chọn mở rộng **Change default execution role**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00067.png?featherlight=false&width=90pc)

3. Trong phần **Change default execution role**.

   + Chọn **Use an existing role**
   + Chọn **theme-park-backend-ThemeParkLambdaRole-XXX**
   + Chọn **Create function**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00068.png?featherlight=false&width=90pc)

4. Trong trang tổng quan về **Lambda function** vừa tạo

+ Chọn **+ Add Trigger**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00069.png?featherlight=false&width=90pc)

5. Trong **Trigger configuration**

   + Chọn **S3**
   + Đối với **Bucket**, chọn **theme-park-backend-uploadbucket-xxx**
   + Đối với **Event Type** chọn  **All  object  create events**
   + Chọn **I acknowledge...**
   + Chọn **Add**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00070.png?featherlight=false&width=90pc)

6. Trở lại giao diện Lambda function,

   + Chọn **Code** tab
   +  Kéo xuống và dừng **Layers card**
   +  Chọn **Add a layer**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00071.png?featherlight=false&width=90pc)

7. Trong trang **Add layer**

   + Chọn **Custom layers**
   + Trong **Custom layers**, chọn **python-opencv-37**
   + Đối với **Version**, chọn **version hiện tại**
   + Chọn **Add**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00072.png?featherlight=false&width=90pc)

8. Quay lại **Lambda function**

+ Chọn **Code** tab.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00073.png?featherlight=false&width=90pc)

9. Quay trở lại **Cloud9**

+ Chọn **3-photos/1-chromakey/app.py**
+ Sao chép code của **app.py**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00074.png?featherlight=false&width=90pc)

10. Dán code của **app.py** vào **lambda_function.py**

+ Lưu lại
+ Chọn **Deploy**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00075.png?featherlight=false&width=90pc)

#### Thêm các biến môi trường

Hàm này sử dụng ba biến môi trường:

  + **OUTPUT_BUCKET_NAME**: tên của bucket chứa  object đầu ra.
  + **HSV_LOWER**: Một bộ giá trị đại diện cho giá trị HSV thấp hơn cho quá trình đối sánh chroma key màn hình xanh lục.
  + **HSV_UPPER**: Một bộ giá trị đại diện cho giá trị HSV cao hơn cho quá trình đối sánh chroma key màn hình xanh lục.

Trong phần này, bạn sẽ truy xuất và cấu hình các Biến Môi trường này cho hàm.

1. Trong trang **Lambda function**s

   + Chọn **Configuration** tab
   + Chọn **Environment variables**
   + Chọn **Edit**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00076.png?featherlight=false&width=90pc)

2. Quay lại giao diện **Cloud9  Terminal**

3. Trong  terminal , hãy nhập lệnh sau để truy xuất giá trị cho **OUTPUT_BUCKET_NAME**

```
aws s3 ls | grep theme-park-backend-processingbucket
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00077.png?featherlight=false&width=90pc)

4. Nhập ba biến môi trường với ba giá trị, như sau:

   + **OUTPUT_BUCKET_NAME**: giá trị từ bước 3 ở trên.
   + **HSV_LOWER**: [36, 100, 100]
   + **HSV_UPPER**: [75, 255, 255]
   + Chọn **Save**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00078.png?featherlight=false&width=90pc)

#### Thay đổi cài đặt cho Lambda Function

1. Trong tab trình duyệt có **theme-park-photos-chromakey** Lambda  function đang mở

+ Chọn **Configuration tab**
+ Chọn **General configuration**
+ Chọn **Edit**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00079.png?featherlight=false&width=90pc)

2. Trong giao diện **General configuration**

   + Thay đổi giá trị **Memory(MB)** lên **3008 MB**
   + Thay đổi giá trị **Timeout** từ **0 lên 10 sec**
   + Chọn **Save**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00080.png?featherlight=false&width=90pc)

3. Hoàn thành thay đổi cài đặt **Lambda function**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00081.png?featherlight=false&width=90pc)

Quá trình **chroma key** sử dụng các thư viện sử dụng nhiều bộ nhớ để hoàn tất quá trình xử lý đồ họa. Bằng cách phân bổ bộ nhớ tối đa, function này sẽ hoàn thành quá trình xử lý nhanh hơn.

#### Kiểm tra function

Bây giờ bạn sẽ kiểm tra function bằng cách sử dụng hình ảnh thử nghiệm có chứa ảnh của một người trên nền màu xanh lá cây. Bạn sẽ sao chép thủ công hình ảnh này vào **Upload bucket** và xem kết quả trong **Processing bucket**.

1. Quay lại tab trình duyệt của bạn khi Cloud9 đang chạy.

2. Điều hướng đến tệp **theme-park-backend\3-photos\green-screen-test.png** và mở. Bạn có thể xem ảnh của một người với màn hình màu xanh lá cây. Đây là hình ảnh thử nghiệm  local  .

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00082.png?featherlight=false&width=90pc)

3. Trong  terminal , nhập lệnh sau để thay đổi thư mục:

```
cd ~/environment/sampleapp/theme-park-backend/3-photos/
```

4. Tìm tên của upload bucket S3 của bạn bằng lệnh sau:

```
aws s3 ls | grep uploadbucket
```

5. Sao chép hình ảnh thử nghiệm  local   vào upload bucket của bạn, thay thế **youruploadbucket** thông số nhóm bằng tên nhóm của bạn từ bước 4:

```
aws s3 cp ./green-screen-test.png s3://youruploadbucketname
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00083.png?featherlight=false&width=90pc)

6. Trong một tab trình duyệt khác, hãy mở bảng điều khiển S3 của AWS Console.

   + Chọn **theme-park-backend-processingbucket**.
   + Chọn **green-screen-test.png**
   + Chọn **Download**
   + Lưu tệp  local   và mở trong trình xem ảnh.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00084.png?featherlight=false&width=90pc)

7. Bạn sẽ thấy hình ảnh màn hình màu xanh lá cây ban đầu đã được sửa đổi hiển thị người có nền màu xanh lá cây hiện đã bị loại bỏ. Hàm Lambda đã được gọi khi ảnh được tải lên S3 bucket. Hàm đã chạy một quá trình chromakey bằng cách sử dụng một thư viện được nhập bằng Lambda Layer để loại bỏ màn hình màu xanh lục và sau đó ghi hình ảnh kết quả vào một S3 bucket khác.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00085.png?featherlight=false&width=30pc)
