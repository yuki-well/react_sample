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
コンポーネントが再レンダーされるたびにアラートが表示されるようになる</<detail>