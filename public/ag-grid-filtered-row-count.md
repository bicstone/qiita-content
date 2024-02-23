---
title: AG-Gridでフィルターされた後の件数をカウントする方法
tags:
  - JavaScript
  - プログラミング
  - TypeScript
  - React
  - AG-Grid
private: true
updated_at: null
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

AG-Gridで、フィルター後の件数を取得する方法を解説します。

(React版v28を使用しています。)

## フィルター後の件数を取得する関数がない

AG-Gridにおいて、フィルター後の件数を取得する関数は用意されていないみたいです。

一方、公式デモでは選択中の件数を表示していたのでどのように行なっているのかソースコードを見てみました。

enterprise版で用意されているStatus barの実装では、カウンターを用意し、 `gridApi.forEachNodeAfterFilter()` で回してカウントしていく方法が取られていました。 (ライセンスの都合上引用が出来ないので下記ソースコードをご確認ください。)

https://github.com/ag-grid/ag-grid/blob/fc77919c164fb5bcd66a8b2897b20c318f5dd58e/enterprise-modules/status-bar/src/statusBar/providedPanels/filteredRowsComp.ts#L52-L61

フィルター後のNode一覧を取得しゴリゴリ足していく方法しか無さそうです。

## サンプル

Status barの実装を参考に実装してみました。

https://codesandbox.io/embed/ag-grid-filtered-row-count-s0nocs?&theme=light&view=preview

```tsx
import { AgGridReact } from "ag-grid-react";
import { useCallback, useState } from "react";

import type {
  AgGridEvent,
  ColDef,
  ModelupdateDateEvent,
} from "ag-grid-community";

type Data = { id: number; name: string };

export const DataGrid = (): JSX.Element => {
  const rowData: Array<Data> = [
    { id: 1, name: "Item 1" },
    { id: 2, name: "Item 2" },
    { id: 3, name: "Item 3" },
    { id: 4, name: "Item 4" },
    { id: 5, name: "Item 5" },
  ];

  const columnDefs: ColDef<Data>[] = [
    { field: "id", filter: true, floatingFilter: true },
    { field: "name", filter: true, floatingFilter: true },
  ];

  const [filteredRowCount, setFilteredRowCount] = useState<number>(0);

  const onFilterChanged = useCallback((event: AgGridEvent): void => {
    let filteredRowCount = 0;
    event.api.forEachNodeAfterFilter(() => {
      filteredRowCount++;
    });
    setFilteredRowCount(filteredRowCount);
  }, []);

  const onModelupdateDate = useCallback(
    (event: ModelupdateDateEvent): void => {
      onFilterChanged(event);
    },
    [onFilterChanged],
  );

  return (
    <>
      <AgGridReact<Data>
        rowData={rowData}
        columnDefs={columnDefs}
        onFilterChanged={onFilterChanged}
        onModelupdateDate={onModelupdateDate}
        domLayout="autoHeight"
      />
      count: {filteredRowCount}
    </>
  );
};
```

## まとめ

AG-Gridでフィルター後の件数を取得できました。なんとなく辛みがあるので、AG-Gridに実装してほしいです。

:::note
この記事は2022/12/12に個人ブログで公開した記事を転載したものです。
:::
