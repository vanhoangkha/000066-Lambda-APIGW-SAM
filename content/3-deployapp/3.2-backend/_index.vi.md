---
title : "Triển khai Backend"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 3.2 </b> "
---

#### Triển khai Backend

Trong phần này chúng ta sẽ tiến hành triển khai phần backend cho ứng dụng mẫu của chúng ta.

Frontend cần hiển thị chi tiết về các chuyến đi và điểm tham quan trong công viên để hữu ích cho khách tham quan. Khi bạn đã xây dựng backend trong phần này, khách của chúng ta sẽ có thể thấy nhiều thông tin hữu ích hơn trong ứng dụng.

#### Kiến trúc backend

![Serverless](/images/3-deployapp/3.2-backend/module1-backend-arch.png?featherlight=false&width=60pc)

Kiến trúc backend của chúng ta bao gồm:
+ Một bảng DynamoDB mà bạn sẽ điền thông tin về tất cả các chuyến đi và điểm tham quan trong công viên.
+ Một hàm Lambda thực hiện quét bảng trên DynamoDB để trả về tất cả các mục.
+ Một API Gateway API tạo một điểm cuối http công khai để ứng dụng front-end truy vấn. Điều này gọi hàm Lambda để trả về danh sách các chuyến đi và điểm tham quan.

Sau khi chúng ta xây dựng các tài nguyên backend cần thiết, chúng ta  sẽ cập nhật cấu hình ứng dụng front-end để truy vấn API Gateway endpoint trong phần tiếp theo.

#### Nội dung
- [Triển khai Infrastructure](3.2.1-infastructure/)
- [Điền thông tin chuyến đi](3.2.2-populatedata/)
- [Kiểm tra cấu hình](3.2.3-testconfig)
