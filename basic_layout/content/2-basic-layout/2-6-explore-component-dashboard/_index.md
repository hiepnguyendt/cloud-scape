---
title : "Explore components for dashboard"
date :  "`r Sys.Date()`" 
weight : 6 
chapter : false
pre : " <b>2.6.</b> "
---
We've added predefined content for the dashboard page (we won't cover this in detail) inside ```src/pages/home/components/prepared-dashboard-content.tsx```. Import this component in the app by using ```import PreparedDashboardContent from './components/prepared-dashboard-content';``` and add the component as a child of the **ContentLayout** component. After that, take a look in your browser to explore the different components.
```
// AppLayout's content property:
content={
  <ContentLayout header={<Header variant="h1">Dashboard</Header>}>
    <PreparedDashboardContent />
  </ContentLayout>
}

```
{{%expand "See how src/pages/home/app.tsx looks like after this step." %}}
```
import React from 'react';
import TopNavigation from '@cloudscape-design/components/top-navigation';
import AppLayout from '@cloudscape-design/components/app-layout';
import Navigation from './components/navigation';
import Breadcrumbs from './components/breadcrumbs';
import ContentLayout from '@cloudscape-design/components/content-layout';
import Header from '@cloudscape-design/components/header';
import PreparedDashboardContent from './components/prepared-dashboard-content';
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
            <PreparedDashboardContent />
          </ContentLayout>
        }
      />
    </>
  );
}
```
{{% /expand%}}

{{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/9.png?false&width=90pc)
{{% /expand%}}