---
title : "Make table rows selectable"
date :  "`r Sys.Date()`" 
weight : 9 
chapter : false
pre : " <b>3.8.</b> "
---
To allow users to take an action on the items in the collection, you can enable the selection functionality. The table component comes with different selection types: ``none`` (default), ``single`` (allows a single item to be selected using a radio button in the table row) and ``multi`` (allows multiple items to be selected at a time using check boxes for each item). To make use of the single selection type, we need to do the following:

- Include the number of selected items in the header resource counter.
- Add ``actions`` to the ``Table``. We'll add two buttons: a "Create flavor" button and an "Edit" button, which is enabled if a user has selected an item in the table. The Edit button redirects to the edit page in this read-only example. Feel free to adapt it when using it in your full interactive application.

Let's start enabling selection by adding the ``selectionType="single`` property to the table. When viewing the application in your browser, you'll see radio buttons in each row.

Next, we'll extend the table header to indicate the selection in the header. For getting the header counter text, we'll make use of this helper outside the table:
  ```
  const getHeaderCounterText = (items: readonly Flavor[] = [], selectedItems: readonly Flavor[] = []) => {
  return selectedItems && selectedItems.length > 0 ? `(${selectedItems.length}/${items.length})` : `(${items.length})`;
};

  ```

Here's the updated Table header property. As you can see, we enable the Edit button only if a row is selected.

```
<Header
  variant="awsui-h1-sticky"
  counter={getHeaderCounterText(flavors, collectionProps.selectedItems)}
  actions={
    <SpaceBetween size="s" direction="horizontal">
      <Button disabled={collectionProps.selectedItems?.length === 0}>Edit</Button>
      <Button href="/create-flavor/index.html" variant="primary">
        Create flavor
      </Button>
    </SpaceBetween>
  }
>
  Flavors
</Header>

```
Take a look in your browser and try out the recently added selection functionality. You'll see that when selecting a row, it gets reset across pagination, sorting, filtering, and page size changes. This is to prevent users from performing actions on items they might not know are selected.

