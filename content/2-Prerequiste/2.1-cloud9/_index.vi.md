---
title : "Tạo Cloud9 instance"
date: 2024-01-01
weight : 1 
chapter : false
pre : " <b> 2.1 </b> "
---

#### Tạo Cloud9 instance


1. Truy cập vào [giao diện quản trị dịch vụ Cloud9](https://console.aws.amazon.com/cloud9/home)
    + Chuyển **Region** về **Region** bạn sử dụng cho bài thực hành.
    + Click **Create environment**.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/0001.png?featherlight=false&width=90pc)

2. Tại mục Name, điền **serverless-development**.

    + Click **Next step**.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/0002.png?featherlight=false&width=90pc)

3. Tại trang **Configure Settings**.

    + Click chọn **t3.small**.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/0003.png?featherlight=false&width=90pc)

4. Chọn **Next step**.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/0004.png?featherlight=false&width=90pc)

5. Kiểm tra lại và chọn **Create environment**

![Cloud9](/images/2-Prerequiste/2.1-cloud9/0005.png?featherlight=false&width=90pc)

6. Quá trình tạo Cloud9 instance sẽ mất một vài phút.

- Click vào close Welcome tab.

![Cloud9](/images/2-Prerequiste/2.1-cloud9/0006.png?featherlight=false&width=90pc)

7. Click **+**, sau đó click **New  Terminal ** để mở giao diện  terminal  mới.


![Cloud9](/images/2-Prerequiste/2.1-cloud9/0007.png?featherlight=false&width=90pc)


8. Tiến hành cài **JQ** để hỗ trợ format **JSON** bằng câu lệnh dưới đây.

```
sudo yum install jq -y
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/0008.png?featherlight=false&width=90pc)

{{% notice warning %}}
Đảm bảo bạn đã cài JQ thành công trước khi thực hiện các bước tiếp theo.
{{% /notice %}}

9. Copy câu lệnh dưới đây và dán vào giao diện ** terminal **.

    + Câu lệnh này nhằm tạo thư mục chứa **sampleapp**.
    + Thay đổi đường dẫn vào thư mục.

```
mkdir sampleapp
cd sampleapp
```

![Cloud9](/images/2-Prerequiste/2.1-cloud9/0009.png?featherlight=false&width=90pc)

Tiếp theo chúng ta sẽ tiến hành tạo CodeCommit repository.




