# TypeScript について

## TypeScriptに必要なもの
では、TypeScriptを使うにはどのような物が必要になるでしょうか？
何を準備すればいいのか簡単にまとめておきます。

1. Node.js(npm)
2. Visual Studio Code
3. TypeScript
4. Webpack
---
1. Node.js をインストールします。
ここではインストール方法については説明しません。
2. Visual Studio Code をインストールします。
ここではインストール方法については説明しません。
3. TypeScript をインストールします。
```bash
npm install typescript
```
インストールが完了すると以下のコマンドを入力してバージョンを確認します。
```bash
npx tsc --V
```
4. Webpack をインストールします。
Webpack とはTypescript をトランスコンパイルとかファイルをまとめたりする時に利用するプログラムで、プロジェクト開発時にインストールします。
プロジェクトフォルダを作成したら、以下のコマンドでインストールします。
```bash
npm install webpack ts-loader @webpack-cli/generators
```
利用時にはWebpack-CLIで初期化して利用します。
```bash
npx webpack-cli init
```
## TypeScript プロジェクトを作成してみる
では、実際にプロジェクトによる開発をしていきます。
プロジェクトフォルダー`typescript_app`を作成してフォルダー内に移動します。
```bash
mkdir typescript_app && cd typescript_app
```
### npm の設定ファイルを作成する
```bash
npm init -y
```
`package.json` と言うnode.js のパッケージ管理ファイルが作成され以下のような内容になってます。
```json
{
  "name": "typescript_app",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```
### TypeScript の設定を行う
続いて、TypeScript をプロジェクトに組み込みます。
```bash
npm install typescript @types/node --save-dev
```
TypeScript の設定ファイルを作成します。
```bash
npx tsc --init
```
`tsconfig.json` と言うTypeScript の設定ファイルが作成されます。
内容については割愛させてもらいますが、このファイルに設定を追記することでTypeScriptの挙動を変更することができます。
### Webpack の設定ファイル作成する
これでTypeScriptを利用したプロジェクトのベースは完成です。
ただし、現状では、TypeScript のトランスコンパイルや、必要なファイル類をまとめて実際に公開するアプリを生成したりする作業は自分で行わないといけないので、こうしたパッケージングの為に`Webpack`というソフトウェアをインストールします。
以下のコマンドをターミナルから実行してください。
```bash
npm install webpack ts-loader @webpack-cli/generators
```
#### Webpack-CLI で初期化する
以下のコマンドを実行して初期化を行います。
```bash
npx webpack-cli init
```
これを実行すると以下のような質問が表示されます。
```
? Which of the following JS solutions do you want to use? Typescript
```
Javascript 関連のソリューションに対応させる為の選択肢が表示されます。
今回は`Typescript`を選択しました。
```
? Do you want to use webpack-dev-server? Yes
```
Webpackの開発サーバーを追加するかの質問です。
今回は`Yes`で追加してます。
```
? Do you want to simplify the creation of HTML files for your bundle? Yes
```
簡略化したHTMLファイルを生成するか？の質問です。
今回は`Yes`で生成してます。
```
? Do you want to add PWA support? Yes
```
PWA(Progressive Web Apps) をサポートするかの質問です。
今回は`Yes`で対応しておきました。
```
? Which of the following CSS solutions do you want to use? none
```
CSS関連のソリューションの対応させる為の選択肢が表示されます。
今回は特に使わないので`none`を選んでいます。
```
? Do you like to install prettier to format generated configuration? Yes
```
設定ファイルを見やすくフォーマットするかの質問です。
今回は`Yes`を選択しました。
```
? Pick a package manager: npm
```
パッケージ管理ソリューションの選択肢が表示されます。
今回は`npm`を選択してます。
```
? Overwrite package.json? overwrite
```
パッケージ管理ファイル`package.json`の設定を上書きするか質問です。
今回は`Yes`を選択してます。
```
? Overwrite tsconfig.json? overwrite
```
TypeScript設定ファイル`tsconfig.json`の設定を上書きするか質問です。
今回は`Yes`を選択してます。

