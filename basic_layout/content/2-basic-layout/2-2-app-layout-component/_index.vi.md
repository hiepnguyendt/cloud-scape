---
title : "Thêm app layout"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b>2.2.</b> "
---
Hãy tiếp tục thêm [app layout component](https://cloudscape.design/comComponents/app-layout/). Thành phần này cung cấp bố cục cơ bản cho tất cả các loại trang, bao gồm điều hướng bên có thể thu gọn và bảng công cụ.
1. Mở file ```src/pages/home/app.tsx```.
2. Thêm ```import AppLayout from '@cloudscape-design/components/app-layout';``` vào code.
3. Thêm ```<AppLayout />``` với tư cách là thành phần bên cạnh thành phần ```<TopNavigation />``` được gói trong [React fragment](https://reactjs.org/docs/fragments.html).
4. Đặt thuộc tính ```ariaLabels``` của AppLayout như được mô tả trong [API AppLayout docummentation](https://cloudscape.design/comComponents/app-layout/).
5.  Thành phần AppLayout có thể được sử dụng với các tiêu đề khác nhau. Nó cung cấp thuộc tính ```headerSelector``` để chỉ định bộ chọn CSS của tiêu đề bạn đang sử dụng. Việc đặt thuộc tính này rất quan trọng để đảm bảo rằng vị trí AppLayout không chồng lên tiêu đề.\
     a. Để bọc component <TopNavigation /> trong một phần tử <div />, bạn có thể làm như sau. Chúng ta thêm một thuộc tính id với giá trị "top-nav" vào phần tử <div> bao quanh component <TopNavigation />.
     b. Thiết lập thuộc tính ```headerSelector``` của AppLayout sử dụng css seclector ```#top-nav```.
6. Để tránh trùng lặp, hãy tạo TopNavigation sticky. Việc này được xử lý bên trong ``src/pages/home/styles.css``.\
    a. Chúng ta cần import file css vào component TopNavigation ```import './styles.css';```.\
    b. Thêm thiết kế cần thiết cho top navigation. Gợi ý: Bạn có thể tìm thấy ví dụ về kiểu này ở cuối [documentation page](https://cloudscape.design/comComponents/top-navigation/?tabId=api#code-examples).
    ```src/pages/home/app.tsx``` sẽ như thế này sau bước này:
    ```
    import React from 'react';
    import TopNavigation from '@cloudscape-design/components/top-navigation';
    import AppLayout from '@cloudscape-design/components/app-layout';

    import './styles.css';

    export default function App() {
    return (
        <>
        <div id="top-nav">
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
        </div>
        <AppLayout
            headerSelector="#top-nav"
            ariaLabels={{
            navigation: 'Navigation drawer',
            navigationClose: 'Close navigation drawer',
            navigationToggle: 'Open navigation drawer',
            notifications: 'Notifications',
            tools: 'Help panel',
            toolsClose: 'Close help panel',
            toolsToggle: 'Open help panel',
            }}
        />
        </>
    );
    }
    ```

 ```src/pages/home/styles.css``` sẽ như thế này sau bước này:
```
    #top-nav
    {
        position: sticky;
        top: 0;
        z-index: 1002;
    }

```

Bây giờ bạn sẽ có một trang trống bao gồm bố cục ứng dụng trống, top navigation và không có nội dung.


{{%expand "{{%expand "Trang sẽ trông như thế nào sau bước này." %}}" %}}
![Preparation](/images/5.png?false&width=90pc)

{{% /expand%}}