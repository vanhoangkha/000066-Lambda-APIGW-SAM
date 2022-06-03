---
title : "Triển khai Frontend"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

#### Triển khai Frontend

Để kích hoạt front-end listen IoT topic cho các thông báo về thời gian đi xe, bạn sẽ cần hai giá trị sau:

+ IoT endpoint hostname
+ The Cognito Identity Pool ID.

Trong phần này, bạn sẽ truy xuất các giá trị này và cập nhật cấu hình front-end. Sau khi thực hiện thay đổi, bạn sẽ cam kết thay đổi vào git repo, repo này sẽ tự động publish lại ứng dụng bằng cách sử dụng **Amplify Console**.

1. Quay lại tab trình duyệt của bạn khi Cloud9 đang chạy. 
2. Chạy lệnh này để hiển thị **Id IdentityPool**:

```
aws cognito-identity list-identity-pools --max-results 10
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00055.png?featherlight=false&width=90pc)

3. Chạy lệnh để hiện thị **IoT endpoint hostname**

```
aws iot describe-endpoint --endpoint-type iot:Data-ATS
```
   + Chạy lệnh để hiện thị AWS REGION

```
echo $AWS_REGION
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00056.png?featherlight=false&width=90pc)

4. Trong  Terminal  Cloud9, trong bảng điều khiển thư mục bên trái, hãy điều hướng đến **theme-park-frontend/src**.

+ Tìm tệp **config.js** và nhấp đúp để mở trong trình chỉnh sửa.
+ Tệp này chứa cấu hình **JSON** cho front-end. Tệp được tách thành các mô-đun tương ứng.
+ Trong phần MODULE 2 ở đầu tệp, hãy cập nhật các thuộc tính iot như sau:
  + **poolId**: đặt thành Id IdentityPool từ phần trên.
  + **host**: đặt thành ioT endpoint hostname từ phần trên.
  + **region**: đặt thành AWS_REGION của bạn từ phần trên (ví dụ 'ap-southeast-1').
+ Sử dụng **Ctrl + S** lưu tệp tin lại.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00057.png?featherlight=false&width=90pc)

#### Push CodeCommit và triển khai qua Amplify. 

1. Trong Cloud9  Terminal , thay đổi thành thư mục front-end bằng lệnh sau:

```
cd ~/environment/sampleapp/theme-park-frontend/
```

2. Commit **CodeCommit** bằng cách thực hiện các lệnh sau:


```
git commit -am "Module 2"
git push
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00058.png?featherlight=false&width=90pc)

3. Sau khi hoàn thành commit, chuyển đến [Amplify Console](https://console.aws.amazon.com/amplify/) . Đảm bảo **Region** chính xác.

+ Trong trang **All apps**, Chọn **theme-park-frontend**


![Cloud9](/images/2-Prerequiste/2.1-cloud9/00059.png?featherlight=false&width=90pc)

4. Bạn sẽ thấy một bản dựng mới đã tự động bắt đầu do kết quả của commit mới trong repo mã cơ bản. Quá trình xây dựng này sẽ mất vài phút.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00060.png?featherlight=false&width=90pc)

5. Mở URL ứng dụng đã publish trong trình duyệt.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00061.png?featherlight=false&width=90pc)

Giờ đây, bạn có thể thấy các chuyến đi và điểm tham quan trên bản đồ công viên giải trí hiển thị thời gian chờ theo thời gian thực và cập nhật mỗi phút.



