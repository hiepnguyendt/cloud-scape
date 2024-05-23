---
title : "Test dark and compact modes"
date :  "`r Sys.Date()`" 
weight : 9 
chapter : false
pre : " <b>2.9.</b> "
---
Cloudscape supports:
- Visual modes  used to optimize the user interface based on environmental conditions and user preferences. We support light and dark modes.
- Density modes  used to optimize data density. We support comfortable and compact modes.

Open the developer tools built into your browser and:
- Toggle dark and light modes by executing the following command in the console
    ```
    document.body.classList.toggle('awsui-dark-mode')

    ```
- Toggle compact and comfortable modes by executing the following command in the console
    ```
    document.body.classList.toggle('awsui-compact-mode')

    ```
{{%expand "See the screenshot of how the page looks like in the different modes." %}}
![Preparation](/images/10.png?false&width=90pc)
{{% /expand%}}
