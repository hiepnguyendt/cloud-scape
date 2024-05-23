---
title : "Add pagination to the table"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b>3.4.</b> "
---
As we've seen when setting up the collection, there are different variables constructed from the ``useCollection`` hook: ``items``, ``filterProps``, ``filteredItemsCount``, ``paginationProps`` and ``collectionProps``. In this step, we make use of ``paginationProps``. We can use the ``paginationProps`` object to spread on the pagination component. Let's do that by adding the ``Pagination`` component as a property to the Table inside ``src/pages/flavors/components/flavors-table.tsx:``

```pagination={<Pagination {...paginationProps} />}```

Take a look in your browser. The table view now shows 20 items per page and comes with a pagination. Feel free to play around with the newly added elements and default values.

{{%expand "See how src/pages/flavors/components/flavors-table.tsx looks like after this step." %}}
```
import React, { useState } from 'react';
import Header from '@cloudscape-design/components/header';
import Table, { TableProps } from '@cloudscape-design/components/table';
import { useCollection } from '@cloudscape-design/collection-hooks';
import { Flavor } from '../data';
import { CollectionPreferencesProps } from '@cloudscape-design/components';
import Pagination from '@cloudscape-design/components/pagination';

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
      pagination={<Pagination {...paginationProps} />}
    />
  );
}
```
{{% /expand%}}

{{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/16.png?false&width=90pc)
{{% /expand%}}
