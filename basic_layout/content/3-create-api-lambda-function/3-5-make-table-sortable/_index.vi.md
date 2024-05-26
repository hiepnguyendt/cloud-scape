---
title : "Làm cho bảng có thể sắp xếp"
date :  "`r Sys.Date()`" 
weight : 6 
chapter : false
pre : " <b>3.5.</b> "
---
Bạn có thể đã nhận thấy cấu hình ``sorting`` trong bước mà chúng tôi thiết lập các hook. Cấu hình này đặt trạng thái sắp xếp ban đầu trong lần hiển thị đầu tiên. Để làm cho các cột có thể sắp xếp, chúng ta cần mở rộng ``columnDefinitions`` bằng trường ``sortingField`` cho mỗi cột. Giá trị được sử dụng trong hook để sắp xếp lại các mục. Cung cấp tên của thuộc tính trong mỗi mục nên được sử dụng để sắp xếp theo cột này.

Đây là columnDefinitions mở rộng:

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

Bây giờ bạn có thể thấy việc sắp xếp trên các cột khác nhau được phản ánh trong trình duyệt.

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

{{%expand "Xem ảnh chụp màn hình của trang trông như thế nào sau bước này." %}}
![Preparation](/images/17.png?false&width=90pc)
{{% /expand%}}
