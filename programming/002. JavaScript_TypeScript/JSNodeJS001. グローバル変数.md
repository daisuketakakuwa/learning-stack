# グローバル変数
文字通り、グローバルに(どこからでも)参照できる変数のことである。
グローバル変数は**グローバルオブジェクト**に格納される。
## グローバルオブジェクト
|実行環境|グローバルオブジェクト名|
|---|---|
|ブラウザJavaScript|window か globalThis|
|NodeJS|global か globalThis|

両環境で利用される可能性があるコンテキストの場合は`globalThis`を使った方が良さそう。

https://ja.javascript.info/global-object

https://qiita.com/uhyo/items/f3b6feef9444e86bef94

## グローバル変数の定義方法
プログラム種別(Script/Module)で、グローバル変数の定義の仕方が変わる。
|プログラム種別|グローバル変数の定義方法|
|---|---|
|Script|グローバルスコープでvar変数/function等を定義|
|Module|**型定義ファイル(.d.ts)以外の場所で`declare global`宣言**<br>例) ReactだったらルートのApp.tsxに定義すればOK|

Scriptでグローバル変数を追加する場合
```ts
var counter = 0;
console.log(window.counter);
```

Moduleでグローバル変数を追加する場合
```ts
declare global {
  interface Window {
    config: Config;
  }
}

// App.jsでconfigオブジェクトに設定を格納
window.config = new Config({-----------});

// 各コンポーネントでConfigを参照したい場合はグローバル変数を参照すればOK
```

https://zenn.dev/qnighy/articles/9c4ce0f1b68350#%E5%90%8D%E5%89%8D%E7%A9%BA%E9%96%93%E3%81%A8namespace%E5%AE%A3%E8%A8%80