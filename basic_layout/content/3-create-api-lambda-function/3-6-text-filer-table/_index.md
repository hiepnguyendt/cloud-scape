---
title : "Add a text filter to the table"
date :  "`r Sys.Date()`" 
weight : 7 
chapter : false
pre : " <b>3.6.</b> "
---
Searching for names manually in this amount of data is time-consuming. Let's fix that by adding a text ``filter`` to the table. We do this through the table component's filter property. We add the [text filter component](https://cloudscape.design/components/text-filter/?tabId=playground)  to the table and wire it with the collection hooks' filterProps. In addition, we set the ``filteringPlaceholder`` and the ``countText`` property. The ``filteringPlaceholder`` is used as placeholder for the filtering input and the ``countText`` is a human-readable, localized string that indicates the number of results.

By adding text filter component, we introduce a new state on the table. What do we display if there aren't any matches? Remember the table view pattern you looked at when we started step 2? Right, it mentions the [empty state pattern](https://cloudscape.design/patterns/general/empty-states/). Take a few minutes and make yourself familiar. We'll use this pattern with a slightly different wording to reflect the ``noMatch`` state, which is displayed when the filtering operation doesn't return any matches.

To add this pattern, let's create a custom component in ``src/pages/flavors/components/flavors-table.tsx`` which builds the UI:

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

  Take a look in your browser and search for anything that doesn't exist in the table to see the empty state reflected. For more details about additional filtering settings, refer to the [collection hooks API documentation](https://cloudscape.design/get-started/dev-guides/collection-hooks/#api).

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

  {{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/18.png?false&width=90pc)
{{% /expand%}}

#### Apply the empty state pattern (optional)

Now it's up to you, add the empty state to the table. Hint: it's part of the collection hooks filter setting. You can find an example in the [collection hooks documentation](https://cloudscape.design/get-started/dev-guides/collection-hooks/#using-with-react).

You can test it by setting the ``allItems`` property of the ``useCollection`` hook to an empty array.

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
{{%expand "See the screenshot of how the page looks like after this step." %}}
![Preparation](/images/19.png?false&width=90pc)
{{% /expand%}}
