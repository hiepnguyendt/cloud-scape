---
title : "Add collection preferences to table "
date :  "`r Sys.Date()`" 
weight : 8 
chapter : false
pre : " <b>3.7.</b> "
---
With collection preferences, users can manage their display preferences within a collection. In this step, we let the user choose how many items are shown per page. Before adding the code, take a look at the [collection preferences component documentation](https://cloudscape.design/components/collection-preferences/).

We'll add the ``CollectionPreferences`` component. Set it as a ``preferences`` property value on our flavors ``Table``. Here's the component after we add it to the ``preferences`` property:

  ```
  <CollectionPreferences
    preferences={preferences} // Specifies the current preference values
    pageSizePreference={{
      // Configures the built-in "page size selection" preference
      //
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

  ```

  Take a look in your browser. You'll now see the settings icon next to the table pagination. Play around with it by setting different values and see how they get reflected in the UI.

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
        <Header variant="awsui-h1-sticky" counter={`(${flavors.length})`}>
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
    />
  );
}
```
{{% /expand%}}

{{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/20.png?false&width=90pc)
{{% /expand%}}