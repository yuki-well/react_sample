<h1>Reactチュートリアル</h1>

<h2>"React.Component"について</h2>
まずはサブクラスについて

```
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}
```

ShoppingListはReactコンポーネントのクラスもしくは型。
コンポーネントはpropsを受け取ってrenderメソッドを通じて
ビューの階層構造を返してくる。
* props => propertiesの略、パラメータのこと

renderメソッドが返すのは、画面上に表示したいものの説明。</br>
renderは描画すべき記述形式であるReact要素を返す。</br>
Reactの開発者はだいたいJSX構文を使用しているらしい。

<div />構文はビルド時に React.createElement('div')に変換される
さっきのコードを置き換えるとこんな感じになる

```
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', Shopping List for {this.props.name}),
  React.createElement('ul', 
                      <li>Instagram</li>
                      <li>WhatsApp</li>
                      <li>Oculus</li>)
                    　);
```

JSXではJavaScriptの全ての能力(能力とは？)を使える。
どんなJavaScriptの式であろうと、JSX内で中括弧({}多分これ)に囲んで記入できる。
各React要素は、変数に格納したり、プログラム内で受け渡しできるJavaScriptのオブジェクト。

ShoppingListコンポーネントは<div />や<li />の組み込みのDOMコンポーネントのみをレンダーしているが
自分で書いたカスタムReactコンポーネントを組み合わせることも可能。
例えば<ShoppingList />と書いてショッピングリスト全体を表示できる

それぞれのReactコンポーネントはカプセル化されていて独立して動作する。
これによって単純なコンポーネントから複雑なUIを作成することが可能になっている。


<h2>スターターコードの中身を確認する</h2>

```
class Square extends React.Component {
  render() {
    return (
      <button className="square">
        {/* TODO */}
      </button>
    );
  }
}

class Board extends React.Component {
  renderSquare(i) {
    return <Square />;
  }

  render() {
    const status = 'Next player: X';

    return (
      <div>
        <div className="status">{status}</div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}

class Game extends React.Component {
  render() {
    return (
      <div className="game">
        <div className="game-board">
          <Board />
        </div>
        <div className="game-info">
          <div>{/* status */}</div>
          <ol>{/* TODO */}</ol>
        </div>
      </div>
    );
  }
}

// ========================================

ReactDOM.render(
  <Game />,
  document.getElementById('root')
);
```

このコードは作成しようとしているもの(三目並べ)のベースとなる。
CSSのスタイルはすでに含まれている
 => 'index.css'をimportしているため

このコードの中には3つのReactコンポーネントが存在する
* Square(正方形のマス目)
* Board (盤面
* Game

Square(マス目)コンポーネントは1つの<button>をレンダーし
Board(盤面)が9個のマス目をレンダーしている。
Gameコンポーネントは盤面とプレースホルダーを描画。
この時点ではインタラクティブなコンポーネントは存在しない

<h2>データをProps経由で渡す</h2>
はじめにBoardコンポーネントからSquareコンポーネントにデータを渡してみる

BoardのrenderSquareメソッド内でpropsとしてvalueという名の値をSquareに渡すようにコードを変更する.

次にSquareのrenderメソッドで渡された値を表示するように{/* TODO */}を
{this.props.value}に置き換える

<h3>インタラクティブなコンポーネントを作る</h3>
Squareコンポーネントがクリックされた時に"X"を表示させる。

まずSquareコンポーネントのrender()関数から返されてるボタンタグを変更

ここでSquareをクリックするブラウザ上でアラートが表示される

<detail><summary>**重要** アロー関数構文を使ってイベントハンドラを記述する</summary>

```
<button className="square" onClick={() => alert('click')}>
```

`onClick={() => alert('click')}`と表記した時にonClickプロパティに渡しているのは関数である。
Reactはクリックされるまでこの関数を実行しない。
`() =>`を書き忘れて`onClick={alert('click)}`と書くのはよくある間違いで
コンポーネントが再レンダーされるたびにアラートが表示されるようになる</detail>


<h2>state</h2>
Reactコンポーネントはコンストラクタで`this.state`を設定することで
状態を持つことができるようになる。
`this.state`はそれが定義されているコンポーネント内でプライベートとみなすべきである。
現在のSquareの状態を`this.state`に保存して、マス目がクリックされた時に変更するようにする

まずSquareクラスにコンストラクタを追加してstateを初期化する

<detail><summary>補足</summary>
JavaScriptのクラスでは、サブクラスのコンストラクタを定義する際は常に`super`を呼ぶ必要がある。
`constructor`を持つReactのクラスコンポーネントでは全てのコンストラクタを`super(props)`の呼び出しから始めるべき</detail>


次にSquareの`render`メソッドを書き換えて、クリックされた時にstateの現在値を表示するようにする

* `<button>`タグ内の`this.props.value`を`this.state.value`に置き換える
* `onClick={...}`というイベントハンドラを`onClick={() => this.setState({value: 'X'})}`に置き換える
* 読みやすくするため`className`と`onClick`の両プロパティをそれぞれ独立した行に配置する。

これらの書き換えでSquareのrenderメソッドから返される`<button>`タグが変わる

Squareの`render`メソッド内に書かれた`onClick`ハンドラ内で`this.setState`を呼び出すことで
Reactに`<button>`がクリックされたら常に再レンダーするようにできる。
データ更新のあと、このSquareの`this.state.value`は `'X'`になっているので
どのマス目をクリックしても盤面に`X`と表示されることになる

`setState`をコンポーネント内で呼び出すと、Reactはその内部の子コンポーネントも自動的に更新する。


<h1>ゲームを完成させる</h1>
<h2>Stateのリフトアップ</h2>
現時点ではそれぞれのSquareコンポーネントがゲームの状態を保持している。
誰が勝利したかをチェックするために9個のマス目の値を１箇所で管理できるようにしていく

Boardが各Squareに、現時点のstateがどうなっているか問い合わせればいいのではない。
Reactでは実装できるがコードが冗長化しやすい上に壊れやすくリファクタリングもしにくくなる。
そこでゲームの状態をSquareがわりに親のBoardコンポーネントで保持するようにする。
BoardコンポーネントはそれぞれのSquareにpropsを渡す事で何を表示するべきかが渡せる。

<detail>複数の子要素からデータを集めたい、もしくは2つの子コンポーネントで
互いにやり取りをさせたい場合は、親コンポーネント内で共有のstateを宣言する必要がある。
親コンポーネントはpropsを使う事で子に情報を返すことができるので、
子コンポーネントが兄弟、あるいは親との間で常に同期されるようになる</detail>


<h3>stateを親コンポーネントにリフトアップする</h3>
Boardにコンストラクタを追加して初期stateに9個のnullが9個のマス目に対応するように
9個のnull値をセットする。

Boardを書き換えてそれぞれの個別Squareに現在の値(`'X'`,`'O'`もしくは`'null'`)を表示させるようにする
`square`配列はBoardのコンストラクタで定義しているのでBoardの`renderSquare`が
そこから値を読み込むように書き換えていく

<h3>マス目がクリックされた時の挙動を変更していく</h3>
現在どのマス目に何が入っているか管理しているのはBoardで
SquareがBoardのstateを更新できるようにする必要がある。
stateはそれを定義しているコンポーネント内でプライベートなものなので
SquareからBoardのstateを直接書き換えることができない。

BoardからSquareに関数を渡すとして、
マス目がクリックされた時にSquareにその関数を読んでもらうようにする



