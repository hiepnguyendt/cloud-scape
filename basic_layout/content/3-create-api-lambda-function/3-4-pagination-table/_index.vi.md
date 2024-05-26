---
title : "Add pagination to the table"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b>3.4.</b> "
---
Như chúng ta đã thấy khi thiết lập, có những biến khác nhau được xây dựng từ ``useCollection`` hook: ``items``, ``filterProps``, ``filteredItemsCount``, ``paginationProps`` và ``collectionProps``. Trong bước này, chúng tôi sử dụng ``paginationProps``. Chúng ta có thể sử dụng đối tượng ``paginationProps`` để phân tán trên thành phần pagination. Hãy làm điều đó bằng cách thêm thành phần ``Pagination`` như một thuộc tính vào bảng bên trong ``src/pages/flavor/components/flavors-table.tsx:``

```pagination={<Pagination {...paginationProps} />}```

Hãy nhìn vào trình duyệt của bạn. Bây giờ dạng xem bảng hiển thị 20 mục mỗi trang và đi kèm với một trang. Thoải mái trải nghiệm với các yếu tố mới được thêm vào và các giá trị mặc định.

{{%expand "Xem src/pages/flavor/components/flavors-table.tsx trông như thế nào sau bước này." %}}
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

{{%expand "Xem ảnh chụp màn hình của trang trông như thế nào sau bước này." %}}
![Preparation](/images/16.png?false&width=90pc)
{{% /expand%}}
