---
title : "Top navigation component"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b>2.1.</b> "
---
Let's start building our application by first adding the [top navigation component](https://cloudscape.design/components/top-navigation/) . The top navigation contains our logo and can include additional navigation elements, all of which will persist across all our service pages.

1. Open `src/pages/home/app.tsx` in your IDE.
2. Import the component by `import TopNavigation from '@cloudscape-design/components/top-navigation';`.
3. Add the <TopNavigation /> component to the `App` component's return statement.
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
    Take a look in your [browser](http://localhost:8000/home/index.html)  to see the top navigation component in action 🚀
    
{{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/3.png?false&width=90pc)
{{% /expand%}}
