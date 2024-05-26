---
title : "Add collection hooks to the table component"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b>3.3.</b> "
---
#### Giới thiệu về hooks
Bây giờ chúng ta đang học về Hooks. Hooks giúp bạn xử lý các hoạt động dữ liệu trong các thành phần thu thập dữ liệu như bảng hoặc thẻ. Gói này cung cấp các tiện ích để xử lý các hoạt động lọc, sắp xếp hoặc trang hoá cho các bộ sưu tập của khách hàng ("bộ sưu tập của khách hàng" có nghĩa là toàn bộ tập dữ liệu mô tả tập hợp được lấy từ phía khách hàng).

Hãy dành vài phút và làm quen với [collection hooks documentation](https://cloudscape.design/get-started/dev-guides/collection-hooks/).

#### Thêm hooks vào thành phần bảng

Gói ``@cloudscape-design/collection-hooks``được exports từ ``useCollection`` hook. Nó cần các mục bộ sưu tập gốc và một cấu hình để trả về nội dung được lọc, sắp xếp và trang hóa, tùy theo cấu hình của bạn. Vậy hãy xác định cấu hình, truyền các mục gốc cho nó, và thiết lập nó.

1. Mở ``src/pages/flavors/components/flavors-table.tsx``
2. Nhập useCollection hook với  ``import { useCollection } from '@cloudscape-design/collection-hooks';``
3. Xác định trạng thái để lưu trữ các trạng thái ưu tiên khác nhau của bộ sưu tập bên trong thành phần VariationTable.
    ```
    const [preferences, setPreferences] = useState<CollectionPreferencesProps['preferences']>({ pageSize: 20 });
    ```
Tiếp theo, chúng tôi thêm ``useCollection``hook bên dưới ``preferences'' hook. Gửi các mục gốc của chúng tôi cho nó và xác định cấu hình. Tìm cái cấu hình chi tiết tại [collection hook API documentation website](https://cloudscape.design/get-started/dev-guides/collection-hooks/#api).

```  
  const { items, filterProps, filteredItemsCount, paginationProps, collectionProps } = useCollection<Flavor>(flavors, {
  filtering: {},
  pagination: { pageSize: preferences?.pageSize },
  sorting: { defaultState: { sortingColumn: columnDefinitions[0] } },
  selection: {},
});
```
Update your table's ``items`` property to now use the returned items value from the hook instead of ``flavors``, and that's it! When you take a look at the page in your browser, you won't see any visual changes. That's because this step contained the basic setup of the collection hook. We'll connect the different parts (pagination, sorting, filter) with the table in the next steps.
Cập nhật thuộc tính ``items`` của bảng để bây giờ sử dụng giá trị của các mục trả về từ hook thay vì ``flavors``, và đó là tất cả! Khi bạn nhìn vào trang trong trình duyệt, bạn sẽ không thấy bất kỳ thay đổi trực quan nào. Đó là bởi vì bước này chứa các thiết lập cơ bản. Chúng tôi sẽ kết nối các phần khác nhau (phân trang, sắp xếp, lọc) với bảng trong các bước tiếp theo.


{{%expand "Xem src/pages/flavor/components/flavors-table.tsx trông như thế nào sau bước này." %}}
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
