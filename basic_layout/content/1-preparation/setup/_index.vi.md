---
title : "Cài đặt môi trường"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 1.2.</b> "
---

Để bắt đầu, hãy thiết lập môi trường được cung cấp bằng cách làm theo các bước dưới đây:

1. Tải repos của workshop
    ````
    git clone https://github.com/aws-samples/cloudscape-design-system-workshop.git

    ````
    ![setup](/images/1.png?featherlight=false&width=50pc)
2. Truy cập đến thư mục của workshop
    ````
    cd cloudscape-design-system-workshop
    ````
3. Cài đặt các thành phần phụ thuộc:
    ```
    npm install
    ```
    ![setup](/images/2.png?featherlight=false&width=50pc)
4. Sau khi đã hoàn thành các bước trên, bạn đã sẵn sàng tiến hành bước đầu tiên của workshop.

#### Giới thiệu về kho lưu trữ của workshop
All steps towards the final application are available as branches on the repository. If you have local changes, stash them by using git stash or force the check out by adding the -f option to the checkout command.
  - Step 1: Basic layout → Starting the step: ```git checkout step-1```. Get the completed step: ```git checkout - step-1-completed```
  - Step 2: Table view → Starting the step: ```git checkout step-2```. Get the completed step: ```git checkout step-2-completed```
  - Step 3: Creation flow → Starting the step: ```git checkout step-3```. Get the completed step: ```git checkout step-3-completed```

Tất cả các bước hướng để xây dựng ứng dụng cuối cùng đều có sẵn dưới dạng các nhánh trong kho lưu trữ. Nếu bạn có các thay đổi, hãy lưu trữ chúng bằng cách sử dụng git stash hoặc kiểm tra bằng cách thêm tùy chọn -f vào lệnh kiểm tra.
   - Bước 1: Bố cục cơ bản → Bắt đầu bước: ```gitcheck step-1```. Đã hoàn thành: ```gitcheck - step-1-completed```
   - Bước 2: Table view → Bắt đầu bước: ```gitcheck step-2```. Đã hoàn thành: ``` git kiểm tra bước 2-hoàn thành```
   - Bước 3: Luồng tạo → Bắt đầu bước: ```gitcheck step-3```. Đã hoàn thành: ``` git kiểm tra bước 3-hoàn thành```
Để xem ứng dụng sau khi hoàn thành, hãy check out main brach
#### Dọn dẹp tài nguyên
Nếu bạn muốn dọn dẹp các tệp được tạo trong hội thảo, hãy điều hướng đến thư mục kho lưu trữ hội thảo và xóa nó.