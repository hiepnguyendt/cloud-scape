---
title : "Add a text filter to the table"
date :  "`r Sys.Date()`" 
weight : 7 
chapter : false
pre : " <b>3.6.</b> "
---
Tìm kiếm tên theo cách thủ công trong lượng dữ liệu này là tốn thời gian. Hãy sửa điều đó bằng cách thêm văn bản ``filter`` vào bảng. Chúng tôi làm điều này thông qua thuộc tính lọc của thành phần bảng. Chúng tôi thêm thành phần bộ lọc văn bản (https://cloudscape.design/components/text-filter/?tabId=playground) vào bảng với hooks filterProps. Thêm vào đó, chúng tôi đặt thuộc tính ``filteringPlaceholder`` và ``countText``. ``filteringPlaceholder`` được sử dụng để giữ vị trí cho đầu vào lọc và ``countText`` là một chuỗi có thể đọc được bởi con người, được định vị cho biết số lượng kết quả.

Bằng cách thêm thành phần bộ lọc văn bản, chúng tôi giới thiệu một trạng thái mới trên bảng. Chúng tôi sẽ hiển thị gì nếu không có kết quả nào? Bạn có nhớ mẫu xem bảng mà bạn đã xem khi chúng tôi bắt đầu bước 2 không? Phải, nó đề cập đến [empty state pattern](https://cloudscape.design/patterns/general/empty-states/). Hãy dành vài phút và làm quen. Chúng tôi sẽ sử dụng mô hình này với một cách diễn đạt hơi khác để phản ánh trạng thái ``noMatch``, được hiển thị khi hoạt động lọc không trả về bất kỳ sự phù hợp nào.

Để thêm mô hình này, hãy tạo một thành phần tùy chỉnh trong ``src/pages/flavours/components/flavors-table.tsx`` mà xây dựng UI:

  ```
  // Add the Box to your imports at the top of the file
  import Box from '@cloudscape-design/components/box';

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

  ```
  Now you set the ``noMatch`` property inside collection hooks preferences and add the ``actions`` variable to the destructuring assignment:

  ```
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
    },
    pagination: { pageSize: preferences?.pageSize },
    sorting: { defaultState: { sortingColumn: columnDefinitions[0] } },
    selection: {},
  }
);

  ```
  Nhìn vào trình duyệt của bạn và tìm kiếm bất cứ thứ gì không tồn tại trong bảng để xem trạng thái trống được phản ánh. Để biết thêm chi tiết về các cài đặt lọc bổ sung, hãy xem [collection hooks API documentation](https://cloudscape.design/get-started/dev-guides/collection-hooks/#api).


  {{%expand "Xem src/pages/flavor/components/flavors-table.tsx trông như thế nào sau bước này." %}}
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
    />
  );
}
```
  {{% /expand%}}

  {{%expand "Xem ảnh chụp màn hình của trang trông như thế nào sau bước này." %}}
![Preparation](/images/18.png?false&width=90pc)
{{% /expand%}}

#### Áp dụng mẫu trạng thái trống (optional)

Bây giờ phụ thuộc vào bạn, thêm trạng thái trống vào bảng. Mẹo: nó là một phần của thiết lập bộ lọc. Bạn có thể tìm thấy một ví dụ trong [documentation hooks collection](https://cloudscape.design/get-started/dev-guides/collection-hooks/#using-with-react).

Bạn có thể kiểm tra nó bằng cách đặt thuộc tính ``allItems`` của ``useCollection`` hook vào một mảng trống.

{{%expand "Xem src/pages/flavor/components/flavors-table.tsx trông như thế nào sau bước này." %}}
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
    />
  );
}
```
{{% /expand%}}
{{%expand "Xem ảnh chụp màn hình của trang trông như thế nào sau bước này." %}}
![Preparation](/images/19.png?false&width=90pc)
{{% /expand%}}