暫く待っていればプロジェクト初期設定が完了します。

#### Webpack-CLI で作成されたもの
プロジェクトでWebpack様に初期化されていますので、いくつかのファイルが生成されています。
| 生成ファイル      | 説明                                           |
| :---------------- | :--------------------------------------------- |
| src/index.ts      | スクリプトファイルがまとめられるフォルダです。 |
| README.md         | Github等でお馴染みのREADMEファイルです。       |
| index.html        | デフォルトで表示されるWebページです。          |
| webpack.config.js | Webpack の設定ファイルです。                   |

基本的に、`src`フォルダにTypeScriptファイルを保存して、トランスコンパイルされる様になります。
#### webpack.config.js について
生成されて`webpack.config.js`ファイルは以下の様になります。
```javascript
// Generated using webpack-cli https://github.com/webpack/webpack-cli

const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const WorkboxWebpackPlugin = require("workbox-webpack-plugin");

const isProduction = process.env.NODE_ENV == "production";

const config = {
  entry: "./src/index.ts",
  output: {
    path: path.resolve(__dirname, "dist"),
  },
  devServer: {
    open: true,
    host: "localhost",
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "index.html",
    }),

    // Add your plugins here
    // Learn more about plugins from https://webpack.js.org/configuration/plugins/
  ],
  module: {
    rules: [
      {
        test: /\.(ts|tsx)$/i,
        loader: "ts-loader",
        exclude: ["/node_modules/"],
      },
      {
        test: /\.(eot|svg|ttf|woff|woff2|png|jpg|gif)$/i,
        type: "asset",
      },

      // Add your rules for custom modules here
      // Learn more about loaders from https://webpack.js.org/loaders/
    ],
  },
  resolve: {
    extensions: [".tsx", ".ts", ".jsx", ".js", "..."],
  },
};

module.exports = () => {
  if (isProduction) {
    config.mode = "production";

    config.plugins.push(new WorkboxWebpackPlugin.GenerateSW());
  } else {
    config.mode = "development";
  }
  return config;
};
```
重要な項目だけ説明しておきます。

| 項目名    | 初期設定                                                       | 説明                                                                                                                      |
| :-------- | :------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------ |
| entry     | entry: "./src/index.ts",                                       | エントリファイル、このファイルが最初に使われるもの、デフォルトは`"./src/index.ts`になってます                             |
| output    | output: {path: path.resolve(__dirname, "dist"),},              | 出力先の設定、デフォルトは、プロジェクト内の`dist`フォルダになってます。                                                  |
| devServer | devServer: {open: true, host: "localhost"},                    | 開発サーバーの設定、デフォルトは`localhost`ドメインで自動的にファイルが開くようになってます。                             |
| plugins   | plugins: [new HtmlWebpackPlugin({template: "index.html",}),],  | プラグイン管理、`HtmlWebpackPlugin`と言うプラグインが用意されており、ここで起動するファイルに`index.html`が指定されている |
| resolve   | resolve: {extensions: [".tsx", ".ts", ".jsx", ".js", "..."],}, | Webpackで処理するスクリプトの対象、`extensions`で`tsx` `ts` `js` と言う拡張子のファイルを処理するように指定してます       |
この様に、Webpackの挙動は、webpack.config.js にて設定出来るようになってます。

