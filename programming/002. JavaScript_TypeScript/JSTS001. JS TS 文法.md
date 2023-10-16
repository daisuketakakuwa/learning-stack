# 文法
## Spread(...) Operator / スプレッド構文
✕配列にする。
〇複数の要素に分ける。
```js
// 文字列
const echo = (a, b, c) => console.log(`1: ${a}|2: ${b}|3:${c}`);
echo(..."ABC"); // "1: A|2: B|3:C" 

// 配列
const obj = [1,2,3,4,5]
console.log(...obj); // 1, 2, 3, 4, 5
```

## スプレッド構文 -> オブジェクトを複製する
```js
const obj1 = {
    name: "DAISUKE"
}
const obj2 = { ...obj1 }
obj2.name = "TAO";
console.log(obj1.name); // DAISUKE
console.log(obj2.name); // TAO
```

# 演算子 operator

## 【op1】== と ===
|||
|----|----|
|==|値を比較する。**異なる型の場合は自動的に型変換を行う。**|
|===|"同じ型"の値を比較する。**異なる型であれば常にfalseを返す。**|

NG
```js
// confirmDocument変数がany型
if (confirmDocument !== SURRENDER_IDENTIFICATION_LABEL.INSURANCE_POLICIES) {
```
OK
```js
// toString()でstringにしてOK
if (confirmDocument.toString() !== SURRENDER_IDENTIFICATION_LABEL.INSURANCE_POLICIES) {
```
**異なる型である場合、=== による比較は常に「false」を返す。**

## 【op2】new演算子
```ts
new constructor[([arguments])]
```
new Xxxxx()で、以下の２つを生成できる。
① ユーザー定義のオブジェクト ←**お前誰や。**
② コンストラクター関数を持つクラスのインスタンス ←**うんうん分かる。**

### ユーザ定義のオブジェクト
オブジェクトの名前とプロパティを指定する関数を書くことで、オブジェクトの型を定義します。 例えば、オブジェクト Foo を生成するコンストラクター関数は以下のようになります。
```js
// オブジェクトの名前: Foo
// プロパティ名を指定する: 引数で。
function Foo(bar1, bar2) {
  this.bar1 = bar1;
  this.bar2 = bar2;
}

const myFoo = new Foo('Bar 1', 2021);
```
