# 名前空間 と namespace宣言
「名前空間」はJavaScript/ECMAScriptの**概念**。
「namespace宣言」はTypeScript固有の**文法**。

「**namespace宣言**」**で**「**名前空間**」**を作成する** という関係性である。

## 名前空間とは
・グローバルスコープに定義したvar変数に「**複数の関連する値がまとめられたオブジェクト**」を代入する。**このvar変数/windowオブジェクトに追加された1プロパティが「名前空間」である。**<br><br>
・グローバルスコープに定義したvar変数は、windowオブジェクトのプロパティとなるため、どこからでも参照できるもの、すなわち一意なグローバル変数となる。
```js
//　グローバルスコープにてvar変数を定義
var MYFUNCTIONS = {
    addition: function(num1,num2){
        return num1+num2;
    },

    multiplication: function(num1,num2){
        return num1*num2;
    }
} 

// 名前空間(windowオブジェクトの変数)である MYFUNCTIONS を参照
var operation = MYFUNCTIONS.addition(5,10);
console.log(operation)
```

https://www.sejuku.net/blog/65850

名前空間 = 複数の関連する値がまとめられたオブジェクト

であることを踏まえると、

**モジュールも名前空間である。**
```ts
import * as hoge from "./hoge";
// hogeモジュールがexportしているものが、hogeオブジェクト(名前空間)内にはいっている。
hoge.hello();
hoge.world();
```

※ `import * as ns`は名前空間IMPORTとも呼ばれる。ns.XXXX で参照できるから。

https://zenn.dev/qnighy/articles/9c4ce0f1b68350#%E5%90%8D%E5%89%8D%E7%A9%BA%E9%96%93%E3%81%A8namespace%E5%AE%A3%E8%A8%80

## namespace宣言とは
JavaScriptでは`グローバルスコープでvar変数を定義する`ことで名前空間を作成したが、TypeScriptでは**TypeScript特有のnamespace宣言を利用して**、名前空間(複数の関連する値がまとめられたオブジェクト)を作成できる。
```ts
// module Foo でもよい。moduleもnamespaceも結局「複数の関連する値がまとめられたオブジェクト」を生成するのだから。
namespace Foo {
  export const x = 42;
  export type T = number;
}

// 名前空間からのインポートには専用構文がある
import V = Foo.T;
```

# モジュールとは？

モジュールとは
- **export キーワードを含む .ts ファイル**
- モジュールは**名前空間**(**複数の関連する値がまとめられたオブジェクト**)である。
- `declare module`で定義したもの

## モジュールIMPORT/EXPORT方法

```ts
export interface MyInterface {
  name: string;
}

export class MyClass implements MyInterface {
  constructor(public name: string) {}
}
```
をIMPORTする。

個別にIMPORT
```ts
import { MyInterface, MyClass } from './lib/mylib';

const obj: MyInterface = new MyClass('Maku');
console.log(obj.name);  //=> Maku
```

まとめてIMPORT
```ts
import * as mylib from './lib/mylib';

const obj: mylib.MyInterface = new mylib.MyClass('Maku');
console.log(obj.name);  //=> Maku
```

>ここでは、as キーワード を使って mylib という変数のプロパティ経由で公開されているインタフェースにアクセスできるようにしています。 ワイルドカードを使用する場合、この変数の指定は必須 であることに注意してください（それにより、不注意による名前の衝突を防げるようになっています）。

https://maku.blog/p/fbu8k8j/

declare moduleを使ってモジュールを作成する場合
```ts
// export module
declare module "url" {
  export interface Url {
    protocol?: string;
    hostname?: string;
    pathname?: string;
  }
  export function parse(
    urlStr: string,
    parseQueryString?,
    slashesDenoteHost?
  ): Url;
}

// import module
import * as URL from "url";
let myUrl = URL.parse("https://www.typescriptlang.org");
```

https://www.typescriptlang.org/docs/handbook/modules.html#ambient-modules