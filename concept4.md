## クラスの継承

さて、実際の料理では料理の分野によって異なる材料を作ることがあります。イタリア料理ではパスタをパスタマシーンで作りますが、和食ではパスタは使いません。こうした違いを表すために継承という概念を使うことが出来ます。例えば`chef`クラスを継承した`italianChef`というクラスは`extends`というキーワードを用いて以下のように書くことが出来ます。

```javascript
class italianChef extends Chef {}
```

### super

さて、上記でchefクラスは、italianChefクラスの親クラスと呼びます。逆にitalianChefクラスはchefクラスの子クラスです。クラスの継承では、この親クラスのプロパティやメソッドにアクセスするために`super`というキーワードを利用します。

```javascript

class ItalianChef extends Chef {
  constructor(props) {
    super(props); // => chefクラスのコンストラクターを呼び出す。
  }
}

const chef1 = Chef.create({
  experty: "イタリア料理",
  name: "務 落合",
  repertoire: ["ボロネーゼ", "カルボナーラ", "ミネストローネ"]
});

chef1.cook("ボロネーゼ"); // => 親クラスのcookメソッドが呼び出される。

```

### 子クラスにメソッドを追加する。

小クラスに親クラスとは別のメソッドを追加することも出来ます。例えば、イタリアンシェフはパスタを練る必要があるのでそのためのメソッドを作ります。

```javascript

class ItalianChef extends Chef {
  constructor(props) {
    super(props); // => chefクラスのコンストラクターを呼び出す。
    this.pastaCount = 0;
  }
  preparePasta() {
    // パスタ生地を作る。
    this.pastaCount += 1; // パスタ生地の数を増やす。
  }
}

chef1.preparePasta(); // => ラザニア生地を作る。
```


### 親クラスのメソッドを書き換える(オーバーライド)

また子クラスでは、親クラスのメソッドを書き換えることが出来ます。例えば、cookファンクションを呼び出した時に、作り出す前にパスタの残り数をチェックして、残っていれば料理を作り、作った後にパスタ生地の数を減らすようにしましょう。すると以下のように書けます。

```javascript
class ItalianChef extends Chef {
  constructor(props) {
    super(props); // => chefクラスのコンストラクターを呼び出す。
    this.pastaCount = 0;
  }
  preparePasta() {
    // パスタ生地を作る。
    this.pastaCount += 1; // パスタ生地の数を増やす。
  }
  cook(name) {
    if (this.pastaCount === 0) {
      console.log("パスタ生地がありません!");
    } else {
      super.cook(name);
      this.pastaCount -= 1;
    }
  }
}

chef1.cook("ボロネーゼ"); // => パスタ生地がありません!
```

## mixins

classは多重継承を行うことが出来ません。これはどういうことかというと、1つのクラスが2つ以上のクラスを親に持つことは出来ないということです。しかし、時には複数の異なる機能を持たせたい場合もあるかと思います。こうした際にはmixinパターンを使うと便利です。

例えばですが、ChefオブジェクトをJSONで返すためのモジュールと、シェフ、サービススタッフ両方に適用可能な評価の仕組みを持つモジュールを組み合わせたいとしましょう。

すると以下のように書くことが出来ます。


```javascript
const serializeMixin = (ParentClass) => {
  return class extends ParentClass {
    /* constructorを書かない場合、
    constructor() {
      super();
    }
    が、呼び出されます。
    */
    serialize() {
      return JSON.stringify(this);
    }
  }
}

const ratingMixin = (ParentClass) => {
  class extends ParentClass {
    constructor() {
      super();
      this.ratings = [];
    }
    addRating(rating) {
      this.ratings.push(rating);
    }
  }
}

class Chef {
  ...
}

class ItalianChef extends serializeMixin(ratingMixin(Chef)) {
  ...
}

const chef1 = ItalianChef.create({
  experty: "イタリア料理",
  name: "務 落合",
  repertoire: ["ボロネーゼ", "カルボナーラ", "ミネストローネ"]
});

chef1.addRating({
  rate: 5,
  message: "とても美味しいパスタでした。"
})
```

上記で、class名を書かずに、

```javascript
return class extends ParentClass
```

としていることに注目してみて下さい。無名ファンクションと同様に、classも名前を付けずに書くことが出来ます。また詳しくは説明しませんが、classは内部では即時関数を返すため、functionと同様に変数内に格納することが出来、ファンクションの引数としても使うことが出来ます。

classがfunctionを返すことは以下のようにして確認出来ます。

```javascript
class Foo {}

console.log(typeof Foo); // => "function"
```

このようにmixinパターンを使うことで複数のクラスを一つのクラスに継承することが出来ます。

## Prototype

ES6のClassは厳密にはES5におけるprototypeを利用した、オブジェクト作成パターンをオブジェクト指向言語のように書けるようにしたものです。ES5でのインスタンス生成にはいくつかのパターンがありますが、ここではConstructorパターンというパターンを通してprototypeの概念を説明します。

```javascript
function Chef(params) {
  this.experty = props.experty;
  this.repertoire = props.repertoire || [];
  this.name = props.name;
}

Chef.prototype.cook = function(name) {
  // 料理を作る
}

const tonio = new Chef({
  experty: "イタリア料理",
  repertoire: ['カプレーゼ'、`プッタネスカ`、`小羊背肉のリンゴソースかけ`、`プリン`],
  name: "トニオ トラサルディー"
});
Chef.cook("カプレーゼ");
```

上記を見て気づいたかと思いますが、cookファンクションを定義する時にChefファンクションに直接定義するのではなく、Chef.prototype.cookという風にprototypeを挟んで定義しています。このprototypeは、一部のビルトインファンクションを除く全てのファンクションがデフォルトで持つプロパティです。ファンクションを用いてインスタンスを作成する際には、このprototypeに定義されているプロパティが共有されます。Chefファンクションに直接、cookファンクションを定義した場合、インスタンス化したオブジェクトにはcookファンクションは共有されないため、prototypeを介す必要があるのです。

prototypeについてはそれだけについて書かれた本もあるくらいなので、ここではより詳しい説明は省きます。Javascriptにより慣れ親しんだ段階で、prototypeについてより理解を深めたいという場面が出てくるかと思いますので、その時に改めて勉強することを推奨します。

## 更に学ぼう

### 記事で学ぶ

- [クラス - Mozilla](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Classes)
