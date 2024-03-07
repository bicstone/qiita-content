---
title: Reactのアクセシビリティ改善に役立つユニークIDの生成
tags:
  - プログラミング
  - React
  - アクセシビリティ
  - useId
  - testing-library
private: false
updated_at: "2024-02-24T17:03:34+09:00"
id: 1689b9a278d2939fdf35
organization_url_name: null
slide: false
ignorePublish: false
---

React 18に追加された新しいフック `useId` を活用することで、重複やハイドレーションエラーを起こさずにユニークなIDを生成する方法を解説します。

(React 19を使用しています)

※ これは2022/12/4に[個人ブログ](https://bicstone.me)で公開した記事を移植したものです。

## ユニーク ID の必要性

アクセシビリティを改善するためにHTML AttributeへIDを指定することがよくあります。

- 固定値を設定した場合、同じコンポーネントが複数レンダリングされた場合に、IDが重複してしまいます。
- 乱数にした場合、生成されたHTMLとハイドレーション時のIDが異なることにより整合性が取れないためパフォーマンスの問題が発生してしまいます。

## useId について

これらの問題を解決するために、React 18で新しいフック `useId` が登場しました。

https://ja.reactjs.org/docs/hooks-reference.html#useid

> useId はハイドレーション時の不整合を防ぎつつサーバとクライアント間で安定な一意 ID を作成するためのフックです。

## 使用方法

公式ドキュメントによると、1つのコンポーネントで複数のIDを使う場合、接尾辞を付けるよう記載がありました。

- labelタグのforでinputを指定する
- バリデーションエラーをWAI-ARIAで指定する

場合次のようになります。

```jsx
import { useId } from "react";

const PasswordInput = () => {
  const id = useId();
  const inputId = id + "_input";
  const errorMessageId = id + "_errorMessage";

  return (
    <>
      <label htmlFor={inputId}>パスワード</label>
      <input
        id={inputId}
        type="password"
        autoComplete="current-password"
        value={props.value}
        onChange={props.onChange}
        aria-invalid={props.hasError}
        aria-errormessage={props.errorMessageId}
      />
      {props.errorMessage !== undefined && (
        <span id={errorMessageId}>{errorMessage}</span>
      )}
    </>
  );
};
```

## 単体テストの書き方

恥ずかしながら今まで1つしかレンダリングされない前提で、IDを定数で実装していました。テストもその形で作っていたので動的なIDの場合に取得する方法がわからず、しばらくtesting-libraryのドキュメントを探し回っていました。

結果として、 [@testing-library/jest-dom](https://github.com/testing-library/jest-dom) の便利マッチャーを使うとシンプルで宣言的にテストを書くことが出来ました。

(jest 29, jest-dom 5を使用しています)

```js
describe("Password", () => {
  // ...
  describe("has error", () => {
    // ...
    test("has error message", () => {
      // ...
      expect(passwordInput).toBeInvalid();
      expect(passwordInput).toHaveErrorMessage(
        "ID またはパスワードが誤っています",
      );
    });
  });
});
```

## まとめ

- ID系のHTML Attributeを追加したい場合は `useId` を使ってみよう
- `@testing-library/jest-dom` 便利
