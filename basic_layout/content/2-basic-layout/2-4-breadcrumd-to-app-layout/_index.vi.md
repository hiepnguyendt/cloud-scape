---
title : "Thêm Breadcumb vào app layout của bạn"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b>2.4.</b> "
---
Tương tự như cách chúng tôi thêm side navigation vào app layput, bây giờ chúng tôi đang tạo breadcrumb và kết nối nó với app layout.

Hãy xem hướng dẫn và ví dụ cho [group breadcrumb](https://cloudscape.design/components/breadcromb-group/). Thêm breadcrumb bằng cách sử dụng dữ liệu dummy. Tạo thành phần trong một tập tin riêng biệt (``src/pages/home/components/breadcrumbs.tsx`) và thêm thành phần vào```breadcreumbs``` của app layout.
Here's what ```src/pages/home/components/breadcrumbs.tsx``` looks like:
```
import React from 'react';
import BreadcrumbGroup, { BreadcrumbGroupProps } from '@cloudscape-design/components/breadcrumb-group';

const items: BreadcrumbGroupProps.Item[] = [
  { text: 'Chocolate Factory', href: '/home/index.html' },
  { text: 'Dashboard', href: '/home/index.html' },
];

export interface BreadcrumbsProps {
  active: BreadcrumbGroupProps.Item;
}

export default function Breadcrumbs() {
  return <BreadcrumbGroup items={items} />;
}
```
Next, we provide the ```Breadcrumbs``` component to the app layout as we did it with the side navigation in the previous step.
Tiếp theo, chúng tôi cung cấp ```Breadcrumbs``` component cho app layout chúng tôi đã làm với side navigation ở bước trước.

{{%expand "Xem src/pages/home/app.tsx trông như thế nào sau bước này." %}}
```
import React from 'react';
import TopNavigation from '@cloudscape-design/components/top-navigation';
import AppLayout from '@cloudscape-design/components/app-layout';
import Navigation from './components/navigation';
import Breadcrumbs from './components/breadcrumbs';

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
        breadcrumbs={<Breadcrumbs />}
        navigation={<Navigation />}
      />
    </>
  );
}

```
{{% /expand%}}

{{%expand "Xem ảnh chụp màn hình của trang trông như thế nào sau bước này." %}}
![Preparation](/images/7.png?false&width=90pc)
{{% /expand%}}
