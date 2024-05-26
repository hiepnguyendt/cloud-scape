---
title : "Thêm side navigation component vào app layout"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b>2.3.</b> "
---
Tiếp theo, chúng ta sẽ mở rộn app layout với [side navigation component](https://cloudscape.design/components/side-navigation/?tabId=playground). Side navigation sẽ được hiển thị bên trong navigation của app layout. Nó chứa danh sách các liên kết điều hướng trỏ đến các trang trong ứng dụng.

Hãy xem hướng dẫn và ví dụ về [side navigation](https://cloudscape.design/comComponents/side-navigation/?tabId=playground) và [app layout](https://cloudscape.design/comComponents/app-layout/?tabId=api). Tạo thành phần trong một tệp riêng biệt (```src/pages/home/comComponents/navigation.tsx```) và cung cấp thành phần đó cho vùng điều hướng của bố cục ứng dụng.

Dưới đây là các bước để tạo side navigation bên trong một tệp riêng biệt:

1. Mở ```src/pages/home/comComponents/navigation.tsx``` và nhập thành phần bằng cách sử dụng ```import SideNavigation, { SideNavigationProps } from '@cloudscape-design/comComponents/side-navigation' ;```.
2. Thêm <SideNavigation /> bên trong thành phần <Navigation /> và kết nối nó với các thuộc tính mà bạn muốn hiển thị trong side navigation:
3. Chỉ định href của liên kết hiện đang hoạt động, chúng tôi sử dụng tên đường dẫn hiện tại location.pathname.
4. Đặt thuộc tính header cho trang hiện tại ```header={{ href: '/home/index.html', text: 'Service' }}```

 ```src/pages/home/components/navigation.tsx``` sẽ trông như thế này:
    ```
    import React from 'react';
    import SideNavigation, { SideNavigationProps } from '@cloudscape-design/components/side-navigation';

    const items: SideNavigationProps['items'] = [
    // More pages got added as part of the workshop.
    { type: 'link', text: 'Dashboard', href: '/home/index.html' },
    ];

    export default function Navigation() {
    return (
        <>
        <SideNavigation
            activeHref={location.pathname}
            header={{ href: '/home/index.html', text: 'Service' }}
            items={items}
        />
        </>
    );
    }

    ```

Bây giờ hãy đính kèm thành phần đã tạo trước đó với app layout để hiển thị nó:
1. Mở file ```src/pages/home/app.tsx```.
2. Thêm ```<Navigation />``` với ```import Navigation from './components/navigation';```.
3. Thêm thành phần ```navigation={<Navigation />}``` vào thuộc tính navigation của thành phần ``<AppLayout />``:
{{%expand "See how src/pages/home/app.tsx looks like after this step." %}}

```
import React from 'react';
import TopNavigation from '@cloudscape-design/components/top-navigation';
import AppLayout from '@cloudscape-design/components/app-layout';
import Navigation from './components/navigation';

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
        navigation={<Navigation />}
      />
    </>
  );
}

```
{{% /expand%}}

{{%expand "Trang sẽ trông như thế này sau khi thực hiện bước này." %}}
![Preparation](/images/6.png?false&width=90pc)
{{% /expand%}}
