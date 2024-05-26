---
title : "Add content to the app layout component"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b>2.5.</b> "
---
Adding content to the app layout component is done via the **content** property. Try it out by adding the property to **AppLayou**t and add dummy content to see the basic functionality.

To place your content in the high contrast header, you can use the [content layout component](https://cloudscape.design/components/content-layout/?tabId=playground) . This component provides the basic layout for the header and content of a page. Take a look at the [API documentation and usage guidelines](https://cloudscape.design/components/content-layout/?tabId=playground). You can then add the component to the **AppLayout** content property with dummy data and see how it looks.

Việc thêm content vào app layout được thực hiện thông qua thuộc tính **content**. Thử nó bằng cách thêm thuộc tính vào **AppLayout** và thêm nội dung nháp để xem chức năng cơ bản.

Để đặt nội dung của bạn trong tiêu đề độ tương phản cao, bạn có thể sử dụng [content layout component](https://cloudscape.design/components/content-layout/?tabId=playground). Thành phần này cung cấp bố trí cơ bản cho tiêu đề và nội dung của một trang. Hãy xem [Documentation API and usage guidelines](https://cloudscape.design/components/content-layout/?tabId=playground). Sau đó bạn có thể thêm thành phần vào thuộc tính nội dung **AppLayout** với dữ liệu ngớ ngẩn và xem nó trông như thế nào.
{{%expand "Xem src/pages/home/app.tsx trông như thế nào sau bước này." %}}
```
import React from 'react';
import TopNavigation from '@cloudscape-design/components/top-navigation';
import AppLayout from '@cloudscape-design/components/app-layout';
import Navigation from './components/navigation';
import Breadcrumbs from './components/breadcrumbs';
import ContentLayout from '@cloudscape-design/components/content-layout';
import Container from '@cloudscape-design/components/container';
import Header from '@cloudscape-design/components/header';

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
        breadcrumbs={<Breadcrumbs />}
        content={
          <ContentLayout header={<Header variant="h1">Dashboard</Header>}>
            <Container>Hello World!</Container>
          </ContentLayout>
        }
      />
    </>
  );
}
```
{{% /expand%}}

{{%expand "Xem ảnh chụp màn hình của trang trông như thế nào sau bước này." %}}
![Preparation](/images/8.png?false&width=90pc)
{{% /expand%}}
