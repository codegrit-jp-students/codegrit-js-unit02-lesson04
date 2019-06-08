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
      // 本来は料理名に併せて_cookCapreseなどのメソッドを呼び出す。
      console.log(`${dishName}を作りました。`);
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

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/j7kquLfm/1/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

これで料理を作れるようになりました。例えば以下のようにして、トニオさんにカプレーゼを作ってもらうことが出来ます。

```javascript
tonio.cook("カプレーゼ"); // カプレーゼを作りました。
tonio.cook("肉じゃが"); // レパートリーにない料理です。
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
    return this.name.split(' ')[1];
  }
}
```

上記のように、getというキーワードをファンクションの前に付けることでgetterを定義できます。ここでは名前をスペースを基準にして2つの要素を持つArray(["トニオ", "トラサルディー"])にしています。その後2つ目の要素を返すことで名字を得ることができます。

実際にgetterメソッドを利用して見ましょう。getterメソッドを呼び出す際には()を使わないことに注意してください。

```javascript
let lastName = tonio.lastName
console.log(lastName) // => "トラサルディー"
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/ga925z70/2/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

## setterメソッド

setterメソッドはオブジェクトのプロパティをアップデートしたい際に利用します。例えば、chefにニックネームが出来た時にそれを追加するsetterメソッドを作ってみましょう。

```javascript
class Chef {
  constructor(props) {
    this._experty = props.experty;
    this._repertoire = props.repertoire || [];
    this._name = props.name;
    this._nickname = props.nickname || '';
  }
  set nickname(val) {
    this._nickname = val;
  }
  get nickname() {
    return this._nickname;
  }
}

tonio.nickname = 'トニー';
console.log(tonio.nickname); // => "トニー"
```

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/h4ejkxz6/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>
