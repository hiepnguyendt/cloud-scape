---
title : "Top navigation component"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b>2.1.</b> "
---
Trước tiên, hãy bắt đầu xây dựng ứng dụng của chúng ta bằng cách thêm [top navigation component](https://cloudscape.design/comComponents/top-navigation/) . Top navigation chứa logo và có thể bao gồm các thành phần điều hướng bổ sung, tất cả những yếu tố này sẽ tồn tại trên tất cả các trang dịch vụ.
1. Mở file `src/pages/home/app.tsx`.
2. Thêm `import TopNavigation from '@cloudscape-design/components/top-navigation';`.
3. Thêm <TopNavigation /> vào `App`.
    ```
    import React from 'react';
    import TopNavigation from '@cloudscape-design/components/top-navigation';

    export default function App() {
    return (
        <TopNavigation
        identity={{
            logo: { src: '/logo.svg', alt: 'Chocolate Factory Logo' },
            title: 'Chocolate Factory',
            href: '/home/index.html',
        }}
        i18nStrings={{
            overflowMenuTriggerText: 'More',
            overflowMenuTitleText: 'All',
        }}
        />
    );
    }

    ```
    Truy cập [browser](http://localhost:8000/home/index.html)  để xem kết quả sau khi thêm top navigation component 🚀
    
{{%expand "Xem sự thay đổi của trang sau khi thực hiện bước này" %}}
![Preparation](/images/3.png?false&width=90pc)
{{% /expand%}}