### アプリケーションをビルドしてみる
では簡単なアプリケーションを作成してビルドしていきます。
まずは、`src/index.ts`ファイルを以下の様に作成します。
```typescript
window.addEventListener('load', (event) => {
	let p = document.querySelector('#target')
	p.textContent = 'This is message y TypeScript.'
})
```
次に、`index.html`ファイルを以下の様に作成します。
```html
<!DOCTYPE html>
<html lang="ja">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0/dist/css/bootstrap.min.css">
    <title>TypeScript</title>
</head>

<body>
    <h1 class="bg-primary text-white p-2">TypeScriptPage</h1>
    <div class="container py-2">
        <p class="h5" id="target">wait...</p>
    </div>
</body>

</html>
```
以下のコマンドを実行してビルドします。
```bash
npm run build
```
ビルドが完了すると、以下のファイルが生成されています。
`dist/index.html`
`dist/main.js`
これが、ビルドされたアプリケーションです。
webpack では、デフォルトで typescript ファイルをトランスコンパイルして `main.js` ファイルにスクリプトがまとめられます。
#### 開発サーバーを実行する
作成されたアプリケーションは、その場で動かして動作を確認する事が出来ます。
以下のコマンドを実行して、開発サーバーを起動してみましょう。
```bash
npm run serve
```
これで開発サーバーが起動して、Webブラウザでidex.htmlファイルが確認できます。
今回作成してアプリケーションは、p タグの文字列 **wait...** をjavascriptで **This is message y TypeScript.**　ページロード後置き換えてます。
Webブラウザで確認出来たら完成です。

## TypeScriptの基本
TypeScriptによる開発の基本がわかったので
基本的な文法を学んでいきます。

ここでは、短いサンプルコードを書きながら文法の基本を学習していきます。
しかし、前述のようなプロジェクトを作成して、ファイルを作成したと言った事を
毎回行わないとなると、めんどくさいので、本項目では「**TypeScript プレイグランド**」
当サービスを使って学習していきます。

TypeScript プレイグランドは、TypeScriptのソースコードを記述し、その場で実行出来るオンラインサービスです。
これは以下のＵＲＬにて公開されています。
https://www.typescriptlang.org/ja/play

### 基本の型とリテラル
では、TypeScriptの文法について説明しましょう。
最初に知っておきたいのは「型」についてです。

TypeScriptでは様々な型を使います。
型にはいくつかの種類(**型** あるいは **タイプ** とよびます)があります。

もっとも基本となる型は「**数値**」「**テキスト**」「**真偽値**」という３つです。
この他にも基本の値はいくつかあるのですが、それたは特殊な役割を果たすものであり、一般的な値として使われるのは、上の３つだと考えていいでしょう。

これらは「基本型(**PrimitiveType**)」と呼ばれます。

### 定数と変数
変数・定数は、値を一時的に保管して置くことができるメモリ上の領域です。
これらを使って、様々な値を保管して利用します。

#### 定数
定数は、値を保管する物ですが、最初に値をせていするとその後、二度と変更出来ません。
その性格から、特定の値に名前を付けて置くのに利用されます。
この定数は、以下の様な形で作成します。
```javascript
const 定数名 = 値
```
これで、指定した名前の定数が作られます。
移行、定数は不通の値のリテラルと同じように扱う事が出来ます。
#### 変数
変数も値を保管するものですが、定数と違い、後でいくらでも値を変更する事ができます。
この変数は以下の様な形で作成します。
```javascript
var 変数名
let 変数名
var 変数名 = 値
let 変数名 = 値
```
`var` または `let` の後に変数名を付けて宣言します。
変数は後で値を設定出来るため、最初に変数だけを作成し、後から値を設定する事も出来ます。
すでにある変数に値を設定する場合も、`=` イコール記号を使います。
```javascript
変数名 = 値
```
この様にすることで、変数に新しい値が設定されます。
変数や定数に値を設定する事を「代入」と言います。
##### var と let
変数を宣言する時に使う`var`と`let`は何が違うのか？
これは、変数が利用できる範囲の違いです。
`var`は、プログラム全体や、それが宣言された関数の中で利用出来るようになっていますが
`let`では宣言された構文の中でのみ使う事ができます。
この辺りは、構文や関数について理解してから改めて説明します。

