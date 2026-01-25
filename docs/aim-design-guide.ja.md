# AIM Design Guide

## 概要
- PlantUMLで表現するAIM(Action Impact Model)の構文・設計・運用方針を説明するドキュメント

---

## 1. パッケージ
### パッケージの概要
- 各パッケージは以下を内包する
    - UI要素
    - ユーザーのアクション

### ステレオタイプ
- `<<Page>>`, `<<Modal>>`, `<<Component>>`, など
    - 必要に応じて適宜追加・統合してよい

---

## 2. クラス
### ユーザーのアクション
- その画面でできることやユースケースを記述する
- `<<Action>>`ステレオタイプを使用する

### UI要素
- UI要素のステレオタイプには、例えば下記があるが、必要に応じて適宜追加・統合してよい
    - `<<Component>>`, `<<Button>>`, `<<TextBox>>`, `<<PullDown>>`, `<<Img>>`, `<<Text>>`, など

```plantuml
package "Settings Page" <<Page>> {
  class "Change user ID" <<Button>>          ' UI要素
  class "Update user ID" <<Action>>          ' アクション
  class "Header" <<Component>>               ' コンポーネント
}
```

---

## 3. 矢印
- `"Action" --> "UI Element" : {impact}`
    - `"Action"`を実行すると `"UI Element"`に影響`{impact}`(変更`{modify}`・削除`{delete}`・追加`{add}`・etc.)を及ぼす ことを表す
    - 影響を表すラベル部(`{impact}`)は、自明な場合は省略してよい (「ユーザーIDを変更する」を実行すると「ユーザーID」の表示内容が変更される場合など)
- `"Action" --> "UI Element" : {impact} {Child Element}`
    - あるUI要素の子要素(Overlayの内容など)にのみ影響を及ぼす場合は、子要素名をラベルに記載する
- `"Action" --> "UI Element" : if {condition} {impact} {Child Element}`
    - ある条件`condition`のもとでの影響を表す

---

## 4. UCODの対応付け
- AIMの各パッケージ・各クラスは、姉妹モデルである[UCOD](https://github.com/hansuoi/ucod)と相互変換できる関係にある

| UCOD | AIM |
| ---- | --- |
| Pageクラス | Pageパッケージ |
| Modalクラス | Modalパッケージ |
| Componentクラス | Componentパッケージ |
| Overlayクラス | 矢印のラベル |
| クラス内のUI要素 | UI要素クラス |
| クラス内のアクション | アクションクラス |
| 画面遷移の矢印 | (対応するもの無し) |
| (対応するもの無し) | 影響の矢印 |

---

## 5. プロダクトの変更による影響範囲に基づく自動テストコードの設計モデルとしてのAIM
- AIMは、アジャイル開発・インクリメンタル開発などにおける、プロダクトの変更による影響範囲を特定する際に有用である
- 特に、生成AIエージェントなどに、AIMとプロダクトコードの差分を与え、影響範囲の自動特定・テストケースの自動設計・自動テストコードの自動実装、といった用途を想定している
    - その際、AIMの姉妹モデルである[UCOD](https://github.com/hansuoi/ucod)も、インプットとして併せて使用することを、強く推奨する

### 実装例
- TypeScriptによるPlaywrightでのPOM(Page Object Model)実装を想定する

#### Page Objectの実装
- UCODに基づいて実装できる
    - 参照:
        - [UCOD Design Guide](https://github.com/hansuoi/ucod/blob/main/docs/ucod-design-guide.ja.md)
        - [プロンプト例](https://github.com/hansuoi/ucod/blob/main/prompts/generate-pom.md)

#### アサーションファイルの実装
- AIMの矢印に付けられているラベルをもとにアサーションを実装できる

#### テストコードの実装
- あるアクションからあるUI要素への矢印の経路自体がテストケースとなる
    - AIMの矢印は「(ある条件下で)あるユーザーのアクションが あるUI要素に影響を与える」ことを表すため