---
title : "Prepare the column definitions"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b>3.1.</b> "
---
We can now define what each column in the table will display by setting column definitions with the following steps:

1. Open ``src/pages/flavors/components/flavors-table.tsx`` in your IDE. This custom component is already embedded in the flavors page inside ``src/pages/flavors/app.tsx``. It receives the data that we want to display in the table as a property.
2. To get a sense about the data we'll display, take a look at the ``Flavor`` interface in src/pages/flavors/data.ts. We use mock data for the workshop. In a real world application the data would be obtained by fetching it from a server.
3. Based on this interface, create the column definitions inside ``src/pages/flavors/components/flavors-table.tsx``. Follow the given structure from the [table API documentation](https://cloudscape.design/components/table/?tabId=api) 
4. Import the TableProps interface using ``import { TableProps } from '@cloudscape-design/components/table';``. Here's the columnDefinitions we're using placed above the existing VariationTable component:
    ```
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

    ```
    