### 型のアノテーションについて
変数を利用する時に、注意したいのは「**値の型**」です。
変数には「**型**」あります。そして、その型の値だけが代入出来るようになってます。
```javascript
let x = 123
```
これで変数`x`が作られ、そこに`123`と言う値が代入されます。
ここで重要なのは、同時に「変数`x`に`number`型が設定される」と言う点です。
最初に代入する値の型が自動的に割り当てられるのです。

もし、「この変数には、この方の値を使う」といった事をはっきりと指定したおきたいのならば、変数名の後にコロンを付けて、型名を指定して宣言をします。
```javascript
var 変数名: 型
let 変数名: 型
```
こうすうると、その変数は指定した型の変数のとして宣言されます。
もちろん、宣言と同時に値の代入も行うこともできます。

この「`: 型`」の部部は「型のアノテーション」と呼ばれます。
**型のアノテーション**はその変数への代入できる値に型の**制約**をかける事ができると言うことになります。
違う型の値を代入するとエラーが表示されます。

例外的に**any**型について、以下の様に変数宣言時に型を指定しない場合は、any 型が定義され、**どんな型でも問題ない変数**として値を扱う事ができますが、TypeScriptの値に関する便利な機能を利用しないのはもったいないので、よほどの理由がない限り、any型は乱用すべきではないです。
```javascript
let x
x = 123
console.log(x)
x = 'OK'
console.log(x)
```
#### 特殊な値
この他に、非常に特殊な役割を果たす値があります。
これらも、ここで紹介しておきますので覚えておきましょう。
- **null**
  これは「**値が存在しない**」状態を表します。nullと記述します。
- **undefined**
  これは「**値が用意されていない**」状態を表します。変数を宣言したが値が代入されていない、と言う様な状態です。
- **NaN**
  これは数値の演算ができない状態を表します。数値でない物を演算しようとした時などの結果として使われたりします。

### 型の変換について
プログラムを作成する上で、例えばテキストで入力された値を数値として計算する等と言った事は多いです。
こうした場合に必要になるのが「**型の変換**」です。
#### 型の変換方法
型の変換にはいくつかのやり方があります。
テキストを数値に変換するのはとても簡単です。
値の前に`+`プラス記号を付けるだけです。
```javascript
let x = 123
console.log(x)
let y = '456'
console.log(y)
x = +y
console.log(x)
```
この様にすると最後の変数`x`、数値型の 456 が `x`に代入されます。
#### 新し型の値を作る
もう一つの方法は、値を元に新しい値を作成する方法です。
- 数値の値を作る
  `Number( 値 )`
- テキストの値を作る
  `String( 値 )`

()の中に値を指定すると、その値を元に別の値を作る事ができます。
```javascript
let x = 123
console.log(x)
let y = '456'
console.log(y)
x = Number(y)
console.log(x)
```
この様にすると最後の変数`x`、`Number(y)` '456' を 456 に変換して `x`に代入されます。
### 演算を行うについて
値は、演算記号を使って演算(計算)する事ができます。
- 数値の演算
  `1 + 2` 結果 3
  `3 * 4` 結果 12
  `6 - 3` 結果 3
  `8 / 2` 結果 4
  `10 % 3` 結果 1
- テキストの演算
  `'abc' + 'xyz'` 結果 'abcxyz'
#### 計算をしてみる
実際に簡単な計算を行ってみます。
```javascript
let price = 12500
let withTax = price * 1.1
let woTax = price / 1.1
console.log("金額: " + price)
console.log("税別の場合、税込み価格: " + withTax)
console.log("税込みの場合、本体価格: " + woTax)
```
結果は
```javascript
"金額: 12500" 
"税別の場合、税込み価格: 13750.000000000002" 
"税込みの場合、本体価格: 11363.636363636362" 
```
実際に計算をしてみると端数が表示されます。
これは、コンピュータ特有の演算誤差になります。
コンピュータ内部では2進数で演算が行われますが、0.1は2進数で循環小数となる為正確な表現が出来ません。そのため誤差を含んでしまいます。
こうした誤差から無縁ではいられないのだということを理解しておきましょう。
