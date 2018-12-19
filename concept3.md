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