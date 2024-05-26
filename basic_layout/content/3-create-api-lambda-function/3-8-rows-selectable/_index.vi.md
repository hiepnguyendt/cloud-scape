---
title : "Làm cho hàng của bảng có thể chọn"
date :  "`r Sys.Date()`" 
weight : 9 
chapter : false
pre : " <b>3.8.</b> "
---
Để cho phép người dùng thực hiện hành động đối với các mục trong bộ sưu tập, bạn có thể bật chức năng chọn. Thành phần bảng đi kèm với các loại lựa chọn khác nhau: ``none`` (tùy chọn mặc định), ``single`` (cho phép một mục duy nhất được chọn bằng một nút vô tuyến trong hàng bảng) và ``multi`` (allows multiple items to be selected at a time using check boxes for each item). Để sử dụng loại lựa chọn đơn, chúng ta cần làm như sau:
- Bao gồm số lượng các mục đã chọn trong bộ đếm tài nguyên tiêu đề.
- Thêm ``action`` vào ``Table``. Chúng tôi sẽ thêm hai nút: nút "Create flavor" và nút "Edit", được bật nếu người dùng đã chọn một mục trong bảng. Nút Sửa sẽ chuyển hướng đến trang sửa trong ví dụ chỉ đọc này. Thoải mái trải nghiệm để tùy chỉnh nó khi sử dụng nó trong ứng dụng tương tác đầy đủ của bạn.

Hãy bắt đầu kích hoạt lựa chọn bằng cách thêm thuộc tính ``selectionType="single`` vào bảng. Khi xem ứng dụng trong trình duyệt của bạn, bạn sẽ thấy các nút radio ở mỗi hàng.

Tiếp theo, chúng tôi sẽ mở rộng tiêu đề bảng để chỉ ra sự lựa chọn trong tiêu đề. Để có được văn bản đếm tiêu đề, chúng tôi sẽ sử dụng trợ lý này bên ngoài bảng:
  ```
  const getHeaderCounterText = (items: readonly Flavor[] = [], selectedItems: readonly Flavor[] = []) => {
  return selectedItems && selectedItems.length > 0 ? `(${selectedItems.length}/${items.length})` : `(${items.length})`;
};

  ```

Đây là thuộc tính tiêu đề bảng được cập nhật. Như bạn có thể thấy, chúng tôi chỉ bật nút sửa nếu một hàng được chọn.

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
Hãy nhìn vào trình duyệt của bạn và thử các chức năng lựa chọn được thêm vào gần đây. Bạn sẽ thấy rằng khi bạn chọn một hàng, nó sẽ được đặt lại trong các thay đổi về trang, sắp xếp, lọc và kích cỡ trang. Điều này là để ngăn người dùng thực hiện các hành động trên các mục mà họ có thể không biết là đã được chọn.

{{%expand "Xem src/pages/flavours/components/flavors-table.tsx trông như thế nào sau bước này." %}}
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

{{%expand "Xem ảnh chụp màn hình của trang trông như thế nào sau bước này." %}}
![Preparation](/images/21.png?false&width=90pc)
{{% /expand%}}