---
title : "Table view"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b>3.</b> "
---
A common use case for a data-heavy web application is [to view resources](https://cloudscape.design/patterns/resource-management/view/) . In this step, we'll look at how we can use the [table view pattern](https://cloudscape.design/patterns/resource-management/view/table-view/)  to visualize and organize data.

We're going to create a table to list all of the chocolates that we produce in our workshop factory. This step is driven by different design patterns. We'll take a look at these patterns, followed by implementing them using Cloudscape components. We'll start with a basic table and end the step displaying the data in a table that is filterable, searchable, sortable, and includes pagination and configurable preferences.

We use mock data for the workshop and focus on how to use the components, rather than on how to connect to APIs to fetch data. In a real world application the data would be obtained by fetching it from a server.

Sounds like a lot, right? Don't worry, some parts are predefined. Let's jump into it!

{{% notice note %}}
You're observing a different application structure compared to the latest state in Step 1. We have adapted the structure of what we've done so far to use the same app layout shared across multiple pages. One important detail about the modifications: the app layout component's contentType property gets set to table. This is to determine the default behavior of the app layout component based on some predefined page layouts.
{{% /notice %}}
See how the app looks before and after this step:
|  Before      |   After       |   
| :-------------: | :-------------: |
| ![Preparation](/images/12.png?false&width=90pc&height=30pc) | ![Preparation](/images/21.png?false&width=90pc&height=30pc) |   