{{%expand "See how src/pages/flavors/components/flavors-table.tsx looks like after this step." %}}
```
import React, { useState, ReactNode } from 'react';
import Header from '@cloudscape-design/components/header';
import Table, { TableProps } from '@cloudscape-design/components/table';
import { useCollection } from '@cloudscape-design/collection-hooks';
import { Flavor } from '../data';
import { CollectionPreferencesProps } from '@cloudscape-design/components';
import Pagination from '@cloudscape-design/components/pagination';
import Box from '@cloudscape-design/components/box';
import Button from '@cloudscape-design/components/button';
import TextFilter from '@cloudscape-design/components/text-filter';
import CollectionPreferences from '@cloudscape-design/components/collection-preferences';
import SpaceBetween from '@cloudscape-design/components/space-between';

const getFilterCounterText = (count: number = 0) => `${count} ${count === 1 ? 'match' : 'matches'}`;

const columnDefinitions: TableProps<Flavor>['columnDefinitions'] = [
  {
    header: 'Name',
    cell: ({ name }) => name,
    sortingField: 'name',
    minWidth: 175,
  },
  {
    header: 'Sold (last month)',
    cell: ({ sold }) => sold,
    sortingField: 'sold',
    minWidth: 160,
  },
  {
    header: 'Produced (last month)',
    cell: ({ produced }) => produced,
    sortingField: 'produced',
    minWidth: 160,
  },
  {
    header: 'Estimated (next month)',
    cell: ({ estimated }) => estimated,
    sortingField: 'estimated',
    minWidth: 150,
  },
  {
    header: 'Retail price (USD)',
    cell: ({ retailPrice }) => retailPrice,
    sortingField: 'retailPrice',
    minWidth: 160,
  },
  {
    header: 'Total revenue (USD)',
    cell: ({ totalRevenue }) => totalRevenue,
    sortingField: 'totalRevenue',
    minWidth: 180,
  },
  {
    header: 'Total cost (USD)',
    cell: ({ totalCost }) => totalCost,
    sortingField: 'totalCost',
    minWidth: 180,
  },
];

const EmptyState = ({ title, subtitle, action }: { title: string; subtitle: string; action: ReactNode }) => {
  return (
    <Box textAlign="center" color="inherit">
      <Box variant="strong" textAlign="center" color="inherit">
        {title}
      </Box>
      <Box variant="p" padding={{ bottom: 's' }} color="inherit">
        {subtitle}
      </Box>
      {action}
    </Box>
  );
};

export interface VariationTableProps {
  flavors: Flavor[];
}

const getHeaderCounterText = (items: readonly Flavor[] = [], selectedItems: readonly Flavor[] = []) => {
  return selectedItems && selectedItems.length > 0 ? `(${selectedItems.length}/${items.length})` : `(${items.length})`;
};

export default function VariationTable({ flavors }: VariationTableProps) {
  const [preferences, setPreferences] = useState<CollectionPreferencesProps['preferences']>({ pageSize: 20 });
  const { items, filterProps, actions, filteredItemsCount, paginationProps, collectionProps } = useCollection<Flavor>(
    flavors,
    {
      filtering: {
        noMatch: (
          <EmptyState
            title="No matches"
            subtitle="We can’t find a match."
            action={<Button onClick={() => actions.setFiltering('')}>Clear filter</Button>}
          />
        ),
        empty: (
          <EmptyState title="No flavors" subtitle="No flavors to display." action={<Button>Create flavor</Button>} />
        ),
      },
      pagination: { pageSize: preferences?.pageSize },
      sorting: { defaultState: { sortingColumn: columnDefinitions[0] } },
      selection: {},
    }
  );

  return (
    <Table<Flavor>
      {...collectionProps}
      items={items}
      columnDefinitions={columnDefinitions}
      stickyHeader={true}
      variant="full-page"
      ariaLabels={{
        selectionGroupLabel: 'Items selection',
        itemSelectionLabel: ({ selectedItems }, item) => {
          const isItemSelected = selectedItems.filter(i => i.name === item.name).length;
          return `${item.name} is ${isItemSelected ? '' : 'not '}selected`;
        },
        tableLabel: 'Flavors table',
      }}
      resizableColumns={true}
      header={
        <Header
          variant="awsui-h1-sticky"
          counter={getHeaderCounterText(flavors, collectionProps.selectedItems)}
          actions={
            <SpaceBetween size="s" direction="horizontal">
              <Button disabled={collectionProps.selectedItems?.length === 0}>Edit</Button>
              <Button href="/create-flavor/index.html" variant="primary">
                Create flavor
              </Button>
            </SpaceBetween>
          }
        >
          Flavors
        </Header>
      }
      pagination={<Pagination {...paginationProps} />}
      filter={
        <TextFilter
          {...filterProps}
          filteringPlaceholder="Find flavors"
          countText={getFilterCounterText(filteredItemsCount)}
        />
      }
      preferences={
        <CollectionPreferences
          preferences={preferences} // Specifies the current preference values
          pageSizePreference={{
            // Configures the built-in "page size selection" preference
            title: 'Select page size',
            options: [
              { value: 10, label: '10 resources' },
              { value: 20, label: '20 resources' },
              { value: 50, label: '50 resources' },
              { value: 100, label: '100 resources' },
            ],
          }}
          onConfirm={({ detail }) => setPreferences(detail)} // Called when the user confirms a preference change using the confirm button in the modal footer.
          title="Preferences" // Specifies the title of the preferences modal dialog.
          confirmLabel="Confirm" // Label of the confirm button in the modal footer.
          cancelLabel="Cancel" // Label of the cancel button in the modal footer.
        />
      }
      selectionType="single"
    />
  );
}
```
{{% /expand%}}

{{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/21.png?false&width=90pc)
{{% /expand%}}