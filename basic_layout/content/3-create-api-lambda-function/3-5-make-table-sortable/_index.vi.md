---
title : "Make the table sortable"
date :  "`r Sys.Date()`" 
weight : 6 
chapter : false
pre : " <b>3.5.</b> "
---
You might have noticed the ``sorting`` configuration in the step where we set up the collection hooks. This configuration sets the initial sorting state on the first render. To make the columns sortable, we need to extend the ``columnDefinitions`` by the ``sortingField`` field for each column. The value is used in collection hooks to reorder the items. Provide the name of the property within each item that should be used for sorting by this column.

Here's the extended columnDefinitions definition:

  ```
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

  ```

To reflect sorting on the table, we need to spread the ``collectionProps`` on the ``Table`` component. It contains the different properties that are needed to reflect sorting (``sortingColumn``, ``sortingDescending``, ``onSortingChange``) among other properties in the user interface (UI). Let's spread the ``collectionProps`` on the ``Table``:
  ```
  <Table<Flavor>
  {...collectionProps}
  items={items}
  columnDefinitions={columnDefinitions}
  stickyHeader={true}
  resizableColumns={true}
  variant="full-page"
  header={
    <Header variant="awsui-h1-sticky" counter={`(${flavors.length})`}>
      Flavors
    </Header>
  }
  pagination={<Pagination {...paginationProps} />}
/>;

  ```

You can now see sorting on the different columns reflected in the browser.

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
    />
  );
}
```
{{% /expand%}}

{{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/17.png?false&width=90pc)
{{% /expand%}}
