---
title : "Add the side navigation component to the app layout"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b>2.3.</b> "
---
Next, we will extend the app layout with the [side navigation component](https://cloudscape.design/components/side-navigation/?tabId=playground). The side navigation will be shown inside the app layout's navigation slot. It contains a list of navigational links that point to the pages within an application.

Take a look at the guidance and examples for [side navigation](https://cloudscape.design/components/side-navigation/?tabId=playground)  and [app layout](https://cloudscape.design/components/app-layout/?tabId=api) . Try to add the side navigation on your own using dummy data. Create the component in a separate file (```src/pages/home/components/navigation.tsx```) and provide the component to the app layout's navigation slot.

Here are the individual steps to create the side navigation in a separate file:

1. Open ```src/pages/home/components/navigation.tsx``` and import the component and it's props using ```import SideNavigation, { SideNavigationProps } from '@cloudscape-design/components/side-navigation';```.
2. Add the <SideNavigation /> inside our custom <Navigation /> component and wire it with the properties we want to display in the side navigation:
3. Specify the href of the currently active link we use the current path name location.pathname.
4. Set the header property to the current page ```header={{ href: '/home/index.html', text: 'Service' }}```

Here's how ```src/pages/home/components/navigation.tsx``` looks like:
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

Now attach the previously created component with the app layout to display it:
1. Open ```src/pages/home/app.tsx``` in your IDE.
2. Import our ```<Navigation />``` component with ```import Navigation from './components/navigation';```.
3. Add it to the ```<AppLayout />```'s navigation property: ```navigation={<Navigation />}```.

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

{{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/6.png?false&width=90pc)
{{% /expand%}}
