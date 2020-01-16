# dasshutuGame

1. view
  1. shadow
1. body

## 問題をフラグ管理
- 流し込むワードを辞書に
- フラグで条件分岐
- 記述をなるべく減らす


## js のオブジェクトとか、コンストラクタ、イニシャライザ(ズ)とか


多分、テンプレの箱作って、その中の要素を書き換えだから上手く機能すると、思うのよね

### 基本

> [オブジェクト : 基本 【 ja.javascript.info 】](https://ja.javascript.info/object)


#### 要素
- プロパティ
		- `key: value` のペア
				- `key` : 文字列(プロパティ名)
				- `value` : なんでも良し

空のオブジェクトは、以下

``` samlpe.js
let hoge = new Object(); // "オブジェクトコンストラクタ" 構文
let hoge = {};  // "オブジェクトリテラル" 構文
```



`{}` 宣言は、**オブジェクトリテラル** と呼ばれる



`hoge` オブジェクトが持っているプロパティ(フィールド？)の呼び出しは

    hoge.fuga


<small>スペースまたぎの `key` 表記方法は省略</small>


#### 省略して、挿入
実際のコードでは、既存の変数をプロパティ名の値として使用することがよくある

``` samlpe.js
function makeUser(name, age) {
  return {
    name: name,
    age: age
    // ...他のプロパティ
  };
}

let user = makeUser("John", 30);
alert(user.name); // John
```

プロパティは、変数と同じ名前だから省略ができる

``` samlpe.js
function makeUser(name, age) {
  return {
    name,  // name: name と同じ
    age    // age: age と同じ
    // ...他のプロパティ
  };
}

let user = makeUser("John", 30);
alert(user.name); // John
```

混ぜ込みもできる

``` samlpe.js
let user = {
  name,  // name:name と同じ
  age: 30
};


#### `for` ループで探索
js の`for` より、Python ぽいね🤗


``` samlpe.js
let user = {
  name: "John",
  age: 30,
  isAdmin: true
};

for(let key in user) {
  // keys
  alert( key );  // name, age, isAdmin
  // values for the keys
  alert( user[key] ); // John, 30, true
}
```

#### オブジェクトの順序付け
`"n": hoge`と数字をつけると、順序付けができる
  - 付けないと、記載順！

#### `const` オブジェクト

`const` 宣言したオブジェクトは、変更できる
  - 参照として処理してるから

``` samlpe.js
const user = {
  name: "John"
};

user.age = 25; // (*)
alert(user.age); // 25
```

`(*)` 行は、エラーは起きない
  - `user` への再代入ではないので
    - `.hoge` に入れてるから

`for` で回したり、以下の`Object.assign`関数 を使うことで、新規にオブジェクト作成が可能

``` samlpe.js
Object.assign(dest[, src1, src2, src3...])
```

オブジェクトのマージに使ったりするみたい

``` samlpe.js
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

// permissions1 and permissions2 のすべてのプロパティを user にコピー
Object.assign(user, permissions1, permissions2);

// now user = { name: "John", canView: true, canEdit: true }
```


