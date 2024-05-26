---
title : " Bước chuẩn bị"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> </b> "
---
Trước khi chúng ta bắt đầu, hãy chuẩn bị môi trường của chúng ta cho bước hội thảo này.
1. Đảm bảo bạn đang ở trong thư mục cloudscape-design-system-workshop.
2. Nếu bạn đã có server đang chạy, hãy dừng lệnh đó trong terminal của bạn.
3. Kiểm tra step-1 branch bằng cách thực hiện:
    ```
    git checkout step-1
    ```

4. Run server lên bằng cách chạy:
    ```
        npm run dev
    ```

5. Verify port 8080.
Kiểm tra đầu ra trong thiết bị đầu cuối của bạn để kiểm tra xem server đang chạy trên cổng 8080. Đầu ra phải đề cập đến cổng 8080 như sau:

    ```

      VITE v3.2.3  ready in 544 ms
      ➜  Local:   http://localhost:8080/
      ➜  Network: use --host to expose
    
    ```
{{% notice note %}}
** Khắc phục sự cố: 'Cổng 8080 đã được sử dụng'.**\
Nếu đầu ra cho biết ``Cổng 8080 đang được sử dụng, hãy thử một cổng khác...`` thì bạn cần phải đóng lệnh sử dụng cổng này. Điều này có thể xảy ra khi server dev dừng lại và cổng không tự động đóng. Để đóng cổng được sử dụng, trước tiên dừng lệnh máy chủ dev tiếp theo bằng cách đóng quá trình chạy trên cổng 8080 bằng cách thực hiện:\
    ```if [[ $(lsof -t -i:8080) ]]; then kill -9 $(sudo lsof -t -i:8080); fi```\
Bây giờ, hãy chạy lại server và kiểm tra port 8080.
{{% /notice %}}

6. Mở trang `/home/index.html` mà chúng ta đang làm việc trong trình duyệt. Các hướng dẫn khác nhau tùy thuộc vào môi trường mà bạn đang chạy.
![Preparation](/images/3.png?false&width=90pc)