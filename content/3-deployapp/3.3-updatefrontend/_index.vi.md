---
title : "Cập nhật Frontend"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3.3 </b> "
---

#### Cập nhật cấu hình Frontend
Trong phần này, bạn sẽ thêm API endpoint mà bạn đã tạo vào cấu hình frontend. Điều này cho phép ứng dụng frontend nhận danh sách các chuyến đi và điểm tham quan thông qua URL APIGateway lấy thông tin từ DynamoDB.

Sau khi cập nhật, bạn sẽ thực hiện commit các thay đổi đối với git repo, repo này sẽ tự động triển khai lại và publish lại ứng dụng.

1. Quay trở lại giao diện Cloud9, tại menu bên trái, điều hướng tới thư mục **theme-park-frontend/src/**
    + Double click vào file **config.js**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00036.png?featherlight=false&width=90pc)

2. Thay thế cấu hình tại dòng 13, điền thêm thông tin API Gateway Endpoint mà chúng ta đã ghi chú trước đó. 

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00037.png?featherlight=false&width=90pc)

{{% notice tip %}}
Chúng ta có thể lấy endpoint URL bằng cách chạy lệnh dưới: \
**aws cloudformation describe-stacks --stack-name theme-park-backend --query "Stacks[0].Outputs[?OutputKey=='InitStateApi'].OutputValue" --output text**
{{% /notice %}}

+ Ấn tổ hợp phím **Ctrl + S** để lưu thay đổi.

3. Thực hiện commit code lên CodeCommit.
```
cd ~/environment/sampleapp/theme-park-frontend/
git commit -am "Module 1"
git push
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00038.png?featherlight=false&width=90pc)

4. Tại trang **All app**, click vào **theme-park-frontend**.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/00039.png?featherlight=false&width=90pc)

5. Bạn sẽ thấy một bản build mới đã tự động bắt đầu do kết quả của việc chúng ta commit code trong repo. Quá trình build này sẽ mất vài phút.

6. Click vào đường link publish để mở ứng dụng của chúng ta.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/00040.png?featherlight=false&width=90pc)

7. Giờ đây, bạn có thể thấy bản đồ chứa các điểm yêu thích của công viên giải trí như trò chơi và điểm tham quan. Bạn có thể chọn bất kỳ trong số chúng và tìm hiểu thêm.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00041.png?featherlight=false&width=90pc)

{{% notice tip %}}
Nếu trình duyệt cache lại trang cũ, bạn có thể ấn phím F5 để thực hiện refresh (Windows) hoặc ⌘ Cmd và ⇧ Shift key sau đó nhấn R (Mac) để thực hiện hard refresh.
{{% /notice %}}

Trong phần này, chúng ta đã:

+ Tạo code repository trong Cloud9 và cấu hình Amplify Console để publish ứng dụng web trong repository này. Bây giờ bạn có một public URL endpoint cho ứng dụng của mình.
+ Triển khai cơ sở hạ tầng backend cho công viên chủ đề và ứng dụng.
+ Đã đưa dữ liệu vào một bảng DynamoDB chứa thông tin về chuyến đi và điểm thu hút cho công viên.
+ Kiểm tra việc triển khai bằng cách sử dụng CLI để quét bảng DynamoDB và sử dụng trình duyệt để kiểm tra API endpoint Gateway.
+ Thực hiện cập nhật frontend với API endpoint mới và xem kết quả trong ứng dụng.
+ Các thay đổi về code được push (dưới dạng cập nhật cấu hình) đối với CodeCommit và Amplify Console tự động phát hiện commit mới và publish các thay đổi lên frontend.

Trong phần tiếp theo, bạn sẽ thêm khả năng quản lý thời gian chờ theo thời gian thực cho các chuyến đi.