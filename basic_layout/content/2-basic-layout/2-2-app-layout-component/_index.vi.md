---
title : "Add the app layout component"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b>2.2.</b> "
---
Let's continue adding the [app layout component](https://cloudscape.design/components/app-layout/). This component provides the basic layout for all types of pages, including collapsible side navigation and a tools panel.

1. Open ```src/pages/home/app.tsx``` in your IDE.
2. Import the component using ```import AppLayout from '@cloudscape-design/components/app-layout';```.
3. Add the ```<AppLayout />``` as a sibling next to the ```<TopNavigation />``` component wrapped in a [React fragment](https://reactjs.org/docs/fragments.html).
4. Set the AppLayout's ```ariaLabels``` properties as described in the [AppLayout API documentation](https://cloudscape.design/components/app-layout/).
5. The AppLayout component can be used with different headers. It provides property ```headerSelector``` to specify the CSS selector of the header you are using. Setting this property is important to ensure that the AppLayout position doesn't overlap the header.\
    a. Wrap the ```<TopNavigation />``` component with a ```<div />``` element. Add a id attribute with value top-nav to the wrapping ```<div />``` element.\
    b. Set the AppLayout's ```headerSelector``` property to a valid css selector ```#top-nav```.
6. To prevent an overlap, make the TopNavigation sticky. This gets handled inside src/pages/home/styles.css.\
    a. Add the required import ```import './styles.css';```.\
    b. Add the necessary styling for the top navigation. Hint: An example of this styling can be found on the bottom of the Top navigation [documentation page](https://cloudscape.design/components/top-navigation/?tabId=api#code-examples).
This is how ```src/pages/home/app.tsx``` looks like after this step:
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

This is how ```src/pages/home/styles.css``` looks like after this step:
```
    #top-nav
    {
        position: sticky;
        top: 0;
        z-index: 1002;
    }

```
You should now have an excitingly empty page consisting of an empty app layout, top navigation, and no content.

{{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/5.png?false&width=90pc)

{{% /expand%}}