---
title : "Triển khai ứng dụng mẫu"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3. </b> "
---

#### Triển khai ứng dụng mẫu

Ứng dụng sample chúng ta sẽ triển khai là ứng dụng công viên chủ đề bao gồm frontend và phần backend. Trong mô-đun này, bạn sẽ thiết lập frontend và sau đó kết nối nó với backend.

Frontend là một Progressive Web App (PWA) được phát triển bằng Vue.js.

Mã nguồn frontend được cung cấp sẵn vì vậy bạn chỉ cần tải mã xuống và triển khai vào kho lưu trữ của riêng bạn trong CodeCommit. Sử dụng AWS Amplify Console, bạn sẽ tạo một quy trình triển khai liên tục để publish ứng dụng của mình.

Phần backend là một tập hợp các dịch vụ nhỏ serverless. Trong phần này, chúng ta sẽ thiết lập như sau:

+ Một bảng DynamoDB chứa thông tin về tất cả các chuyến đi và điểm tham quan trong công viên.
+ Một hàm Lambda thực hiện quét bảng trên DynamoDB để trả về tất cả các mục.
+ Một API Gateway API tạo một public http endpoint để ứng dụng frontend truy vấn. Điều này gọi hàm Lambda để trả về danh sách các chuyến đi và điểm tham quan.

Khi bạn đã xây dựng các tài nguyên back-end cần thiết, bạn sẽ cập nhật cấu hình ứng dụng front-end để truy vấn API endpoint Gateway và hiển thị thông tin về tất cả các chuyến đi và điểm tham quan.

#### Nội dung
- [Triển khai Frontend](3.1-frontend/)
- [Triển khai Backend](3.2-backend/)
- [Cập nhật Frontend](3.3-updatefrontend)

