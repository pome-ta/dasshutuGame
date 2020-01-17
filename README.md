# dasshutuGame

1. view
	  1. shadow
1. body










## 問題をフラグ管理
- 流し込むワードを辞書に
- フラグで条件分岐
- 記述をなるべく減らす

### 要素整理

#### 変わるもの
- 問題No.
- 出題
	- ヒント(あれば)
- 正誤判断


#### 変わったあと
- ネストは嫌だな


## js のオブジェクトとか、コンストラクタ、イニシャライザ(ズ)とか


多分、テンプレの箱作って、その中の要素を書き換えだから上手く機能すると、思うのよね

### [オブジェクト 【 ja.javascript.info 】](https://ja.javascript.info/object)


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
```

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

### [ガベージコレクション 【 ja.javascript.info 】](https://ja.javascript.info/garbage-collection)
省略

### [オブジェクトメソッド, `this` 【 ja.javascript.info 】](https://ja.javascript.info/object-methods)


ちょー単純には、Python の`self` の認識かな？🤗


オブジェクト指定を、直接名指しでもええけど別変数名になった時に死ぬ ☠️


同じ関数でも、違う`this` を呼び出す可能性がある

``` sample.js
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert( this.name );
}

// 2つのオブジェクトで同じ関数を使う
user.f = sayHi;
admin.f = sayHi;

// これらの呼び出しは異なる this を持ちます
// 関数の中の "this" は "ドット" の前のオブジェクトです
user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin['f'](); // Admin (ドットでも角括弧でも問題なくメソッドにアクセスできます)
```


#### `this` を見逃す可能性

``` sample.js
let user = {
  name: "John",
  hi() { alert(this.name); },
  bye() { alert("Bye"); }
};

user.hi(); // John (シンプルな呼び出しは動作します)

// 今、name に応じて user.hi または user.bye を読んでみましょう
(user.name == "John" ? user.hi : user.bye)(); // Error!
```

これは、動く 🙆‍♂️

``` sample.js
user.hi();
```

これは、ダメ🙅‍♂️ (評価されたメソッド)

``` sample.js
(user.name == "John" ? user.hi : user.bye)(); // Error!
```

##### 評価後に、`this` を見失わないためには

`obj.method()` 呼び出される機能の理解が大事

1. `.` が、`obj.method` を抽出
1. `()` で、'それ' を実行

つまり

``` sample.js
let hi = user.hi;
hi(); // Error, this は undefined なので
```

<small>無理やり`()` つけたら、動いたぽいけども、ダメなんかな？ 🤔</small>


##### アロー関数は、`this` を持たない

外部の`this` とか、呼び出せるんだって 🤗 知らんけど



### [コンストラクタ 演算子 `new` 【 ja.javascript.info 】](https://ja.javascript.info/constructor-new)


コンストラクタ関数

  1. 先頭大文字で名付け
  1. `new` 演算子を使い実行


``` sample.js
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

let user = new User("Jack");

alert(user.name); // Jack
alert(user.isAdmin); // false
```

`new User(...)` が呼び出された時の動き
1. 新しい空のオブジェクトが作られ、`this` に代入
1. 関数本体を実行
    - 通常は
        - `this` を変更
        - 新しいプロパティを追加
1. `this` の値を返却


``` sample.js
function User(name) {
  // this = {};  (暗黙)

  // this へプロパティを追加
  this.name = name;
  this.isAdmin = false;

  // return this;  (暗黙)
}
```



