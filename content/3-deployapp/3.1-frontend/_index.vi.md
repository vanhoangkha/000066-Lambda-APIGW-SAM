---
title : "Triển khai Frontend"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1 </b> "
---

#### Clone code base

1. Trở lại giao diện  terminal  của **Cloud9**.

2. Copy các câu lệnh dưới đây và dán vào giao diện **terminal**.

```
git config --global credential.helper '!aws codecommit credential-helper $@'
git config --global credential.UseHttpPath true
```

   + Để lưu thông tin đăng nhập.
   + Sử dụng cho các kho lưu trữ khác nhau nên định cấu hình thông tin đăng nhập để sử dụng đường dẫn kho lưu trữ đầy đủ.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00013.png?featherlight=false&width=90pc)

3. Thực hiện tạo thư mục để chứa **source code front-end**.

```
mkdir ~/environment/sampleapp/theme-park-frontend
cd ~/environment/sampleapp/theme-park-frontend
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00014.png?featherlight=false&width=90pc)
4. Chạy các câu lệnh dưới đây để download source code từ **GitHub** và copy vào thư mục con.

   + Sau đó, thực hiện giải nén bằng **unzip**

```
wget --no-check-certificate https://github.com/First-Cloud-Journey/000066-serverlesssample/raw/8b8f82165b55afcd4e4ccb2066511d1ab425b646/theme-park-frontend-202110.zip
unzip theme-park-frontend-202110.zip
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00015.png?featherlight=false&width=90pc)

5. Chạy câu lệnh sau để cấu hình **local Git repository và commit**.

```
git init -b main
git add .
git commit -am "First commit"
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00016.png?featherlight=false&width=90pc)

6. Push code lên **CodeCommit repository** chúng ta đã tạo cho phần Frontend.

```
AWS_REGION=$(curl -s http://169.254.169.254/latest/meta-data/placement/availability-zone | sed 's/\(.*\)[a-z]/\1/')
git push --set-upstream https://git-codecommit.$AWS_REGION.amazonaws.com/v1/repos/theme-park-frontend main
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00017.png?featherlight=false&width=90pc)

#### Triển khai với Amplify
1. Truy cập vào giao diện quản trị dịch vụ **Amplify**

2. Chuyển Region về **Region** bạn sử dụng cho bài thực hành.
   + Kéo chuột xuống phía dưới, click **Get started** tại mục **Host your web app**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00018.png?featherlight=false&width=90pc)

3. Click chọn **AWS CodeCommit**.
   + Click **Continue**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00019.png?featherlight=false&width=90pc)

4. Tại trang **Add repository branch**.
   + Click chọn **repository theme-park-frontend**.
   + Tại mục **Branch**, click chọn **main**.
   + Click **Next**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00020.png?featherlight=false&width=90pc)

5. Tại trang **Configure build settings**, giữ tuỳ chọn mặc định và click **Next**.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/00021.png?featherlight=false&width=90pc)

6. Tại trang **Review** kiểm tra lại cấu hình và click **Save and deploy**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00022.png?featherlight=false&width=90pc)

7. Khi quá trình xây dựng đã hoàn thành giai đoạn **Verify**, hãy chọn mở liên kết được cung cấp cho ứng dụng của bạn ở một tab mới.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00023.png?featherlight=false&width=90pc)

8. Bạn sẽ thấy bản đồ công viên trống với thanh điều hướng.chúng ta sẽ bổ sung các tính năng đó tiếp theo.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/00024.png?featherlight=false&width=90pc)

9. Sao chép **URL đã publish của ứng dụng web** vào file ghi chú của bạn. Bạn sẽ cần thông tin này trong các bước tiếp theo.
