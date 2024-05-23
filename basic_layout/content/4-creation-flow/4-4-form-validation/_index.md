---
title : "Apply form field validation pattern"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b>4.4.</b> "
---

Before we start coding, take a look at the [validation pattern](https://cloudscape.design/patterns/general/validation/)  which we'll apply in this step. We'll apply field validation to fields that are not labeled as optional. To summarize, we'll validate the data after a user submits a form for the first time. We don’t validate before the user submits the form for the first time. On subsequent attempts, validation is happening as the user completes each field. For showing the error text, we use the ``FormField`` component ``errorText`` property. If it contains text, it will be shown. If it's empty, nothing will be shown. With that explained, you're ready to go. Give it a try and apply the validation pattern.

{{%expand "See how src/pages/create-flavor/app.tsx looks like after this step:" %}}
```
import React, { useState } from 'react';
import Button from '@cloudscape-design/components/button';
import Form from '@cloudscape-design/components/form';
import Header from '@cloudscape-design/components/header';
import HelpPanel from '@cloudscape-design/components/help-panel';
import SpaceBetween from '@cloudscape-design/components/space-between';
import ContentLayout from '@cloudscape-design/components/content-layout';
import FormField from '@cloudscape-design/components/form-field';
import Tiles from '@cloudscape-design/components/tiles';
import Container from '@cloudscape-design/components/container';
import Multiselect, { MultiselectProps } from '@cloudscape-design/components/multiselect';
import RadioGroup from '@cloudscape-design/components/radio-group';
import Textarea from '@cloudscape-design/components/textarea';
import ColumnLayout from '@cloudscape-design/components/column-layout';
import Input from '@cloudscape-design/components/input';

import Breadcrumbs from '../../components/breadcrumbs';
import Navigation from '../../components/navigation';
import ShellLayout from '../../layouts/shell';
import { chocolate, fruits } from '../flavors/data';

const options = [...fruits, ...chocolate].map(i => ({ value: i, label: i }));
const isEmptyString = (value: string) => !value?.length;

export default function App() {
  const [shape, setShape] = useState('bar');
  const [organic, setOrganic] = useState('yes');
  const [selectedIngredients, setSelectedIngredients] = useState<MultiselectProps['selectedOptions']>([]);
  const [wholeSalePrice, setWholeSalePrice] = useState('');
  const [retailPrice, setRetailPrice] = useState('');
  const [additionalNotes, setAdditionalNotes] = useState('');
  const [isFormSubmitted, setIsFormSubmitted] = useState(false);

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
        <form
          onSubmit={event => {
            event.preventDefault();
            setIsFormSubmitted(true);
          }}
        >
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
            <SpaceBetween size="l">
              <Container header={<Header variant="h2">Chocolate shape</Header>}>
                <FormField label="Shape" stretch={true}>
                  <Tiles
                    items={[
                      {
                        value: 'bar',
                        label: 'Bar',
                        description: 'Traditional bar-shaped chocolate',
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
              <Container header={<Header variant="h2">Ingredients</Header>}>
                <SpaceBetween direction="vertical" size="l">
                  <FormField
                    label="List of ingredients"
                    errorText={
                      isFormSubmitted && selectedIngredients.length === 0 && 'List of ingredients is required.'
                    }
                    i18nStrings={{
                      errorIconAriaLabel: 'Error',
                    }}
                  >
                    <Multiselect
                      placeholder="Select all ingredients"
                      selectedOptions={selectedIngredients}
                      onChange={({ detail }) => setSelectedIngredients(detail.selectedOptions)}
                      options={options}
                      deselectAriaLabel={option => {
                        const label = option?.value || option?.label;
                        return label ? `Deselect ${label}` : 'no label';
                      }}
                    />
                  </FormField>
                  <FormField label="Organic">
                    <RadioGroup
                      value={organic}
                      onChange={({ detail }) => setOrganic(detail.value)}
                      items={[
                        { value: 'no', label: 'No' },
                        { value: 'yes', label: 'Yes' },
                      ]}
                    />
                  </FormField>
                </SpaceBetween>
              </Container>
              <Container header={<Header variant="h2">Marketing</Header>}>
                <FormField label="Prices" description="Define the prices for wholesale and retail.">
                  <SpaceBetween direction="vertical" size="l">
                    <ColumnLayout columns={2}>
                      <FormField
                        label="Wholesale price"
                        stretch={true}
                        errorText={isFormSubmitted && isEmptyString(wholeSalePrice) && 'Wholesale Price is required.'}
                        i18nStrings={{
                          errorIconAriaLabel: 'Error',
                        }}
                      >
                        <Input
                          value={wholeSalePrice}
                          onChange={({ detail }) => setWholeSalePrice(detail.value)}
                          type="number"
                        />
                      </FormField>
                      <FormField
                        label="Retail price"
                        stretch={true}
                        errorText={isFormSubmitted && isEmptyString(retailPrice) && 'Retail Price is required.'}
                        i18nStrings={{
                          errorIconAriaLabel: 'Error',
                        }}
                      >
                        <Input
                          value={retailPrice}
                          onChange={({ detail }) => setRetailPrice(detail.value)}
                          type="number"
                        />
                      </FormField>
                    </ColumnLayout>
                    <FormField
                      label={
                        <>
                          Additional notes<i> - optional</i>
                        </>
                      }
                      stretch={true}
                    >
                      <Textarea onChange={({ detail }) => setAdditionalNotes(detail.value)} value={additionalNotes} />
                    </FormField>
                  </SpaceBetween>
                </FormField>
              </Container>
            </SpaceBetween>
          </Form>
        </form>
      </ContentLayout>
    </ShellLayout>
  );
}
```
{{% /expand%}}

Take a look at the form page in your browser and test your implementation of the form field validation pattern.

{{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/28.png?false&width=90pc)
{{% /expand%}}
