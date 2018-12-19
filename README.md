# レッスン4. オブジェクト指向Javascript

## 目的

- オブジェクト指向の概念を理解する。
- クラスを利用してプログラムを書けるようになる。

## オブジェクト指向とは

オブジェクト指向は、簡単にいうとプログラム内であるモノ(オブジェクト)と、そのモノが持つプロパティと機能を一緒に定義し、より再利用性と可読性が高いコードを書くためのプログラミングの一つの手法であり考え方です。

プログラミング言語は多数あるのですが、オブジェクト指向型と呼ばれる言語もあれば、そうでない言語もあります。Javascriptは厳密にはオブジェクト指向型の言語ではありません。しかし、ES6でClassという概念が導入されたことでオブジェクト指向型言語に近い形でプログラムを書けるようになりました。

## Class

上記の説明では難しく感じるかと思うので、簡単な例で考えてみましょう。ここでは、レストランで働くシェフというオブジェクトについて考えてみましょう。

シェフ(Chef)というオブジェクトのことを表現するには、クラスをまず定義します。以下がそのためのシンタックスです。これを**クラス宣言**と呼びます。

```javascript
class Chef {}
```

さて、シェフが持つ属性にはどういうものがあるでしょう？様々なものが考えられますが、ここでは簡単に名前、専門分野、レパートリーの3つで考えてみましょう。これらのプロパティの値を定義するためにClassでは、constructorという特別なファンクションを用意しています。

```javascript
class Chef {
  constructor(props) {
    this.experty = props.experty
    this.repertoire = props.repertoire || []
    this.name = props.name
  }
}
```

## インスタンス化

さて、クラス宣言でシェフという概念を表現することが出来ました。クラスがあると、そこから実際のシェフを作ることが出来ます。これをインスタンス化と呼びます。インスタンス化のための書き方はオブジェクト生成とほぼ同じです。

```javascript
const tonio = new Chef({
  experty: "イタリア料理",
  repertoire: ['カプレーゼ', `プッタネスカ`, `小羊背肉のリンゴソースかけ`, `プリン`],
  name: "トニオ トラサルディー"
});
```

これでトニオさんという、シェフを作り出すことが出来ました。

## インスタンスメソッド

さて、シェフを作り出すことは出来ましたが今のままでは、何もすることが出来ません。そこで、シェフが料理を出来るようにしましょう。そのためにはインスタンスメソッドを定義します。ここではcookというメソッドを定義します。

```javascript
class Chef {
  constructor(props) {...}
  cook(dishName) {
    // 料理を作る
    const checkRepertoire = this.repertoire.find((name) => {
      return name === dishName;
    })
    if (typeof checkRepertoire !== "undefined") {
      return this[`_${dishName}`]; // 動的に異なるファンクションを呼び出す。
    } else {
      console.log("レパートリーにない料理です。")
    }
  }
  _cookCaprese() {
    // カプレーゼを作って、出来たものを返す。
  }
  _cookPuttanesca() {
    // プッタネスカを作って、出来たものを返す。
  }
  ...
}
```

これで料理を作れるようになりました。例えば以下のようにして、トニオさんにカプレーゼを作ってもらうことが出来ます。

```javascript
tonio.cook("カプレーゼ"); // => カプレーゼが返ってくる。
tonio.cook("肉じゃが"); // => レパートリーにない料理です。
```

## プライベートメソッドとカプセル化

ここで、cookから動的に_cookCaprese()というメソッドを呼び出すようにしていることに気づいたでしょうか？ なぜcookCaprese()というメソッドを作ってそれを直接呼び出さないのでしょう？ これは、それをするためには、オーダーする側がシェフのレパートリーを全て知っていないといけないからです。それでは、難しいのでcookという一つのメソッドだけを知っていればオーダーを出来るようにしています。

Javascriptではプライベートという概念がないのですが、他のオブジェクト指向型言語では通常`_cookCaprese`のようなメソッドをプライベートなメソッドと呼びます。javascriptの場合は慣例的にアンダースコア`_`を最初に付けることでプライベートなメソッドを表現します。こうすることで、シェフは外部に漏らしたくない具体的な料理の作り方を隠すことが出来ます。

