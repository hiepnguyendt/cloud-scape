---
title : "Add the first input form field"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b>4.2.</b> "
---

Let's get interactive and fill our form with life. In this step, we'll add input form elements. Cloudscape input form elements are controlled components. This means their value is controlled by React (more information about controlled components on [React's documentation website](https://reactjs.org/docs/forms.html#controlled-components)). Also, every input form components will be wrapped by the FormField component. 

Here's how the form will look after we added the input form components:
![Preparation](/images/27.png?false&width=90pc)

Before you take a look at the code after this step, try it by yourself. Use the component's documentation pages for help.

1. For this field we will add the following imports:
    ```    
    import Container from '@cloudscape-design/components/container';
    import FormField from '@cloudscape-design/components/form-field';
    import Tiles from '@cloudscape-design/components/tiles';
    ```

2. Add a ``Container`` component to the form with a ``Header`` (variant: h2) named "Chocolate Shape" as a child to the ``Form`` component.
3. Add a ``FormField`` component as child of the ``Container`` with the label set to "Shape" and ``stretch`` set to ``true``.
4. Add a ``Tiles`` component as a child of ``FormField`` and add the labels according to the screenshot above.

{{%expand "See how src/pages/create-flavor/app.tsx looks like after this step:" %}}
```
import React, { useState } from 'react';
import Button from '@cloudscape-design/components/button';
import Form from '@cloudscape-design/components/form';
import Header from '@cloudscape-design/components/header';
import HelpPanel from '@cloudscape-design/components/help-panel';
import Container from '@cloudscape-design/components/container';
import SpaceBetween from '@cloudscape-design/components/space-between';
import ContentLayout from '@cloudscape-design/components/content-layout';
import FormField from '@cloudscape-design/components/form-field';
import Tiles from '@cloudscape-design/components/tiles';

import Breadcrumbs from '../../components/breadcrumbs';
import Navigation from '../../components/navigation';
import ShellLayout from '../../layouts/shell';

export default function App() {
  const [shape, setShape] = useState('bar');

  return (
    <ShellLayout
      contentType="form"
      breadcrumbs={<Breadcrumbs active={{ text: 'Create Flavor', href: '/create-flavor/index.html' }} />}
      navigation={<Navigation />}
      tools={<HelpPanel header={<h2>Help Panel</h2>} />}
    >
      <ContentLayout
        header={
          <Header
            variant="h1"
            description="Create a new flavor by specifying ingredients, quality, and price. On creation a flavor will be tested by the product and marketing team."
          >
            Create Flavor
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
          >
            <Container header={<Header variant="h2">Chocolate Shape</Header>}>
              <FormField label="Shape" stretch={true}>
                <Tiles
                  items={[
                    {
                      value: 'bar',
                      label: 'Bar',
                      description: 'Traditional bar shaped chocolate',
                    },
                    {
                      value: 'praline',
                      label: 'Pralines',
                      description: 'Sophisticated and premium-looking chocolate shape',
                    },
                  ]}
                  value={shape}
                  onChange={e => setShape(e.detail.value)}
                />
              </FormField>
            </Container>
          </Form>
        </form>
      </ContentLayout>
    </ShellLayout>
  );
}
```
{{% /expand%}}
{{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/26.png?false&width=90pc)
{{% /expand%}}