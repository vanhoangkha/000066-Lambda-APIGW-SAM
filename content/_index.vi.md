---
title : "Serverless với Lambda, API Gateway và SAM"
date: 2024-01-01
weight : 1
chapter : false
---

# Serverless với Lambda, API Gateway và SAM

#### Giới thiệu

Công viên giải trí mới thú vị này, được xây dựng trên hòn đảo Isla Sanserver hẻo lánh, kết hợp tàu lượn siêu tốc và cưỡi ngựa với các buổi biểu diễn và triển lãm. Công viên sẽ mở cửa hàng ngày và dự kiến ​​đón 50.000 lượt khách mỗi ngày. Nó tự hướng dẫn, sử dụng một ứng dụng web mà khách có thể duyệt trên điện thoại thông minh của họ.

Vấn đề "nhỏ" duy nhất là nhóm phát triển đã đột ngột rời đi và ngày Khai mạc công viên là hôm nay! Bạn chỉ có hàng giờ để hoàn thành việc lắp ráp các phần còn lại của ứng dụng trước khi cánh cổng mở ra. Nhưng đừng lo lắng, serverless luôn sẵn sàng! Các hướng dẫn này sẽ hướng dẫn bạn cách sử dụng các dịch vụ AWS để tập hợp một ứng dụng hoàn chỉnh để bạn có thể tiết kiệm trong ngày.

#### Cấu trúc ứng dụng
Bạn sẽ sử dụng phương pháp tiếp cận micro-services để định cấu hình **front-end** và xây dựng **backend serverless application**.

#### Frontend
Ứng dụng web front-end bao gồm một ứng dụng web JavaScript hiện có được quản lý bằng giao diện **AWS Amplify Console**  với các dịch vụ trên backend. Bạn sẽ chỉ cần thực hiện các thay đổi nhỏ đối với tệp cấu hình trong mã front-end để hoàn thành hội thảo này.

**Amplify Console** cung cấp quy trình làm việc đơn giản, dựa trên **Git** để triển khai và lưu trữ các ứng dụng **web fullstack serverless**. **Amplify Console** có thể tạo cả **frontend và backend** nhưng đối với hội thảo này, chúng tôi sẽ sử dụng **Amplify Console** chỉ cho **frontend**.

**Amplify** sẽ được sử dụng để lưu trữ các tài nguyên web tĩnh bao gồm **HTML, CSS, JavaScript** và các tệp hình ảnh được tải trong trình duyệt của người dùng qua **S3**.

#### Backend
Kiến trúc ứng dụng backend sử dụng **AWS Lambda, Amazon API Gateway, Amazon S3, Amazon DynamoDB và Amazon Cognito**.

**JavaScript** được thực thi trong ứng dụng trình duyệt front-end gửi và nhận dữ liệu từ **API** backend public được xây dựng bằng **API Gateway** và **Lambda**. **DynamoDB** cung cấp lớp lưu trữ dữ liệu liên tục được sử dụng bởi các hàm Lambda của API.

Xem sơ đồ bên dưới để biết kiến ​​trúc hoàn chỉnh.

![Serverless](/images/1-Introduce/architecture-light.png?featherlight=false&width=90pc)

#### Nội dung

1. [Giới thiệu](1-introduce/)
2. [Chuẩn bị](2-prerequiste/)
3. [Triển khai ứng dụng mẫu](3-deployapp/)
4. [Triển khai ride times system](4-realtime-ridewaittimes/)
5. [Xử lý ảnh trên xe](5-on-ridesphotoprocessing/)
6. [Dọn dẹp tài nguyên](6-cleanup/)