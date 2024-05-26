---
title : "Kiểm tra chế độ tối và nhỏ gọn"
date :  "`r Sys.Date()`" 
weight : 9 
chapter : false
pre : " <b>2.9.</b> "
---
Cloudscape hỗ trợ:
- Chế độ hình ảnh được sử dụng để tối ưu hóa giao diện người dùng dựa trên điều kiện môi trường và sở thích của người dùng. Chúng tôi hỗ trợ chế độ sáng và tối.
- Chế độ mật độ được sử dụng để tối ưu hóa mật độ dữ liệu. Chúng tôi hỗ trợ chế độ thoải mái và nhỏ gọn.

Mở các công cụ phát triển được xây dựng trong trình duyệt của bạn và: 
- Chuyển sang chế độ tối và ánh sáng bằng cách thực hiện lệnh sau đây:
    ```
    document.body.classList.toggle('awsui-dark-mode')

    ```
- Chuyển đổi chế độ nhỏ gọn và thoải mái bằng cách thực hiện lệnh sau đây:
    ```
    document.body.classList.toggle('awsui-compact-mode')

    ```
{{%expand "Xem ảnh chụp màn hình của trang trông như thế nào trong các chế độ khác nhau." %}}
![Preparation](/images/10.png?false&width=90pc)
{{% /expand%}}
