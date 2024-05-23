---
title : "Add the form component"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b>4.1.</b> "
---

Let's add the form component to our page. This component should be used for a section of a page that has interactive controls with which a user can submit information to a web server. Take a look at the [form component documentation](https://cloudscape.design/components/form/?tabId=usage) and make yourself familiar with the usage guidelines and API.
{{% notice info %}}
Compared to the previous steps, there is one change to the app layout. Can you figure it out? ``AppLayout's contentType`` is now set to ``form``. We apply the recommended default panel states by setting contentType='form' in the app layout component. This sets the side navigation to be closed by default.
{{% /notice %}}

1. Open ``src/pages/create-flavor/app.tsx`` in your IDE.
2. Import the component with ``@cloudscape-design/components/form;``.
3. Add the ``<Form />`` component to the app layout component's return statement. As we've seen in the component's playground example, we have to add it as a child of the native ``<form />`` (lowercase "F") element. The native form enables us to handle the submission and make use of [React FormEvents API](https://reactjs.org/docs/forms.html).
4. Lastly, add a "Cancel" and a "Create flavor" button to the form by using the ``actions`` property (Hint: You can find a code example in the [form component's playground](https://cloudscape.design/components/form/?tabId=playground))

{{%expand "See how src/pages/create-flavor/app.tsx looks like after this step:" %}}
```
import React from 'react';
import Button from '@cloudscape-design/components/button';
import Form from '@cloudscape-design/components/form';
import Header from '@cloudscape-design/components/header';
import HelpPanel from '@cloudscape-design/components/help-panel';
import SpaceBetween from '@cloudscape-design/components/space-between';
import ContentLayout from '@cloudscape-design/components/content-layout';

import Breadcrumbs from '../../components/breadcrumbs';
import Navigation from '../../components/navigation';
import ShellLayout from '../../layouts/shell';

export default function App() {
  return (
    <ShellLayout
      contentType="form"
      breadcrumbs={<Breadcrumbs active={{ text: 'Create flavor', href: '/create-flavor/index.html' }} />}
      navigation={<Navigation />}
      tools={<HelpPanel header={<h2>Help panel</h2>} />}
    >
      <ContentLayout
        header={
          <Header
            variant="h1"
            description="Create a new flavor by specifying ingredients, quality, and price. On creation a flavor will be tested by the product and marketing team."
          >
            Create flavor
          </Header>
        }
      >
        <form onSubmit={event => event.preventDefault()}>
          <Form
            actions={
              <SpaceBetween direction="horizontal" size="xs">
                <Button href="/flavors/index.html" variant="link">
                  Cancel
                </Button>
                <Button formAction="submit" variant="primary">
                  Create flavor
                </Button>
              </SpaceBetween>
            }
          ></Form>
        </form>
      </ContentLayout>
    </ShellLayout>
  );
}
```
{{% /expand%}}

{{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/25.png?false&width=90pc)
{{% /expand%}}