このようにクラス外部からアクセスする際に知る必要のないプロパティやメソッドを隠すことをオブジェクト指向では、カプセル化と呼びます。プロパティについても、慣習に従って`_`を利用してプライベートプロパティのように表現することができます。

```javascript
class Chef {
  constructor(props) {
    this._experty = props.experty
    this._repertoire = props.repertoire || []
    this._name = props.name
  }
  ...
}
```

## getterメソッド

Javascriptにはgetterとsetterメソッドというものがあります。getterメソッドは、オブジェクトの持つプロパティに何らかの変化を加えたものを得るために利用します。

例えば、tonioさんの名字だけを返すgetterメソッドを定義してみましょう。

```javascript
class Chef {
  constructor(props) {
    this.experty = props.experty
    this.repertoire = props.repertoire || []
    this.name = props.name
  }
  get lastName() {
    return name.split(' ')[1];
  }
}
```

上記のように、getというキーワードをファンクションの前に付けることでgetterを定義できます。ここでは名前をスペースを基準にして2つの要素を持つArray(["トニオ", "トラサルディー"])にしています。その後2つ目の要素を返すことで名字を得ることができます。

実際にgetterメソッドを利用して見ましょう。getterメソッドを呼び出す際には()を使わないことに注意してください。

```javascript
let lastName = tonio.lastName
console.log(lastName) // => "トラサルディー"
```

## setterメソッド

setterメソッドはオブジェクトのプロパティをアップデートしたい際に利用します。例えば、chefにニックネームが出来た時にそれを追加するsetterメソッドを作ってみましょう。

```javascript
class Chef {
  constructor(props) {
    this.experty = props.experty;
    this.repertoire = props.repertoire || [];
    this.name = props.name;
    this.nickname = "";
  }
  set setNickname(nickname) {
    this.nickname = nickname;
  }
}

tonio.setNickname("トニー");
console.log(tonio.nickname); // => "トニー"
```

## クラスメソッド(staticメソッド)

ここまでは、インスタンス化したオブジェクトから呼び出すメソッドについて説明してきました。クラスメソッドは、インスタンスではなくクラスから直接呼び出すことが出来るメソッドです。クラス・メソッドを定義するにはファンクション定義の前にstaticというキーワードを加えます。ここでは、複数のシェフからレパートリーの数の平均値を返すメソッドを作ってみましょう。

```javascript
class Chef {
  constructor(props) {
    this.experty = props.experty;
    this.repertoire = props.repertoire || [];
    this.name = props.name;
    this.nickname = "";
  }
  static aveNumOfRepertoire(chefs = []) {
    if (chefs.size === 0) {
      return 0;
    } else {
      let sum = 0.0;
      for (chef of chefs) {
        sum += chef.repertoire.size;
      }
      return sum / chefs.size;
    }
  }
}
```

実際に使ってみます。

```javascript
const chef1 = new Chef({
  experty: "中華料理",
  name: "建一 陳",
  repertoire: ["麻婆豆腐", "ホイコーロー"]
});

const chef2 = new Chef({
  experty: "イタリア料理",
  name: "務 落合",
  repertoire: ["ボロネーゼ", "カルボナーラ", "ミネストローネ"]
});

const ave = Chef.aveNumOfRepertoire([chefi, chef2, tonio]);
console.log(ave); // => 3.0
```

このようにstaticメソッドはインスタンスに対して利用するユーティリティメソッドを作成するのに便利です。


```javascript
class Chef {
  constructor(props) {
    this.experty = props.experty;
    this.repertoire = props.repertoire || [];
    this.name = props.name;
    this.nickname = "";
  }
  static create(props) {
    return new Chef(props);
  }
}

const props = {
  experty: "中華料理",
  name: "建一 陳",
  repertoire: ["麻婆豆腐", "ホイコーロー"]
}

chef1 = Chef.create(props);
```

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

## チャレンジ

- [チャレンジ5](./challenge/README.md)

## 更に学ぼう

### 記事で学ぶ

- [クラス - Mozilla](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Classes)
