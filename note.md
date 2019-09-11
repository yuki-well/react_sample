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