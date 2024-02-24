---
title: AG-Gridでヘッダーの下にアクションエリアを表示する方法
tags:
  - JavaScript
  - プログラミング
  - TypeScript
  - React
  - ag-grid
private: false
updated_at: ""
id: null
organization_url_name: null
slide: false
ignorePublish: false
---

AG-Gridでテーブルのヘッダー行の下にアクションができる固定行を表示する方法を解説します。

(React版v28を使用しています。)

※ これは2022/12/13に個人ブログで公開した記事を移植したものです。

## ヘッダー行の下にアクションができる行とは？

正式な名称がわからないので説明的ですが、次のようなUIです。 (もし名前ご存知でしたら教えてください)

![Gmail のスクリーンショット。メールを選択すると、テーブル内の最上部に「このメッセージ内のメッセージ50件すべてが選択されています。」という行(セル)が増える](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/684999/0c5a490d-38b2-9f65-d3cd-d4a029270312.png)

AG-Gridではこの機能は搭載されていませんでした。そこで、AG-Gridの様々な機能を活用して無理やり実装してみました。

https://www.ag-grid.com/react-data-grid/full-width-rows/#understanding-full-width

https://www.ag-grid.com/react-data-grid/row-pinning/

https://www.ag-grid.com/react-data-grid/row-selection/

## サンプル

どれか行を選択すると、上から件数と選択されたIDを表示するセルが表示されます。

(個人的には選択するとすべての行が動いてしまうので避けたい実装ではありますが、Gmailと挙動を合わせてみました)

https://codesandbox.io/embed/ag-grid-action-row-mmssuf?&theme=light&view=preview

```tsx
import { AgGridReact } from "ag-grid-react";
import { useCallback, useMemo } from "react";

import type {
  ColDef,
  IsFullWidthRowParams,
  RowHeightParams,
  RowSelectedEvent,
} from "ag-grid-community";

type Data = { id: number; name: string };

// 表示するコンポーネント
const FullWidthCell = ({ api }: IsFullWidthRowParams) => {
  const length = useMemo(() => api.getSelectedRows().length, [api]);
  const selectedIds = useMemo(
    () => api.getSelectedRows().map((v) => v.id),
    [api],
  );

  return (
    <div
      style={{
        borderBottom: `1px solid #eee`,
        width: "100%",
        height: "100%",
        paddingLeft: 24,
      }}
    >
      {length}件を選択中。 {selectedIds.join(",")}
    </div>
  );
};

export const DataGrid = (): JSX.Element => {
  const rowData: Array<Data> = [
    { id: 1, name: "Item 1" },
    { id: 2, name: "Item 2" },
    { id: 3, name: "Item 3" },
    { id: 4, name: "Item 4" },
    { id: 5, name: "Item 5" },
  ];

  const columnDefs: ColDef<Data>[] = [
    {
      field: "check",
      headerCheckboxSelection: true,
      checkboxSelection: true,
    },
    { field: "id" },
    { field: "name" },
  ];

  // 上部に固定
  const isFullWidthRow = useCallback(
    (params: IsFullWidthRowParams) => params.rowNode.rowPinned === "top",
    [],
  );

  // 高さを固定
  const getRowHeight = useCallback(
    (params: RowHeightParams) => (params.node.rowPinned ? 40 : undefined),
    [],
  );

  // 選択時に有効を切り替え
  const onRowSelected = useCallback((event: RowSelectedEvent) => {
    event.api.setPinnedTopRowData(
      event.api.getSelectedRows().length > 0 ? [true] : undefined,
    );
  }, []);

  return (
    <AgGridReact<Data>
      rowData={rowData}
      columnDefs={columnDefs}
      fullWidthCellRenderer={FullWidthCell}
      isFullWidthRow={isFullWidthRow}
      getRowHeight={getRowHeight}
      onRowSelected={onRowSelected}
      domLayout="autoHeight"
      rowSelection="multiple"
      rowMultiSelectWithClick
    />
  );
};
```

## まとめ

AG-Gridでヘッダーの下にアクションエリアを表示できました。なんとなく辛みがあるので、AG-Gridに実装してほしいです。
