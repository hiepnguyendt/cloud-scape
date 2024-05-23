---
title : "Add the breadcrumb component to the app layout"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b>2.4.</b> "
---
Similar to how we added the side navigation component to app layout, we're now creating a breadcrumb group component and wiring it up to the app layout component.

Take a look at the guidance and examples for [breadcrumb group](https://cloudscape.design/components/breadcrumb-group/) . Add the breadcrumb group component on your own using dummy data. Create the component in a separate file (```src/pages/home/components/breadcrumbs.tsx```) and attach the component to the app layout's ```breadcrumbs``` slot.

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

{{%expand "See how src/pages/home/app.tsx looks like after this step." %}}
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

{{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/7.png?false&width=90pc)
{{% /expand%}}
