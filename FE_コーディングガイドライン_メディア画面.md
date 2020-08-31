## 前提
- [must] は絶対
- [want] はできれば。なので任意

## ファイル・ディレクトリ構造
### 共通
- [must] ページ共通の資材は`common`に格納する

### ページ毎
- [must] そのページだけにしか使わない資材は`views`に格納する

### 粒度
- [want] 1関数 - 1ファイル
    - テストを意識するとそうなるはず
    - 切り出したファイルを他でも必要となった際に、ディレクトリ移動だけで済むというメリットもある

### ファイル分割時
- [want] `hoge/index.ts(x)` 分割時、
    - `hoge/index.ts(x)` 
    - `hoge/modules/fuga/index.ts(x)`
とする
	- `hoge/fuga`, `hoge/piyo` … と分割後のファイルが増えた際に `hoge` 直下が見づらくなるため

### ディレクトリ名
- [must] 関数は `lower camel`, class や component, 定数は `upper camel`
- [want] `hoge.ts(x)` という命名は禁止。 `hoge/index.ts(x)` とする
    - ファイルの量がかさみ、あとからファイル分割する際に `hoge.ts`, `hoge-fuga.ts` といったファイルが増えると、ディレクトリ直下がみづらくなるため

## コード内
- [want] node_modules とそれ以外の import が入り乱れると、 `import-oder@eslint` で怒られるため、明確に node_modules とそれ以外がわかるように、コメントでブロック分けする
- [want] import 群と実装そのものも明確に区分けする

``` ts
// sample
// import node_modules
import React from "react"

// components
import { Button } from "./**/Button"

// modules
import { getMessage } from "./**/getMessage"

// import others
import { Const } from "./**/Const"

// main
...
```

- [want] 関数定義は極力関数式を使用する
    - 関数の型が定義されている・定義する場合、関数宣言だと適用できないため
- [want] コードから実装の意図等が読み取れない場合はコメントを残す
    - https://twitter.com/t_wada/status/904916106153828352

### export
- [must] default は基本禁止
    - [理由](https://typescript-jp.gitbook.io/deep-dive/main-1/defaultisbad)
    - 例外：default export でないとだめな場合
        - 例：next.js の pages 配下の component

### 型関連
- [must] 配列型は `string[]` を使用
- [want] `any` は極力使わない。使う場合はコメント必須
    - `any` であるべき時もあるので、 TODO コメントはマストとしない
- [want] 型宣言時に `type`, `interface` どちらでも定義できる場合は `type` を使用

### 定数
- [must] カラーコードは全て定数管理とする
    - [todo] 管理するファイルが決まったらパス記述
- [must] 定数内の変数は `upper camel`, プロパティは全て大文字
- [want] オブジェクト定数の場合は `const assertion` する

```ts
// sample
const COLOR_3675EF = "#3675ef"
const COLOR_DADDE3 = "#dadde3"
const COLOR_FFFFFF = "#fff"

export const Colors = {
  COLOR_3675EF,
  COLOR_DADDE3,
  COLOR_FFFFFF,
} as const

```

### 関数
- [want] 関数の引数は分割代入しない
  - インラインで分割代入後1つの変数しか使わない。等例外あり

### ディレクトリ
フロントエンド資材は `media/frontend` に入れる。
出力先は `media/public/assets` である。

### HTML(erb)
- [must] Javascriptの起点に利用するclassは接頭辞に`js-`をつける

### SCSS
- [must] MindBEMding
- [must] 接頭辞ルール
  - `atoms` は `a-`
  - `molecules` は `m-`
  - `organisms` は `o-`
  - `pages` は `p-${page_name}`
  - `templates` は `t-`