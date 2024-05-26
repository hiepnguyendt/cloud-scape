---
title : "Chuẩn bị định nghĩa cột"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b>3.1.</b> "
---
Bây giờ chúng ta có thể xác định những gì mỗi cột trong bảng sẽ hiển thị bằng cách thiết lập định nghĩa cột với các bước sau:

1. Mở ``src/pages/flavours/components/flavors-table.tsx`` trong IDE của bạn. Thành phần tùy chỉnh này đã được nhúng vào trang bên trong ``src/pages/flavors/app.tsx``. Nó nhận được dữ liệu mà chúng ta muốn hiển thị trong bảng như một thuộc tính.
2. Để hiểu được dữ liệu chúng tôi sẽ hiển thị, hãy xem giao diện ``Flavor`` trong ``src/pages/flavors/data.ts``. Chúng tôi sử dụng dữ liệu tạm cho workshop. Trong một ứng dụng thực, dữ liệu sẽ được lấy từ một máy chủ.
3. Dựa trên giao diện này, tạo ra các định nghĩa cột bên trong ``src/pages/flavor/components/flavors-table.tsx``. Thực hiện theo cấu trúc từ [table API documentation](https://cloudscape.design/components/table/?tabId=api) 
4. Nhập giao diện TableProps bằng cách sử dụng ``import { Tableprops } from '@cloudscape-design/components/table';``. Đây là columnDefinitions chúng tôi đang sử dụng đặt trên thành phần VariationTable hiện có:
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
    