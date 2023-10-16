# JavaScript
- Javaのようにフィールド宣言する必要なし。
- コンストラクタ内で`this.xxxx = xxxxx`と定義すればプロパティ定義される。
```js
class BookJS {
    constructor(name, publishedYear) {
        this.name = name;
        this.publishedYear = publishedYear;
    }
    introduce() {
        return `NAME: ${this.name} PUBLISHED_YEAR: ${this.publishedYear}`;
    }
}

console.log(new BookJS("JSのBOOK", 2020).introduce());
```

# TypeScript
- JSと異なり**アクセス修飾子(public/private/readonly)** を扱う。<br>
　→ 何もつけないと`public`が自動的に付与される。
- JSと異なり、Javaみたいにフィールド宣言をしてあげる。
```ts
class BookTS1 {
    name: string;
    publishedYear: number;
    constructor(name: string, publishedYear: number) {
        this.name = name;
        this.publishedYear = publishedYear;
    }
    introduce() {
        return `NAME: ${this.name} PUBLISHED_YEAR: ${this.publishedYear}`;
    }
}

console.log(new BookJS("TS1のBOOK", 2022).introduce());
// デフォルトではpublicフィールドとなるので、下記のように直接参照可能。
console.log(new BookJS("TS1のBOOK", 2022).name);
console.log(new BookJS("TS1のBOOK", 2022).publishedYear);
```
**constructorの引数にpublic修飾子をつける＝プロパティ定義不要となる。**
```ts
// コンストラクタにpublic修飾子をつける＝プロパティ定義が不要となる＝JSと同じ感じになる。
class BookTS2 {
    constructor(public name: string, public publishedYear: number) {
        this.name = name;
        this.publishedYear = publishedYear;
    }
    introduce() {
        return `NAME: ${this.name} PUBLISHED_YEAR: ${this.publishedYear}`;
    }
}
console.log(new BookJS("TS2のBOOK", 2022).introduce());
```

# JS/TS共通するところ

1. constructorは1つしか定義できない。
2. 定義の仕方は「一般的なクラス宣言」「**変数に代入する**クラス式(有名/無名)」の２つ。
3. [自動的にstrictモードでコンパイルされる。](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Classes#strict_%E3%83%A2%E3%83%BC%E3%83%89)
4. getter/setter
5. 静的宣言(staticキーワード)

## クラス式
```ts
// 無名
let Rectangle1 = class {
  constructor(public height: number, public width: number) {
    this.height = height;
    this.width = width;
  }
};
const rec1 = new Rectangle1(10,20);
console.log(`width: ${rec1.width} height: ${rec1.height}`);

// 名前つき
let Rectangle2 = class Rectangle2 {
  constructor(public height: number, public width: number) {
    this.height = height;
    this.width = width;
  }
};
const rec2 = new Rectangle2(100,200);
console.log(`width: ${rec2.width} height: ${rec1.height}`);
```
## getter/setter
・オブジェクトと同様。
```ts

class Person {
    private name: string;

    constructor(name: string) {
        this.name = name;
    }

    get getName() {
        return this.name;
    }

    set setName(name: string) {
        this.name = name;
    }
}

const p1 = new Person("TARO");
// call getter
console.log(p1.getName);
// call setter
p1.setName = "JIRO";
console.log(p1.getName);
```

## 静的宣言(staticキーワード)
- クラスをインスタンス化せずに呼ばれるフィールド/メソッド。
- インスタンスを介して呼べないフィールド/メソッド。
- **インスタンス間で複製する必要のないもの(axiosインスタンス/utility関数)にGOOD。**
```js
class HogeUtils {
  static add(num1, num2) {
    return num1 + num2;
  }
}

console.log(HogeUtils.add(1,2));
```

# 参考

https://zenn.dev/kimura141899/articles/60bd0bc399296c#private%2Cpublic%2Creadonly%E4%BF%AE%E9%A3%BE%E5%AD%90
