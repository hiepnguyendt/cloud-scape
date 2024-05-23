---
title : "Add collection hooks to the table component"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b>3.3.</b> "
---
#### Introduction to collection hooks

We're now learning about collection hooks. Collection hooks help you handle data operations in collection components such as table or cards. The package provides utilities to handle filtering, sorting, or pagination operations for client-side collections ("client-side collections" means the full dataset describing the collection is fetched on the client side).

Take a few minutes and familiarize yourself with the [collection hooks documentation](https://cloudscape.design/get-started/dev-guides/collection-hooks/).

#### Add collection hooks to the table component

The ``@cloudscape-design/collection-hooks`` package exports the ``useCollection`` hook. It takes the original collection items and a configuration to return filtered, sorted, and paginated content, according to your configuration. So let's define the configuration, pass the original items to it, and get it set up.

1. Open ``src/pages/flavors/components/flavors-table.tsx``
2. Import the useCollection hook with ``import { useCollection } from '@cloudscape-design/collection-hooks';``
3. Define a state to store the various preference state of our collection inside the VariationTable component.
    ```
    const [preferences, setPreferences] = useState<CollectionPreferencesProps['preferences']>({ pageSize: 20 });
    ```
Next, we add the ``useCollection`` hook below the preferences hook. Pass our original items to it and define the configuration. Find the configuration details on the [collection hook API documentation website](https://cloudscape.design/get-started/dev-guides/collection-hooks/#api).

```  
  const { items, filterProps, filteredItemsCount, paginationProps, collectionProps } = useCollection<Flavor>(flavors, {
  filtering: {},
  pagination: { pageSize: preferences?.pageSize },
  sorting: { defaultState: { sortingColumn: columnDefinitions[0] } },
  selection: {},
});
```
Update your table's ``items`` property to now use the returned items value from the hook instead of ``flavors``, and that's it! When you take a look at the page in your browser, you won't see any visual changes. That's because this step contained the basic setup of the collection hook. We'll connect the different parts (pagination, sorting, filter) with the table in the next steps.


{{%expand "See how src/pages/flavors/components/flavors-table.tsx looks like after this step." %}}
```
import React from 'react';
import Header from '@cloudscape-design/components/header';
import Table, { TableProps } from '@cloudscape-design/components/table';
import { useCollection } from '@cloudscape-design/collection-hooks';
import { Flavor } from '../data';
import { useState } from 'react';
import { CollectionPreferencesProps } from '@cloudscape-design/components';

const columnDefinitions: TableProps<Flavor>['columnDefinitions'] = [
  {
    header: 'Name',
    cell: ({ name }) => name,
    minWidth: 175,
  },
  {
    header: 'Sold (last month)',
    cell: ({ sold }) => sold,
    minWidth: 160,
  },
  {
    header: 'Produced (last month)',
    cell: ({ produced }) => produced,
    minWidth: 160,
  },
  {
    header: 'Estimated (next month)',
    cell: ({ estimated }) => estimated,
    minWidth: 150,
  },
  {
    header: 'Retail price (USD)',
    cell: ({ retailPrice }) => retailPrice,
    minWidth: 160,
  },
  {
    header: 'Total revenue (USD)',
    cell: ({ totalRevenue }) => totalRevenue,
    minWidth: 180,
  },
  {
    header: 'Total cost (USD)',
    cell: ({ totalCost }) => totalCost,
    minWidth: 180,
  },
];

export interface VariationTableProps {
  flavors: Flavor[];
}

export default function VariationTable({ flavors }: VariationTableProps) {
  const [preferences, setPreferences] = useState<CollectionPreferencesProps['preferences']>({ pageSize: 20 });
  const { items, filterProps, filteredItemsCount, paginationProps, collectionProps } = useCollection<Flavor>(flavors, {
    filtering: {},
    pagination: { pageSize: preferences?.pageSize },
    sorting: { defaultState: { sortingColumn: columnDefinitions[0] } },
    selection: {},
  });

  return (
    <Table<Flavor>
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
    />
  );
}
```
{{% /expand%}}
