## クラスメソッド(staticメソッド)

ここまでは、インスタンス化したオブジェクトから呼び出すメソッドについて説明してきました。クラスメソッドは、インスタンスではなくクラスから直接呼び出すことが出来るメソッドです。クラス・メソッドを定義するにはファンクション定義の前にstaticというキーワードを加えます。ここでは、複数のシェフからレパートリーの数の平均値を返すメソッドを作ってみましょう。

```javascript
class Chef {
  constructor(props) {
    this._experty = props.experty;
    this._repertoire = props.repertoire || [];
    this._name = props.name;
    this._nickname = "";
  }
  static aveNumOfRepertoire(chefs = []) {
    if (chefs.length === 0) {
      return 0;
    } else {
      let sum = 0.0;
      for (let chef of chefs) {
        sum += chef._repertoire.length;
      }
      return sum / chefs.length;
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

const ave = Chef.aveNumOfRepertoire([chef1, chef2, tonio]);
console.log(ave); // => 3.0
```

<iframe width="100%" height="600" src="//jsfiddle.net/codegrit_hiro/guy2tLom/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>

このようにstaticメソッドはインスタンスに対して利用するユーティリティメソッドを作成するのに便利です。


```javascript
class Chef {
  constructor(props) {
    this._experty = props.experty;
    this._repertoire = props.repertoire || [];
    this._name = props.name;
    this._nickname = "";
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

<iframe width="100%" height="300" src="//jsfiddle.net/codegrit_hiro/nucm50d3/3/embedded/js,result/dark/" allowfullscreen="allowfullscreen" allowpaymentrequest frameborder="0"></iframe>
